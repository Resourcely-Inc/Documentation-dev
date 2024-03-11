---
description: Source Code Management (SCM) Setup
---

# 🐱 GitHub

Once you have configured your GitHub account, the following is required:

* GitHub Teams configured within your GitHub Organization
* Terraform configured in the repositories you will be using with Resourcely

## GitHub Teams

Teams are groups of organization members that reflect your company or group's structure with cascading access permissions and mentions. To learn how to setup GitHub Teams see the following documentation:

{% embed url="https://docs.github.com/en/organizations/organizing-members-into-teams" %}
GitHub Teams documentation
{% endembed %}

The team must be configured with the following settings:

* Members who will be reviewing infrastructure resource definitions should be [added to the team](https://docs.github.com/en/organizations/organizing-members-into-teams/adding-organization-members-to-a-team)
* The team must be [added to the repositories](https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/managing-team-access-to-an-organization-repository) which will leverage Resourcely
* Auto-assignment must be enabled in team [code review settings](https://docs.github.com/en/organizations/organizing-members-into-teams/managing-code-review-settings-for-your-team)&#x20;

<figure><img src="../../../.gitbook/assets/Screenshot 2023-08-24 at 11.13.44 AM.png" alt=""><figcaption><p>GitHub Teams</p></figcaption></figure>

Once the GitHub setup has been completed, you must integrate Terraform into the project you will be using for resource provisioning. Learn more in the [terraform-integration](../terraform-integration/ "mention") section.
