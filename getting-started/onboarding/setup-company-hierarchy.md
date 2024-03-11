---
description: Adding your company and setting up the organizational hierarchy
---

# Setup Company Hierarchy

Let’s get started with configuring your company's hierarchy within Resourcely. This step entails:

* Providing a Company name
* Defining the Organization(s) within your Company
* Defining the Groups within your Organization(s)
* Assigning Reviewers to Group(s)
* Defining Team(s) within your Organization(s)

{% hint style="info" %}
To learn more about the organizational hierarchy within Resourcely, see the [Resourcely Terms](../resourcely-terms.md).
{% endhint %}

## Providing a Company Name

In this section, you must provide your **Company name** which will be used to identify your tenant installation of Resourcely.

{% hint style="info" %}
The company name can be changed after onboarding is complete.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.26.35 PM.png" alt=""><figcaption><p>Add Company Name</p></figcaption></figure>

Once you've entered the name, press the **Save and continue** button to proceed.

## Defining the Organization(s) within your Company

Now let's define an **Organization** that is part of your company. An Organization represents a collection of settings within your company. You can see below that the Organization name is set to **Default.**&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-21 at 12.24.24 PM.png" alt=""><figcaption><p>Default Organization</p></figcaption></figure>

You can personalize the Default organization to better represent a segment of your company. Press the **Settings** button in the Default section to personalize the organization.

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.27.26 PM (3).png" alt=""><figcaption><p>Configuring an Organization</p></figcaption></figure>

You can change the organization name and description and press **Continue** to proceed.

{% hint style="info" %}
**Note:** You only need one organization to start the onboarding process, but you'll have the option to add more in the future.
{% endhint %}

## Defining Group(s) within your Organization(s)

Now let's create a **Group** within our organizations. You can create a Group by pressing the **Add group** button. Here you will need to provide the name of your Group (Example: Security, Development, Compliance, etc.)

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.27.33 PM (2).png" alt=""><figcaption><p>Configuring Groups</p></figcaption></figure>

You can now provide a name and email for your group. The provided email will be sent notifications when reviews are required.

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.28.19 PM.png" alt=""><figcaption><p>Adding a new Security Group</p></figcaption></figure>

You can press the **Add group button to add more groups as needed.**

{% hint style="info" %}
**Note:** To begin onboarding, only one group is essential. However, you have the flexibility to add more groups later.
{% endhint %}

Press the **Continue** button to proceed.

## Assigning Reviewer(s) to Group(s)

After the Group has been added, we must add a Reviewer to the group. You can add a reviewer by pressing the **Add reviewer** button. You must then **select a provider (GitHub or GitLab)** and **type in the Reviewer's username**.

{% hint style="warning" %}
If using GitHub as a provider, you must add a GitHub team as a reviewer instead of individual users. For GitLab, you must add the exact username.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-21 at 1.32.43 PM.png" alt=""><figcaption><p>Adding Source Control Reviewers</p></figcaption></figure>

{% hint style="info" %}
**Note:** Only one reviewer is required to onboard. You can add more later as needed.
{% endhint %}

Once reviewers have been assigned to a group you can press the **Continue** button to proceed to creating a Team.

## Defining Team(s) within your Organization(s)

Teams are relevant for providing functions like RBAC (Role-Based Access Control) or determining notification recipients.&#x20;

{% hint style="danger" %}
Teams are currently not being used but will be enabled in the future. For simplicity, set your team name to mirror the group name. You can manage teams after onboarding.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.30.25 PM.png" alt=""><figcaption><p>Setting up Teams</p></figcaption></figure>

Once you have added a Team, press the **Publish** button to proceed. You'll be taken back to the organization management page, where you can see the changes made to your organization.

<figure><img src="../../.gitbook/assets/Screenshot 2023-12-20 at 7.30.39 PM.png" alt=""><figcaption><p>Organization overview</p></figcaption></figure>

You can create more organizations if needed or press the **Save and continue** button to proceed to configure [Source Code Management integrations](source-code-management-integration/).

{% hint style="info" %}
**Note:** Only one Organization is required to onboard. You can add more later as needed.
{% endhint %}
