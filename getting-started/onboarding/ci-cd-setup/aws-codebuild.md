---
description: CI/CD Automation
---

# 🐟 AWS CodeBuild

You can integrate Resourcely with AWS CodeBuild to automatically evaluate your Terraform plans and policies on every pull request, and provide feedback directly within your pull/merge request. To set up Resourcely with AWS CodeBuild, you must perform the following steps:

* Verifying Prerequisites
* Storing the Resourcely API key
* Granting CodeBuild access to the Resourcely API Key
* Adding the Resourcely command

## Verify Prerequisites

This document assumes that you have a pre-existing AWS CodeBuild project configured. Before adding Resourcely to your build logic, please verify your build environment:

* Your worker instances have Internet egress (e.g. through a NAT Gateway).
* Your worker instances run in privileged mode.

{% hint style="info" %}
Privileged mode is required for a CodeBuild worker to use Docker. Please contact us if you can't run your worker using privileged mode.
{% endhint %}

## Storing the Resourcely API Token

The Resourcely CLI needs access to a Resourcely API key at build time so it can report findings in your Terraform plans. There are many ways to securely store secrets and make them available in AWS CodeBuild. This document uses AWS Secrets Manager as an example.

1. In the AWS Console, navigate to AWS Secrets Manager.
2. Click 'Store a new secret'.
3. Choose 'Other' for 'Secret type'.
4. Under 'Key/value pairs', choose any key. Later, this document will assume the key is `resourcely-api-key`.
5. Paste the API Token generated from the Resourcely portal as the value.

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

6. Click 'Next'.
7. Name the secret. Later, this document will assume the name of `resourcely-cli-secrets`.
8. Click 'Next' until you complete the wizard by clicking `Store`. Do not configure rotation.
9. Copy the ARN of your secret for use in the next step.

{% hint style="info" %}
See the [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) documentation for more information.
{% endhint %}

## Granting CodeBuild access to the Resourcely API Key

CodeBuild needs AWS IAM permissions to be able to read the Resourcely API Token. If you stored the token in AWS Secrets Manager, attach a policy like the one below to your CodeBuild Project's role. The role of ARN can be found in the project's Environment settings.

{% hint style="info" %}
If you do not use AWS Secrets Manager, the policy details will differ
{% endhint %}

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CodeBuildReadsResourcelyToken",
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "<the ARN of your secret>"
        }
    ]
}
```

{% hint style="info" %}
We recommend using a new policy rather than updating an existing policy. Adding a statement to an existing CodeBuild policy, _that is managed by CodeBuild_ will disable the automated management.
{% endhint %}

## Adding the Resourcely command

Now we can add a `resourcely-cli` command to your `buildspec.yml`. It evaluates your Terraform plan(s) by downloading policies from Resourcely, assessing them, and submitting the results to Resourcely. These findings will be displayed on the pull request associated with this build.

Here is an example `buildspec.yml` with commentary.

```
version: 0.2

env:
  variables:
    # Valid values: github | gitlab
    VCS: github
    
    # Resourcely needs to know the URL of the pull request associated with this
    # CodeBuild build. The source of this data will vary based on how CodeBuild
    # was integrated with your source control.
    #
    # For builds with no associated PR, a blank PR_URL will cause resourcely-cli
    # to do nothing (instead of failing due to an unknown PR).
    PR_URL: "TODO"
    
    # This directory should contain one or more Terraform plan json files when
    # the Resourcely command runs. The command at the bottom of this example
    # assumes this path is relative to the current working directory.
    PLANS_PATH: "TODO"
    
    RESOURCELY_API_HOST: https://api.resourcely.io
    
  secrets-manager:
  
    # If you stored the Resourcely API Key in AWS Secrets Manager, this will
    # make it available in CodeBuild. The string below is made up of the
    # secret name and the 'key' under which you stored the secret value.
    RESOURCELY_API_TOKEN: resourcely-cli-secrets:resourcely-api-key

# The phases below are just examples. You can run the resourcely-cli command
# in any phase, as long as the Terraform plan json is present by the time it
# runs.

phases:
  install:
    on-failure: ABORT
    commands:
      - docker pull ghcr.io/resourcely-inc/resourcely-cli:latest
  build:
    on-failure: ABORT
    commands:
      - docker run --rm \
        -e RESOURCELY_API_HOST=$RESOURCELY_API_HOST \
        -e RESOURCELY_API_TOKEN=$RESOURCELY_API_TOKEN \
        --mount type=bind,src=$(pwd)/$PLANS_PATH,dst=/plans \
        ghcr.io/resourcely-inc/resourcely-cli \
        evaluate \
        --vcs $VCS \
        --change_request_url ${PR_URL:-skip} \
        --change_request_sha $(git rev-parse HEAD) \
        $(ls -1 $PLANS_PATH/* | sed "s|^$PLANS_PATH|/plans|" | tr '\n' ',' | sed 's/,$//')
```

AWS CodeBuild should now run the Resourcely CLI on every build whenever PR\_URL is set.
