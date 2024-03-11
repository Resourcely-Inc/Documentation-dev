---
description: Source Code Management (SCM) Integration
---

# 🦊 GitLab

Let’s get started with integrating GitLab with Resourcely. This step entails:

* Authenticate Resourcely with GitLab
* Integrating Resourcely with GitLab using a Group Access Token
* Configuring a Webhook for Resourcely
* Choosing a Repository for Resourcely to Monitor

## Authenticate Resourcely with GitLab

Now let's connect Resourcely to GitLab to enable automatic runs on git commits, automatic runs on merge-requests, and status updates. Press the **Connect to GitLab** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-20 at 7.30.51 PM (1).png" alt=""><figcaption><p>Configure change management onboarding home</p></figcaption></figure>

For Resourcely to modify GitLab settings, you must first grant the appropriate level of authorization on your behalf. Press the **Authorize with GitLab** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.33.55 PM (2).png" alt=""><figcaption><p>Authorize with GitLab request</p></figcaption></figure>

You will then be taken to your GitLab user settings and asked to Authorize Resourcely to use your account. Press the **Authorize** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.42.12 PM (1).png" alt=""><figcaption><p>GitLab Resourcely Authorization page</p></figcaption></figure>

Go back to the Resourcely window and proceed with onboarding.&#x20;

## Integrating Resourcely with GitLab using a Group Access Token

To grant access to all projects within the GitLab group a group access token is required. Enter the Group Access Token created as seen in the [gitlab.md](../../prerequisites/source-code-management/gitlab.md "mention") prerequisites section. Then press the **Continue** button to proceed.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-20 at 7.37.17 PM.png" alt=""><figcaption><p>Provide a group access token</p></figcaption></figure>

{% hint style="info" %}
The group access token is used by Resourcely to verify the GitLab integration such as verifying the webhook
{% endhint %}

## Configuring a GitLab Webhook for Resourcely

GitLab Webhooks deliver notifications to an external web server whenever a defined event occurs on GitLab. To proceed to configure a GitLab webhook, click on the **GitLab webhook Settings** link and copy over the **Webhook URL** and **Webhook secret token** provided in the following dialogue:

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.57.19 PM.png" alt=""><figcaption><p>Create a GitLab webhook</p></figcaption></figure>

You must then configure the webhook with the following settings:

* &#x20;**Trigger:** Merge request events

{% hint style="info" %}
All other webhook settings can remain unchanged.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2023-10-19 at 9.23.19 AM.png" alt=""><figcaption><p>GitLab Webhook Settings</p></figcaption></figure>

Once the webhook has been properly set up, scroll down to the bottom of the page and press the **Add webhook** button. You can now see that the webhook was added successfully.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-10-19 at 9.23.46 AM.png" alt=""><figcaption><p>Active Webhooks</p></figcaption></figure>

Navigate back to the Resourcely window and press the **Continue** button to proceed.

## Choosing a Repository for Resourcely to Monitor

You must now choose the repository that hosts your Terraform code. This is the repository in which Resourcely will watch for commits and merge-requests.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.57.29 PM.png" alt=""><figcaption><p>Choose a repository</p></figcaption></figure>

Press the **Add a repository** button and select a repository from the dropdown.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.57.34 PM.png" alt=""><figcaption><p>Select a repository from the list</p></figcaption></figure>

You can add more repositories to monitor by pressing the **Add repository** button.

{% hint style="info" %}
To begin onboarding, only one repository is needed. However, you have the flexibility to add more later.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.57.38 PM.png" alt=""><figcaption><p>Repository selected</p></figcaption></figure>

Press the **Publish** button to continue. You should now see that GitLab has been successfully integrated with Resourcely.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-12-21 at 12.57.53 PM.png" alt=""><figcaption><p>GitLab integrated into Change Managment</p></figcaption></figure>

Now that Resoucely has been fully integrated into GitLab, we must add the Resourcely CI/CD Job to our project for Resourcely to validate our infrastructure resources. Proceed to [api-access-token-generation.md](../api-access-token-generation.md "mention")
