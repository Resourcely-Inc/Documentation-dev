---
description: Source Code Management (SCM) Setup
---

# 🦊 GitLab

{% hint style="warning" %}
In order to integrate Resourcely with GitLab, you must have a **Premium** or greater tier subscription that is not part of a free trial. See [GitLab pricing](https://about.gitlab.com/pricing/) for more information.

**Note:** Only the GitLab SaaS offering is supported. We will provide support for Self-Managed GitLab instances in the near future.
{% endhint %}

Once you have configured your GitHub account, the following is required:

* GitLab Top-Level Group
* GitLab Group Access Token

## GitLab Top-Level Group

In GitLab, groups are used to manage one or more related projects simultaneously. In order to configure Resourcely you must have the [Maintainer or Owner role](https://docs.gitlab.com/ee/user/permissions.html#roles) within the group. You can search for your group by the **Search or go to** button in the GitLab side tab**.**

<figure><img src="../../../.gitbook/assets/Screenshot 2023-10-19 at 9.06.14 AM.png" alt=""><figcaption><p>Top-level group displaying projects</p></figcaption></figure>

To learn more about Groups, see the following documentation:

{% embed url="https://docs.gitlab.com/ee/user/group/" %}
GitLab Groups - Documentation
{% endembed %}

## GitLab Group Access Token

GitLab group access tokens allow you to use a single token to perform actions for groups and manage the projects within the group.

Before a group access token can be created, you must enable Group Access Tokens within your Group. See the following documentation to learn more about Group Access Tokens:

{% embed url="https://docs.gitlab.com/ee/user/group/settings/group_access_tokens.html#enable-or-disable-group-access-token-creation" %}
Enable Group Access Tokens - Documentation
{% endembed %}

Once you have enabled group access tokens, you can create one from the **Settings > Access Tokens** section within the Group side tab. You must create one with the following permissions:

* **Role:** Developer Role
* **Scope:** api

<figure><img src="../../../.gitbook/assets/Screenshot 2023-10-19 at 9.11.35 AM.png" alt=""><figcaption><p>Group Access Token Permissions</p></figcaption></figure>

See the following documentation to learn more about creating Group Access Tokens:

{% embed url="https://docs.gitlab.com/ee/user/group/settings/group_access_tokens.html#create-a-group-access-token-using-ui" %}
Create Group Access Tokens - Documentation
{% endembed %}

Once the GitLab setup has been completed, you must integrate Terraform into the project you will be using for resource provisioning. Learn more in the [terraform-integration](../terraform-integration/ "mention") section.
