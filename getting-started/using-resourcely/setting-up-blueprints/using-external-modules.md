---
description: Customize and use custom Terraform Modules
---

# Using External Modules

If there is a specific module that you would like to use that Resourcely does not provide in its catalog, you can import one from an external source. You can see all blueprints available to you by clicking on the **Blueprints** tab in Resourcely's navigation menu. You can press the **Create a blueprint** button to begin the process of creating one.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.26.56 PM.png" alt=""><figcaption><p>Resourcely Available Blueprints </p></figcaption></figure>

Resourcely allows you to import an existing module and manually set up the shape of your new blueprint. In order to import a module you must choose the **Import a module from external source** option and then press the **Continue** button.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 2.36.40 PM.png" alt=""><figcaption><p>Import a module from external source</p></figcaption></figure>

Now we can select what source we want to import a module from. Resourcely allows module imports from:

* GitHub/GitLab Repository
* Terraform Registry

{% hint style="warning" %}
For **GitHub/GitLab Repository** import the **Repository URL**, **Module Path**, and **Authentication** are required.

\
For **Terraform Registry** import the **Source** (\<namespace>/\<module\_name>/\<provider>), and **Module Version** are required.
{% endhint %}

I am going to use the Terraform module as a source, import the [Google Service Accounts module](https://registry.terraform.io/modules/terraform-google-modules/service-accounts/google/latest?tab=inputs), and then press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.33.52 PM.png" alt=""><figcaption><p>Select a Source for Custom Module</p></figcaption></figure>

To create an accurate blueprint, Resourcely detects your module's inputs. In most cases, Resourcely can automatically infer types for each input. For cases where module fields cannot be auto-configured, you'll find these inputs under the **Manually Configured** tab.

{% hint style="warning" %}
When modifying types, you can either choose from the options in the dropdown, or you can enter the type yourself using [Terraform type constraints](https://developer.hashicorp.com/terraform/language/expressions/type-constraints).
{% endhint %}

Make sure you verify the auto-configured values, set various other metadata, and press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.34.10 PM.png" alt=""><figcaption><p>Configure Module Inputs</p></figcaption></figure>

Now we can Organize and group inputs to make it easier for developers to utilize blueprints. Press the **Continue** button to proceed.\


<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.34.57 PM.png" alt=""><figcaption><p>Organize and Group Inputs</p></figcaption></figure>

Now you can edit the Blueprint template details to better catalog your blueprint and make it easier to find. After editing the information for your blueprint, press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.36.09 PM.png" alt=""><figcaption><p>Blueprint template details</p></figcaption></figure>

You can now see your Module in the Blueprint templates catalog. Make sure to select the Blueprint we created and then press the **Continue** button so we can apply it to our workspace.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.36.28 PM.png" alt=""><figcaption><p>Select a Blueprint template</p></figcaption></figure>

In the next screen, you can remove guardrails. The appropriate guardrails enabled in your instances are added by default. After determining what guardrails you would like to maintain, press the **Continue** button.&#x20;

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.36.36 PM.png" alt=""><figcaption><p>Manage guardrails</p></figcaption></figure>

Now you must configure the fields used to collect information from developers. To the right, you will see the fields inherited from your Global Context. These fields can be removed as needed.

To the left, you can see the fields configured by guardrails. You can press the **lock** icon in order to disable a particular guardrail.

{% hint style="warning" %}
Changes to guardrails will require approval from the guardrails owner(s)
{% endhint %}

You can also collect additional context from developers by adding a context question with custom fields. Once you have configured your required fields, press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.36.48 PM.png" alt=""><figcaption><p>Configure fields and collect context</p></figcaption></figure>

Now you can configure the metadata for the Blueprint which determines how it will look within the main Blueprint screen that developers can use to provision infrastructure. The added information should be as descriptive as possible. Then press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.36.55 PM.png" alt=""><figcaption><p>Provide blueprint metadata</p></figcaption></figure>

In the next section, you can review and confirm that the Blueprint you are creating meets your requirements. Press the **Publish Blueprint** button to add the blueprint to your namespace.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.37.02 PM.png" alt=""><figcaption><p>Review and confirm</p></figcaption></figure>

After reviewing and confirming the blueprint, it will become available in the main Blueprint section that developers can select from.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-07 at 2.37.14 PM.png" alt=""><figcaption><p>Blueprints in namespace</p></figcaption></figure>

Once a blueprint is available you can use it to provision infrastructure using CICD. To learn more about provisioning infrastructure from a blueprint, see the guide below:

{% content-ref url="../provisioning-infrastructure.md" %}
[provisioning-infrastructure.md](../provisioning-infrastructure.md)
{% endcontent-ref %}
