---
description: Support Terraform Integrations
---

# Terraform Integration

Resourcely works with Terraform in order to validate infrastructure guardrails and prevent misconfigurations. Terraform must run in your repository in order to create plans that Resourcely will validate. The following is required in order to get started with using Terraform:

* A Terraform solution to integrate into your version control system (GitHub/GitLab)
* Terraform provider definitions within the repository used for infrastructure provisioning

## Terraform Solutions (Version Control System Integration)

{% hint style="warning" %}
Resourcely has only been tested with Terraform Cloud. Other Terraform integrations may work but are not guaranteed.
{% endhint %}

A way of running Terraform on your repository and creating [Terraform plans](https://developer.hashicorp.com/terraform/tutorials/cli/plan) is required in order to get started. See below for Configuration options for different Terraform solutions:

{% content-ref url="terraform-cloud.md" %}
[terraform-cloud.md](terraform-cloud.md)
{% endcontent-ref %}

## Terraform Provider Definitions in Repository

Repositories that will leverage Resourcely require the Terraform providers to be defined and stored within the repository. This is required for a Terraform plan to be generated.

{% hint style="info" %}
The Terraform provider configuration is usually added to a folder named T**erraform** within the root of your repository.
{% endhint %}

See the Terraform documentation to see how to properly configure Terraform providers within your repository:

{% embed url="https://developer.hashicorp.com/terraform/language/providers/configuration" %}
Terraform Provider Configuration
{% endembed %}

Once all the prerequisites have been met, you can continue to [onboarding](../../onboarding/ "mention").
