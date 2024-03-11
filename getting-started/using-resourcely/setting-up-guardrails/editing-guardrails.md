---
description: Edit guardrails that govern how cloud resources are created and changed
---

# Editing Guardrails

Resourcely allows you to edit the available guardrails seen in the Guardrail catalog. From the Guardrail section, select the guardrail you would like to edit, in the popup press the **Manage** button and select **Edit**

{% hint style="info" %}
You can also Deactivate, Clone, and Delete a guardrail from the same menu
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.18.27 PM.png" alt=""><figcaption><p>Guardrail Action Selection</p></figcaption></figure>

In the next screen, you can configure your guardrail functionality. This determines what changes Resourcely will watch and potentially block.

In this example, we edit the **S3 naming convention** guardrail which requires a specific **prefix in** the name of an S3 bucket**.** We can configure the prefix from the default `resourcely-` to anything we wish. We can also select the **Group** responsible for approving guardrail overrides.

After your guardrail has been configured, press **Continue** to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.18.45 PM.png" alt=""><figcaption><p>Configure Guardrail Functionality</p></figcaption></figure>

Now we must provide the metadata for a guardrail in order to effectively describe the change. Guardrail metadata includes:

* Name
* Description
* Category

After the metadata has been filled, press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.19.05 PM.png" alt=""><figcaption><p>Provide Guardrail Metadata</p></figcaption></figure>

In this section, we can verify the changes and then press the **Update Guardrail** button to publish your guardrail and make it immediately available to your end users.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-09-01 at 1.19.14 PM.png" alt=""><figcaption><p>Review and Confirm Guardrail</p></figcaption></figure>
