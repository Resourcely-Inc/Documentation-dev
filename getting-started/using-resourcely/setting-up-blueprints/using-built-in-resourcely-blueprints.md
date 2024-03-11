---
description: Create and manage secure-by-default cloud infrastructure templates
---

# Using Built-in Resourcely Blueprints

Resourcely allows you to create infrastructure from secure-by-default templates. You can see all blueprints available to you by clicking on the **Blueprints** tab in Resourcely's navigation menu. You can press the **Create a blueprint** button to begin the process of creating one.

{% hint style="warning" %}
Those with the Resourcely **Developer** role will only see the Blueprints that a Resourcely **Admin** role has configured
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.26.56 PM (1).png" alt=""><figcaption></figcaption></figure>

Resourcely provides a catalog with a wide set of available blueprints which can be further configured. In order to select a blueprint from our catalog you must choose the **Select a Resourcely Blueprint** option and then press the **Continue** button.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.27.01 PM.png" alt=""><figcaption><p>Choose from Blueprint Catalog</p></figcaption></figure>

You must then determine what type of resource you would like to create, highlight it, and then click on the **Continue** button. You can sort by **Provider**, **Category**, and **Keyword**, as well as **Search** for an available blueprint.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.27.16 PM.png" alt=""><figcaption><p>Built-in Blueprint Catalog</p></figcaption></figure>

In the next screen, you can remove guardrails. The appropriate guardrails enabled in your instances are added by default. After determining what guardrails you would like to maintain, press the **Continue** button.&#x20;

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.27.26 PM.png" alt=""><figcaption><p>Enable/Disable Guardrails</p></figcaption></figure>

Now you must configure the fields used to collect information from developers. To the right, you will see the fields inherited from your Global Context. These fields can be removed as needed.

To the left, you can see the fields configured by guardrails. You can press the **lock** icon in order to disable a particular guardrail.

{% hint style="warning" %}
Changes to guardrails will require approval from the guardrails owner(s)
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.27.59 PM.png" alt=""><figcaption><p>Configure fields and collect context</p></figcaption></figure>

You can also collect additional context from developers by adding a context question with custom fields. Once you have configured your required fields, press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.28.26 PM.png" alt=""><figcaption><p>Collecting Additional Context</p></figcaption></figure>

Now you can configure the metadata for the Blueprint which determines how it will look within the main Blueprint screen that developers can use to provision infrastructure. The added information should be as descriptive as possible. Then press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.28.35 PM.png" alt=""><figcaption><p>Provide blueprint metadata</p></figcaption></figure>

In the next section, you can review and confirm that the Blueprint you are creating meets your requirements. Press the **Publish Blueprint** button to add the blueprint to your namespace.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.28.48 PM.png" alt=""><figcaption><p>Review and confirm Blueprint</p></figcaption></figure>

After reviewing and confirming the blueprint, it will become available in the main Blueprint section that developers can select from.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.28.57 PM.png" alt=""><figcaption><p>Available Blueprints</p></figcaption></figure>

Once a blueprint is available you can use it to provision infrastructure using CI/CD. To learn more about provisioning infrastructure from a blueprint, see the guide below:

{% content-ref url="../provisioning-infrastructure.md" %}
[provisioning-infrastructure.md](../provisioning-infrastructure.md)
{% endcontent-ref %}
