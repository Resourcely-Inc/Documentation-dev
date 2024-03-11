---
description: >-
  This guide demonstrates how to configure AWS with OpenID Connect role, create
  permissions, a GitHub Actions workflow, and deploy Terraform code to AWS.
---

# 🐹 AWS with OpenID Connect

OpenID Connect serves as an identity layer built on the OAuth 2.0 protocol, allowing third-party applications to verify the identity of end-users or, in this context, an AWS role.

## AWS role

In AWS, you need to create an identity provider for GitHub and create a role for web identity or OIDC, associating it with the created identity provider.

### Creating an Identity Provider for GitHub in AWS

Go to the IAM section -> select `Identity Providers`, and then create a new one. Choose `OpenID Connect` as the type and enter the following details:&#x20;

* Provider URL: `https://token.actions.githubusercontent.com`
* Audience: `sts.amazonaws.com`

in Provider URL > click on `Get thumbprint`.

### Configuring the AWS Role

Next, create a role by navigating to IAM -> Roles and opting for a new role creation. Choose "Web Identity" for the role type and select the previously created GitHub identity provider. Assign the role the desired permissions. Give the role a name e.g `github_action_role` .

After the role's creation, add the GitHub repository into the role's trust relationships. This integration is done by modifying the trust relationship's JSON configuration to resemble the following, where the GitHub repository and conditions for its access are specified: Go to Roles > click on the role name `github_action_role` -> Click on add permissions > Create inline policy -> Select JSON and past the below content. Make sure to replace `StringLike` with your repo information.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::12345678:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:Resourcely-Inc/scaffolding-github-actions:*"
                }
            }
        }
    ]
}

```

This setup specifies which GitHub repositories are permitted to assume this AWS role, using a wildcard to include all branches of the example repository.

### Setting Up GitHub Actions

To deploy Terraform code to AWS via GitHub Actions, you need to:

1. **Assign Necessary Permissions:** The job must have permissions to request the JWT and read the contents for actions like checkout. This is specified in the YAML configuration as:

```
permissions:
  id-token: write
  contents: read
```

2. **Configure AWS Credentials in GitHub Actions:** Add the `aws-actions/configure-aws-credentials` action to your GitHub job, specifying the role to assume and the AWS region.
3. **Incorporate Terraform Operations:** Include steps for setting up Terraform, initializing, formatting, and deploying the Terraform code within the GitHub Action workflow.

The complete workflow for deploying Terraform code to AWS using OpenID Connect is detailed in a YAML configuration below, including steps for AWS credential configuration, Terraform setup, deployment commands, and Resourcely CLI setup structured to trigger on specific GitHub events.

```
name: Plan and Apply Terraform

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  id-token: write
  contents: read

jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest
    environment: production

    defaults:
      run:
        shell: bash

    steps:
      - name: Configure aws credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: arn:aws:iam::12345678:role/github_action_role
          aws-region: us-west-2

      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan
        run: terraform plan -out=plan.raw

      - name: Convert the plan to JSON
        id: planToJson
        run: terraform show -json plan.raw

      - name: Save JSON to a file
        uses: fishcharlie/CmdToFile@v1.0.0
        with:
          data: ${{ steps.planToJson.outputs.stdout }}
          output: plan.json

      - name: Upload Terraform Plan Output
        uses: actions/upload-artifact@v2
        with:
          name: plan-file
          path: plan.json

      - name: Terraform Apply
        if: github.ref == 'refs/heads/main' && github.event_name == 'push'
        run: terraform apply -auto-approve -input=false

  resourcely-ci:
    needs: terraform
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Download Terraform Plan Output
        uses: actions/download-artifact@v2
        with:
          name: plan-file
          path: tf-plan-files/

      - name: Resourcely CI
        uses: Resourcely-Inc/resourcely-action@v1
        with:
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          resourcely_api_host: "https://api.resourcely.io"
          tf_plan_directory: "tf-plan-files"
```
