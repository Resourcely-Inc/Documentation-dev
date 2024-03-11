---
description: Setting up Terraform Cloud VCS Integration
---

# Terraform Cloud VCS Integration

[Terraform Cloud](https://cloud.hashicorp.com/products/terraform) allows you to manage [Terraform](https://www.terraform.io/) runs in a consistent and reliable environment instead of on your local machine.

{% hint style="info" %}
Resourcely supports all Terraform Cloud pricing tiers. For more information see the [Terraform Pricing page](https://www.hashicorp.com/products/terraform/pricing?ajs\_aid=05e2ac47-57f9-4ae6-ad20-e3de38d12c06\&product\_intent=terraform).
{% endhint %}

Resourcely leverages Terraform Cloud in order to:

* Use Terraform in a consistent and reliable environment
* Easily access shared state and secret data
* Run Terraform within a repository using CI/CD

If you don't currently have a Terraform Cloud account, you can set one up using the following tutorial:

{% embed url="https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-sign-up" %}
Introduction to Terraform Cloud
{% endembed %}

Once you have access to Terraform Cloud you must configure as stated below.

## Configuring Terraform Cloud for Resourcely

Once you have a Terraform Cloud account you must configure it for use with Resourcely by performing the following:

* Adding External Cloud Provider Credentials
* Creating a new Workspace
* Generating a Team Token

### Adding External Cloud Provider Credentials

In order to provision resources using Resourcely, you must have permission to actually generate cloud resources using Terraform. This requires access to a cloud provider such as Amazon Web Services (AWS), Google Cloud Platform (GCP), or Microsoft Azure.

You can create a credential variable set by going through the following guide:

{% embed url="https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-create-variable-set" %}
Tutorial on adding Cloud Provider Credentials
{% endembed %}

### Creating a new Workspace (Version Control Integration)

Workspaces determine how Terraform Cloud organizes infrastructure. A workspace contains your Terraform configuration (infrastructure as code), shared variable values, your current and historical Terraform state, and run logs.

You must create a [Version control workflow](https://developer.hashicorp.com/terraform/cloud-docs/run/ui) that stores your Terraform configuration in your git repos and triggers run based on pull requests and merges. See the following documentation to learn more:

{% embed url="https://developer.hashicorp.com/terraform/cloud-docs/workspaces/creating" %}
Creating Workspaces Documentation
{% endembed %}

Adding the **Version control workflow** will enable Terraform plans to run whenever a code change is made within a branch. Resourcely will use that plan in order to validate guardrails before the infrastructure can be provisioned.

### Generating Team API Token

Team API tokens are used by services, for example, a CI/CD pipeline, to perform plans and apply them on a workspace. Resourcely uses the token in order to verify the Terraform plan. See the following documentation to learn how to generate a Team API Token:

{% embed url="https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens#team-api-tokens" %}
API Token Documentation
{% endembed %}

{% hint style="warning" %}
An organization token is not valid and cannot be used to access a terraform plan which will cause a plan detection error when using the Resourcely CI/CD Job.
{% endhint %}

Be sure to **save your token in a safe place**, you will need to refer to it when setting up the Resourcely CI/CD job.

{% hint style="info" %}
The team token you generate should have access to the workspace you will be using with Resourcely.
{% endhint %}
