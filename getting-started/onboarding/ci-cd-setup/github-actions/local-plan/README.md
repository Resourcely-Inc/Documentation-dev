---
description: CI/CD Automation
---

# 🐱 Local Plan

This integration requires that the Terraform plan file be available to GitHub Actions and visible to the Resourcely. You must perform the following steps:

* Adding Required Variables to the Repository
* Configure GitHub Actions as a Terraform Runner

## Adding Required Variables to the Repository

Resourcely can be configured using environment variables. Some variables are optional and used for configuration, while others must be defined before the guardrails can be validated.

| Key                    | Description                                                                                                                                                                | Secret |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `RESOURCELY_API_TOKEN` | **(Required)** Token generated from the Resourcely portal. Used to verify infrastructure guardrails.                                                                       | Yes    |
| `TF_PLAN_DIRECTORY`    | <p><strong>(Optional)</strong> The directory where the terraform files to verify are located. </p><p></p><p><strong>Default Value:</strong> <code>tf-plan-files</code></p> | No     |
| `TF_PLAN_PATTERN`      | <p><strong>(Optional)</strong> Pattern for Terraform plan files (e.g., <code>plan*</code>). </p><p></p><p><strong>Default Value:</strong> <code>plan*</code></p>           | No     |

Secret variables allow you to store sensitive information in your organization, repository, or repository environments.

1. Open the repository you want Resourcely to watch
2. Click the **Settings** tab
3. Under the **Security** section, select **Secrets and variables**
4. Under the **Secret** section, press the **New Repository secret** button
5. Add the following variables and their values one at a time and press the **Add secret** button

You can learn more about GitHub Secret variables by checking out the following documentation:

{% embed url="https://docs.github.com/en/actions/security-guides/encrypted-secrets" %}
GitHub Secret Variable Documentation
{% endembed %}

## Github Actions as Terraform Runner

Now let's add the Resourcely job to GitHub Actions in order to perform the following actions:

* Obtain the `resourcely-cli` Docker container, which is used to download policies from Resourcely, assess them, and submit the results to Resourcely. As a result, these findings will be displayed on your GitHub pull requests each time a new pull request is submitted.

1. Open the repository you want Resourcely to watch
2. Create a file named `resourcely.yml` under `.github/workflows`. If the directory does not exist, create it.
3. Copy and paste the following code and make configuration changes as needed

```yaml
name: Plan and Apply Terraform

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest
    environment: production

    defaults:
      run:
        shell: bash

    steps:
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

{% hint style="info" %}
**Note:** The[ Resourcely CI Action ](https://github.com/marketplace/actions/resourcely-change-management)is imported from the GitHub Actions Marketplace
{% endhint %}

4. Commit the change to the **main** branch

You should now have the Resourcely Action enabled in GitHub, which will run each time a new resource generation PR is created.

### Resourcely Github Actions Scaffolding

This [repository](https://github.com/Resourcely-Inc/scaffolding-github-actions) helps to integrate Resourcely into repository that used Github Actions as the Terraform runner.

It contains a [workflow](https://github.com/Resourcely-Inc/scaffolding-github-terraform-cloud/blob/main/.github/workflows/terraform.yml) that waits for terraform plan and then uses the [Resourcely Github Action](https://github.com/Resourcely-Inc/resourcely-action) to evaluate guardrails on that plan.

{% hint style="info" %}
**Note:** If you wish to use the GitHub Actions Scaffolding and plan to configure AWS credentials, we recommend the assume role approach with [OpenID Connect](aws-with-openid-connect.md).
{% endhint %}
