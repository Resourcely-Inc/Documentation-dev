---
description: CI/CD Automation
---

# 🦊 GitLab Pipelines

GitLab Pipelines allows Resourcely to automatically evaluate your Terraform plans and policies on every pull request, and provide feedback directly within your merge-request. To set up Resourcely with GitHub Actions, you must perform the following steps:

* Adding Required Variables to the Repository
* Create a Resourcely Job

## Adding Required Variables to the Repository

Resourcely can be configured using environment variables. Some variables are optional and used for configuration, while others must be defined before the guardrails can be validated.

| Key                    | Description                                                                                                                                                                  | Secret |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `RESOURCELY_API_TOKEN` | **(Required)** Token generated from the Resourcely portal. Used to verify infrastructure guardrails.                                                                         | Yes    |
| `TF_DIRECTORY`         | <p><strong>(Optional)</strong> The directory where the terraform files to verify are located. </p><p></p><p><strong>Default Value:</strong> <code>$CI_PROJECT_DIR</code></p> | No     |

In GitLab, you can define non-secret variables within your `.gitlab-ci.yml` as follows:

```
variables:
  TF_DIRECTORY: "/root/terraform/plans"
```

Secret variables should be applied using GitLab UI CI/CD variables. You can use  UI CI/CD variables in this case to store values you don't want others to see by avoiding hard-coding them in your `.gitlab-ci.yml` file and limit who can access them.

{% hint style="info" %}
You must be a project member with the Maintainer role in order to create and manage  UI CI/CD variables.
{% endhint %}

You can add a UI CI/CD variable as follows:

1. Open the repository you want to Resourcely to watch
2. Click the **Settings** tab
3. Click on the **CI/CD**
4. Scroll down and expand the **Variables** section
5. Click on the **Add Variable** button
6. Add the following variables and their values one at a time and press the **Add variable** button

{% hint style="warning" %}
Make sure to select the **Mask variable** flag so that secret data is not exposed in the job output.&#x20;
{% endhint %}

You can learn more about GitLab CI/CD variables by checking out the following documentation:

{% embed url="https://docs.gitlab.com/ee/ci/variables/" %}
GitLab CICD variable documentation
{% endembed %}

## Creating the Resourcely Job

1. Open the GitLab project you wish to use.
2. Create a file named `.gitlab-ci.yml` your the project's root directory, note that it may already exist
3. Copy and paste the following code

```yaml
stages:
  - test

include:
  - remote: https://raw.githubusercontent.com/Resourcely-Inc/resourcely-gitLab-template/main/.resourcely.gitlab-ci.yml
```

{% hint style="info" %}
**Note:** The [Resourcely guardrail validation GitLab template](https://github.com/Resourcely-Inc/resourcely-gitLab-template) is imported directly from the Resourcely
{% endhint %}

4. Commit the change to the **main** branch

You should now have the Resourcely Job enabled in your GitLab pipeline, which will run each time a new resource generation Merge Request is created.&#x20;
