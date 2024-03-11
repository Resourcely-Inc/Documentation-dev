---
description: Enable calls to Resourcely API from your CI/CD pipeline
---

# API Access Token Generation

Now let's generate a token to configure change management and enable calls to the Resourcely API from a CI/CD pipeline. Select an Expiration and press the **Generate token** button.

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.59.21 PM.png" alt=""><figcaption><p>Generate API token</p></figcaption></figure>

A new access token is now generated. Be sure to save the token somewhere secure, it will be used to configure your CI/CD pipelines. Exit out of the screen by pressing the **X**.

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.59.29 PM.png" alt=""><figcaption><p>New Access Token</p></figcaption></figure>

Press the **Publish** button and tada, you have now successfully onboarded to Resourcely! 🎉🥳🍾

{% hint style="success" %}
**Note:** You can make changes to the items configured during onboarding by visiting the **Settings** page.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.59.41 PM.png" alt=""><figcaption><p>Onboarding Complete</p></figcaption></figure>

Now that you have successfully onboarded, you must create a CI/CD pipeline for Resourcely to scan your repositories for guardrail violations. Proceed to [ci-cd-setup](ci-cd-setup/ "mention")
