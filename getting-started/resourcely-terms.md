---
description: Glossary of Resourcely Key Concepts
---

# 💡 Resourcely Key Concepts

{% hint style="info" %}
**Note:** This glossary is meant to serve as a guide to help Resourcely users understand key concepts related to Resourcely.
{% endhint %}

## Cloud Resources

Resourcely provides the following resources to assist in securing your cloud infrastructure:

* [#blueprints](resourcely-terms.md#blueprints "mention"): Configurable templates used to provision cloud infrastructure resources.
* [#guardrails](resourcely-terms.md#guardrails "mention"): Rules that determine how cloud resources can be created and altered.
* [#global-contexts](resourcely-terms.md#global-contexts "mention"): Context-prompting questionnaires used to gather data from developers before provisioning a resource.
* [#global-values](resourcely-terms.md#global-values "mention"):  Allow admins to define custom drop-downs for customizing terraform infrastructure resource properties before they are provisioned.
* [#pull-requests-prs](resourcely-terms.md#pull-requests-prs "mention"): Manage requests to commit a change from one Git branch to another.&#x20;
* [#environments](resourcely-terms.md#environments "mention"): Separate directory (with separate Terraform variables) within your repository used to define a deployment environment.
* [#foundry](resourcely-terms.md#foundry "mention"): Create custom resources (Blueprints, Guardrails, etc.) by cloning and configuring existing resources or starting from scratch.

### **Blueprints**

Blueprints are configurable templates used to provision cloud infrastructure resources. Blueprints allow you to:

* Define which options are available for properties of your resource(s).
* Apply guardrails to your resource(s) to prevent misconfiguration.
* Define what information to collect from your developers before resource provision.

Once a blueprint has been configured for use it becomes available in the Resourcely service catalog. Resourcely provides 2 different types of blueprints, **Resourcely Blueprints**, and **External Modules**.

#### **Resourcely Blueprints**

{% hint style="info" %}
**Resourcely Blueprints** are the recommended way to create a guided secure-by-default cloud infrastructure resource template.
{% endhint %}

Resourcely blueprints are built-in cloud infrastructure resource templates that are secure and compliant by design.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-23 at 1.26.56 PM.png" alt=""><figcaption><p>Resourcely Blueprint Catalog</p></figcaption></figure>

These blueprints allow you to create a variety of different types of resources with the click of a button. The blueprints can be sorted by the following criteria:

* **Provider -** The cloud provider (Amazon, Azure, Google, etc.)
* **Category -** The type of resource (VM, Storage Bucket, VPC, etc.)
* **Keyword -** A word that relates to a resource (ec2, database, firewall, etc.)&#x20;

#### **External Modules**

If you don't see the resource you need in Resourcely's built-in blueprints, you can supply an external module. External modules allow you to start from an existing Terraform module and manually set up the shape of your new blueprint.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-23 at 1.31.27 PM.png" alt=""><figcaption><p>External Module Import</p></figcaption></figure>

You can import an external module from:

* a GitHub or GitLab repository
* the [Terraform registry](https://registry.terraform.io/)

{% hint style="info" %}
**Note:** Authentication is required for private repositories.
{% endhint %}

### **Guardrails**

Guardrails govern how cloud resources can be created and altered, preventing infrastructure misconfiguration. Guardrails are applied to Blueprints so that they can be verified before resource provisioning.

{% hint style="info" %}
**Note:** You can define Guardrail override behavior in case exceptions are required.
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot 2023-08-23 at 1.26.44 PM.png" alt=""><figcaption><p>Resourcely Guardrail Catalog</p></figcaption></figure>

Before infrastructure is provisioned, Resourcely examines the changes being made and prevents a merge if any guardrail requirements are violated. Some examples of guardrails include:

* Approval for making S3 public
* Allowed compute image types
* GCP Allowed Regions
* Allowed compute instance types

Resourcely provides a catalog with a wide set of available guardrails which can be further configured. Guardrails are available for the following categories:

* Access Control
* Best Practices
* Cost Efficiency

### **Global Contexts**

Global Contexts are context-prompting questionnaires used to gather data from developers before provisioning a resource. Global Contexts are designed to gather and store insightful data related to the resource that will be generated. Some examples include:

* What type of data will be stored in this infrastructure?
* In which environment(s) will this infrastructure be deployed?
* What application is this infrastructure associated with?
* What is the email address of the person/team responsible for this infrastructure?

<figure><img src="../.gitbook/assets/Screenshot 2023-08-23 at 1.27.16 PM.png" alt=""><figcaption><p>Global Context Catalog</p></figcaption></figure>

You can create your own Global Contexts and apply them to your Blueprint(s) so that they must be filled out before infrastructure is provisioned. Global Contexts can be defined with the following properties:

* Single Choice
* Multiple Choice
* Text Field

### Global Values

Global Values allow admins to define custom drop-downs for customizing terraform infrastructure resource properties before they are provisioned.

<figure><img src="../.gitbook/assets/Screenshot 2023-11-22 at 12.19.27 PM (1).png" alt=""><figcaption><p>Collections of Global Values</p></figcaption></figure>

You can create and update various collections of Global Values and apply them to your Blueprint(s).&#x20;

{% hint style="info" %}
After publishing a collection, the global values will become read-only to avoid breaking existing references to Blueprints.&#x20;
{% endhint %}

Global Values Collections contain the following properties:

* **Key:** Unique key used to reference the collection
* **Type:** The type of data the collection will store. The following data types can be used:
  * Text
  * Number
  * List
  * Object
* **Name:** Name of the collection
* **Description:** Description of the collection

Once a collection has been created, you can add values with the following properties:

* **Key:** Unique key used to reference the option within the collection.
* **Label:** Label shown to developers in the dropdown field of the pull-request form.
* **Value:** Value to be applied to the generated Terraform.
* **Description:** Description of the option.

Once Global Values have been created, they can be applied when creating a resource provision request from a Blueprint.

### **Pull Requests (PRs)**

Pull Requests (PRs) are requests to commit a change from one Git branch to another. They allow you to keep track of changes, configure reviewers, set up acceptance criteria for merging, and much more.&#x20;

{% hint style="info" %}
**Note:** In GitLab terminology, a Pull Request (PR) is called a Merge Request (MR). Resourcely uses the term **"Pull Request (PR)"** for both GitLab and GitHub.
{% endhint %}

You can create and manage infrastructure PRs directly from Resourcely. Resourcely tracks the following data related to the generated PR:

* Pull Request Title
* Status
* Blueprint
* Provider
* Last Updated On
* Created By

<figure><img src="../.gitbook/assets/Screenshot 2023-08-23 at 1.27.12 PM.png" alt=""><figcaption><p>Pull Request Management Screen</p></figcaption></figure>

When a new PR is generated, Resourcely examines the changes being made and prevents the PR from being merged if it violates any of the guardrail requirements.

### Environments

In Resourcely an **Environment** is a separate directory (with separate Terraform variables) within your repository used to define a deployment environment. Before creating a pull request, Resourcely allows you to select which environment you will be deploying to.&#x20;

{% hint style="warning" %}
You must define a CI/CD configuration to deploy Terraform to different environments based on directory structure.
{% endhint %}

While you can create your own environments, resourcely provides the following by default:

* prod
* test
* staging

## Foundry

Foundry allows you to create custom blueprints, guardrails, etc. instead of using the ones provided in the Resourcely catalog. Additionally, you can clone and customize existing resources.

<figure><img src="../.gitbook/assets/Screenshot 2024-02-03 at 11.50.42 AM.png" alt=""><figcaption><p>Resourcely Foundry Screen</p></figcaption></figure>

When creating a custom resource using Foundry, you are provided with step-by-step instructions and input validation to ensure your resource is valid.

{% hint style="info" %}
Currently, you can only create custom blueprints. Custom guardrail creation and experimental features are coming soon.
{% endhint %}

Foundry provides the following views for the resource that will be created:

* **Settings:** Allows you to select an existing resource (S3 bucket, Compute instance, etc.) which can then be configured using Resourcely DSL.&#x20;
* **Developer Experience:** Provides a preview of the fields that your developers will see and interact with as they create new resources.
* **Terraform:** Provides a simplified view of the PR that will be created when a developer uses this blueprint. You can adjust inputs under "Developer Experience" to see how the output changes.

### Creating a Blueprint

Using foundry, you can create your blueprint by cloning and customizing an existing blueprint or starting from scratch. For custom blueprint creation, you will be provided with the following sections containing step-by-step instructions and validation:

* **Configure Context:** Select an existing blueprint to clone and edit or write your own using Resourcely DSL.
* **Customize Guardrails:** Include or exclude applicable guardrails to ensure compliance. (Resourcely DSL configuration support coming soon)
* **Define Metadata:** Provide metadata to improve the discoverability of your blueprint.
* **Attach Context:** Specify context questions to allow the collection of relevant information in pull requests.

{% hint style="info" %}
Settings, Developer Experience, and Terraform views are available for all the Blueprint sections listed above.
{% endhint %}

***

## Organizational Hierarchy

The Resourcely organizational hiearchy is compromized of **Company > Organization(s) > Group(s) > Reviewer(s)**

### Company

A Company is used to identify your tenant install of Resourcely and may show up for logged-in users within the company in custom modules. A Company is comprised of a variety of Organizations.

### Organization

An Organization is used to define different parts of your company, for example, Development and Security can be 2 different organizations within the same company. Each Organization is composed of a variety of Groups.

### **Group**

A Group is a set of stakeholders for guardrails that can be looped in during changes. Groups contain the following fields:

* Name
* Contact Info (Email)
* Organization it belongs to
* List of Reviewers

### Reviewer

A reviewer is a person who will be set to review a Pull Request (PR). Resourcely automatically assigns a reviewer defined in a group to review an infrastructure resource before it is provisioned.&#x20;

### Team

Each organization can consist of various teams, relevant for functions like RBAC (Role-Based Access Control) or determining notification recipients. For simplicity, your team name can initially mirror the group name. Choose any format that suits your organization, such as "Product", "Sales", etc.
