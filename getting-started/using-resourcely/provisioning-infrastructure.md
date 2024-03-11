---
description: Create and manage cloud infrastructure with Resourcely
---

# Provisioning Infrastructure

Now let's get started with using a Blueprint in order to provision infrastructure. We can see the blueprints we have available to us by clicking on the **Blueprints** tab in Resourcely's navigation menu.

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.28.57 PM (1).png" alt=""><figcaption><p>Select a Blueprint</p></figcaption></figure>

Click on the blueprint that you would like to use to learn more and then press the **use to create resource** button to get started.

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.29.39 PM.png" alt=""><figcaption><p>Examine Blueprint details</p></figcaption></figure>

In the next screen, we must fill out the required information such as:

* Repository Configuration
* Context
* General Settings

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.35.30 PM.png" alt=""><figcaption><p>Configure Resource - Repository</p></figcaption></figure>

{% hint style="info" %}
You can select which environment you will be deploying your infrastructure to (e.g. staging, prod, etc.) Environments are directories that contain different [Terraform input variables](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-variables). Press the **Configure environments** button to configure or create your own environments as needed
{% endhint %}

After filling out the required items scroll down to the **General Settings**.

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.36.52 PM.png" alt=""><figcaption><p>Configure Resource - Context</p></figcaption></figure>

Then we add a name to the resource. We can see that a guardrail is an in-effect that prevents buckets without **resourcely-** at the beginning of the name from being created. This guardrail can be removed by pressing the **lock** icon and then clicking the **Proceed** button.&#x20;

{% hint style="warning" %}
If a guardrail is ignored upon creation, the Resoucely CICD job will require a review by the appropriate reviewers before a Resource generation PR of that nature can be merged
{% endhint %}

After configuring your resource, press the **Continue** button to proceed

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.37.51 PM.png" alt=""><figcaption><p>Configure Resource - General Settings</p></figcaption></figure>

After doing so, press the **Continue** button to proceed

{% hint style="info" %}
If you wish to separate a resource definition into multiple files, you can highlight the section you wish to move and press the **Add selection to another destination** button.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.44.43 PM.png" alt=""><figcaption><p>Configure Pull Request</p></figcaption></figure>

Now we can review and confirm the Pull Request before we generate it. We must add a **Title,** and a **Description,** and verify the information is correct. Then we can proceed by pressing the **Open pull request to publish resource** button.

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.45.57 PM.png" alt=""><figcaption><p>Review and Conifrm a Resource Pull-Request</p></figcaption></figure>

Now you will see the Pull Request you created at the top of the screen.

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-01 at 1.50.25 PM.png" alt=""><figcaption></figcaption></figure>

You can view your Pull Request in the Source Code Management (SCM) you configured by pressing the **View PR** button. Resourcely will then validate your PR,

{% content-ref url="guardrails-in-action/github-actions.md" %}
[github-actions.md](guardrails-in-action/github-actions.md)
{% endcontent-ref %}

{% content-ref url="guardrails-in-action/gitlab-pipelines.md" %}
[gitlab-pipelines.md](guardrails-in-action/gitlab-pipelines.md)
{% endcontent-ref %}
