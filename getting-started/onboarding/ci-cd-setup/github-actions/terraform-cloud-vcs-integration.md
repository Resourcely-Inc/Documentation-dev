---
description: CI/CD Automation
---

# 🐶 Terraform Cloud VCS Integration

This integration requires Terraform Cloud VCS to be integrated with the GitHub projects you will be using with Resourcely. The first step is to configure environment variables. To set up Resourcely with GitHub Actions using Terraform Cloud VCS, you must perform the following steps:

* Creating a Personal Access Token
* Adding Required Variables to the Repository
* Creating the Resourcely GitHub Action

## Creating a Personal Access Token (Classic)

Personal access tokens are an alternative to using passwords for authentication to GitHub. Resourcely uses personal access tokens to access GitHub resources on your behalf using the API.&#x20;

{% hint style="info" %}
Resourcely requires a **Classic** Personal Access Token with the **repo** scope.
{% endhint %}

1. [Verify your email address](https://docs.github.com/en/get-started/signing-up-for-github/verifying-your-email-address), if it hasn't been verified yet.
2. In the upper-right corner of any page, click your profile photo, then click **Settings**.
3. In the left sidebar, click **Developer settings**.
4. In the left sidebar, under **Personal access tokens**, click **Tokens (classic)**.
5. Select **Generate new token**, then click **Generate new token (classic)**.
6. In the "Note" field, give your token a descriptive name.
7. To give your token an expiration, select **Expiration**, then choose a default option or click **Custom** to enter a date.
8. Select the **repo** scope
9. Click **Generate token**.

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-09-01 at 1.56.15 PM.png" alt=""><figcaption><p>Personal Access Token Generation</p></figcaption></figure>

Once you have generated a personal access token, store it in a safe place, it will later be added to your GitHub repository as a secret. You can learn more about Personal Access Tokens by checking out the following documentation:

{% embed url="https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic" %}
Personal Access Token documentation
{% endembed %}

## Adding Required Variables to the Repository

Resourcely can be configured using environment variables. Some variables are optional and used for configuration, while others must be defined before the guardrails can be validated.

| Key                    | Description                                                                                                                                                                                                                                                | Secret |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `RESOURCELY_API_TOKEN` | **(Required)** Token generated from the Resourcely portal. Used to verify infrastructure guardrails.                                                                                                                                                       | Yes    |
| `GH_ACCESS_TOKEN`      | **(Required)** Your GitHub personal access token (classic) with `repo` scope only.                                                                                                                                                                         | Yes    |
| `TF_DIRECTORY`         | <p><strong>(Optional)</strong> The directory where the terraform files to verify are located. </p><p></p><p><strong>Default Value:</strong> <code>tf-plan-files</code></p>                                                                                 | No     |
| `TF_API_TOKEN`         | <p><strong>(Optional)</strong> Your Terraform Cloud <code>team</code> token with workspace permission set to <code>Manage all workspaces</code>.</p><p></p><p>This is required only if you are using Terraform Cloud for infrastrucuture provisioning.</p> | Yes    |

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

## Creating the Resourcely Action

Now let's add the Resourcely job to GitHub Actions to perform the following actions:

* Obtain the `resourcely-cli` Docker container, which is used to download policies from Resourcely, assess them, and submit the results to Resourcely. As a result, these findings will be displayed on your GitHub pull requests each time a new pull request is submitted.

1. Open the repository you want to Resourcely to watch
2. Create a file named `resourcely.yml` under `.github/workflows`. If the directory does not exist, create it.
3. Copy and paste the following code and make configuration changes as needed

```yaml
name: Resourcely CI

# Trigger conditions for running this action
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# Permissions for the action
permissions:
  contents: read  # Read repository content
  packages: read  # Read packages from the repository

# Define jobs to be run
jobs:
  # Name of the job
  resourcely-ci:
    runs-on: ubuntu-latest  # The type of machine to run the job on
    steps:
      - uses: Resourcely-Inc/resourcely-action@v1 # import the action
        with:
          # grab the GitHub access token stored in the repo secrets
          gh_access_token: ${{ secrets.GH_ACCESS_TOKEN }}
          # grab the terraform api token stored in the repo secrets
          tf_api_token: ${{ secrets.TF_API_TOKEN }}
          # grab the resourcely api token stored in the repo secrets
          resourcely_api_token: ${{ secrets.RESOURCELY_API_TOKEN }}
          # set the resourcely api host
          resourcely_api_host: "https://api.resourcely.io"
```

{% hint style="info" %}
**Note:** The[ Resourcely CI Action ](https://github.com/marketplace/actions/resourcely-change-management)is imported from the GitHub Actions Marketplace
{% endhint %}

4. Commit the change to the **main** branch

{% hint style="warning" %}
Please note that this setup assumes your GitHub is integrated with Terraform Cloud. If you are using other runners, you will need to modify your script accordingly.
{% endhint %}

You should now have the Resourcely Action enabled in GitHub, which will run each time a new resource generation PR is created.

### Terraform Cloud VCS Integration Considerations

If you are using Terraform Cloud to provision your infrastructure resources and have performed the following actions:

* Linked your GitHub repository with Terraform Cloud using VCS
* Set the `TF_API_TOKEN` variable within GitHub

Then the applied Resourcely action will run the `wait-for-terraform-plan` job which performs the following before verifying the configured guardrails:

* Searches for the Terraform Cloud job run in the Resourcely generated pull-request
* Continuously checks the status of the Terraform Cloud Job until completion
* Downloads the Terraform plans once the Terraform Cloud Job has been completed successfully

Once the plan has been downloaded, Resourcely will validate the plans against the implemented guardrails.

### Resourcely Terraform Cloud Scaffolding

This [repository](https://github.com/Resourcely-Inc/scaffolding-github-terraform-cloud) helps to integrate Resourcely into repository that uses Terraform Cloud as the Terraform runner.

It contains a [workflow](https://github.com/Resourcely-Inc/scaffolding-github-terraform-cloud/blob/main/.github/workflows/terraform.yml) that waits for terraform plan and then uses the [Resourcely Github Action](https://github.com/Resourcely-Inc/resourcely-action) to evaluate guardrails on that plan.
