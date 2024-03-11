---
description: Create and customize cloud infrastructure templates step-by-step
---

# Custom Blueprints with Foundry

Resourcely allows you to create and customize infrastructure by following an intuitive step-by-step menu. You can begin to create a blueprint by clicking on the **Foundry** tab in Resourcely's navigation menu. [Foundry](../../resourcely-terms.md#foundry) allows you to define the following parts of a blueprint:

* [Configure Content](custom-blueprints-with-foundry.md#configure-content)
* [Customize Guardrails](custom-blueprints-with-foundry.md#customize-guardrail)
* [Define Metadata](custom-blueprints-with-foundry.md#define-metadata)
* [Attach Context](custom-blueprints-with-foundry.md#attach-context)

Once these sections have been defined, you can proceed to [Preview and Create your Blueprint](custom-blueprints-with-foundry.md#preview-and-create-blueprint).

{% hint style="info" %}
Foundry is currently in beta and only supports authoring blueprints. Support for guardrails is coming soon.
{% endhint %}

## Configure Content

This section of the **Create a Blueprint Foundry** allows you to create your blueprint by either cloning an existing blueprint or by starting from scratch.

<figure><img src="../../../.gitbook/assets/1.png" alt=""><figcaption><p>Create a Blueprint Foundry View</p></figcaption></figure>

To clone a [built-in Resourcely blueprint](../../resourcely-terms.md#resourcely-blueprints), simply press the **Select a Blueprint starter** drop-down and select a Blueprint. The dropdown provides a list of all the available secure-by-default templates provided by Resourcely.&#x20;

{% hint style="info" %}
To create a Blueprint from scratch simply define the blueprint in the edit box by following the[ Authoring Your Own Blueprints ](authoring-your-own-blueprints.md)documentation
{% endhint %}

<figure><img src="../../../.gitbook/assets/2.png" alt=""><figcaption><p>Selecting a Resourcely Blueprint </p></figcaption></figure>

Once you select a starter Blueprint you can then select additional aspects of the Blueprint to clone, such as _Guardrail Configurations_, _Metadata_, _etc_. Press the **Clone Blueprint** button and you will see that the edit box has been populated.&#x20;

<figure><img src="../../../.gitbook/assets/3.png" alt=""><figcaption><p>Cloing a starter Blueprint</p></figcaption></figure>

You can further customize this starter Blueprint programmatically using the provided edit box by following the instructions in the [Authoring Your Own Blueprints ](authoring-your-own-blueprints.md)documentation

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 3.08.23 PM.png" alt=""><figcaption><p>Configuring Blueprint via edit box</p></figcaption></figure>

Then you can proceed to Customize the Blueprint's Guardrails by clicking on the **Customize Guardrails** tab.

## Customize Guardrails

This section of the **Create a Blueprint Foundry** allows you to include or exclude applicable guardrails to ensure compliance.

{% hint style="info" %}
Guardrails are inherently global and should be excluded only in rare circumstances, as doing so could increase the risk of misconfigurations.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 12.35.26 PM.png" alt=""><figcaption><p>Customize Applicable Guardrails</p></figcaption></figure>

You can include/exclude guardrails from a blueprint by selecting/unselecting the **Applicable Guardrails** radio buttons. Then you can proceed to Define the Blueprint's Metadata by clicking on the **Define Metadata** tab.

## Define Metadata

This section of the **Create a Blueprint Foundry** allows you to provide metadata to improve the discoverability of your blueprint.

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 12.36.33 PM.png" alt=""><figcaption><p>Defining Blueprint Metadata</p></figcaption></figure>

Make sure to populate all the required fields before proceeding

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 2.33.02 PM.png" alt=""><figcaption><p>Defining Blueprint Metadata - Filled</p></figcaption></figure>

Then you can proceed to Attaching Context to the Blueprint by clicking on the **Attach Context** tab.

## Attach Context

This section of the **Create a Blueprint Foundry** allows you to specify context questions to allow the collection of relevant information in pull requests.

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 12.43.14 PM.png" alt=""><figcaption><p>Attaching Context to a Bueprint</p></figcaption></figure>

You can include/exclude different contexts that are inherited from the global context by selecting/unselecting the **Inherited from global context** radio buttons.  Additional context can be added by pressing the **+** button under the **Collect Additional Context** section

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 3.19.43 PM.png" alt=""><figcaption><p>Collect Additional Context</p></figcaption></figure>

Then you can select the type of context you would like to add. The following options are available and can be added by pressing their respective button:

* **Text Field**
* **Single Select**
* **Multi Select**

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 3.19.46 PM.png" alt=""><figcaption><p>Collect Additional Context data</p></figcaption></figure>

Once an option is selected, you will see a popup that allows you to edit the context field. Make sure to fill in all the required sections and press the **Save Field** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 3.20.52 PM.png" alt=""><figcaption><p>Edit context field - Single Select</p></figcaption></figure>

Once the context has been saved it will be displayed in the **Collect Additional Context** section

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 3.21.02 PM.png" alt=""><figcaption><p>Added Additional Context</p></figcaption></figure>

Now you can proceed to **Preview and Create** your new custom Blueprint

## Preview and Create Blueprint

Resourcely Foundry allows you to view how the Blueprint will be created. The following preview types are available:

* **Developer Experience**
* **Terraform**

Click the **Developer Experience** tab to preview the fields that your developers will see and interact with as they create new resources

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 12.43.28 PM.png" alt=""><figcaption><p>Foundry Developer Experience</p></figcaption></figure>

Once you confirm that the **Developer Preview** meets your needs, you can see the Terraform definition this blueprint will generate by clicking the **Terraform** tab

{% hint style="info" %}
Adjust inputs under **Developer Experience** to see how they reflect in the Terraform output
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 12.43.32 PM.png" alt=""><figcaption><p>Foundry Terraform Preview</p></figcaption></figure>

After validating the Terraform file that will be created, you go ahead and create the blueprint by pressing the **Create Blueprint** button.

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 2.35.39 PM.png" alt=""><figcaption><p>Creating a Blueprint with Foundry</p></figcaption></figure>

You'll be given the option to either:

* **Publish this blueprint:** Developers will immediately be able to use it to create pull requests
* **Create the blueprint without publishing:** Admins can review and further configure it before developers can use it to create pull requests

Once you click on an option the Blueprint will be created and you will be taken to its Blueprint definition. Here you can manage the blueprints and perform more edits when needed

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 2.36.25 PM.png" alt=""><figcaption><p>Resourcely Blueprint definition</p></figcaption></figure>

You can also publish/unpublish blueprints directly from the Blueprint menu by hovering over a Blueprint and pressing the **Publish/Unpublish** button

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-15 at 2.38.22 PM.png" alt=""><figcaption><p>Available Blueprints Menu</p></figcaption></figure>

Once a blueprint is available you can use it to provision infrastructure using CI/CD. To learn more about provisioning infrastructure from a blueprint, see the guide below:

{% content-ref url="../provisioning-infrastructure.md" %}
[provisioning-infrastructure.md](../provisioning-infrastructure.md)
{% endcontent-ref %}
