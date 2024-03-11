---
description: Setting up the Resourcely CI/CD Job
---

# CI/CD Setup

In order for Resourcely to verify infrastructure resource definitions and guardrails, it must run alongside your CI/CD pipeline.&#x20;

{% hint style="info" %}
Resourcely is designed with data privacy in mind and does not transmit terraform plans to our servers, as they may contain sensitive information.
{% endhint %}

Select the appropriate platform to continue onboarding:

{% content-ref url="github-actions/" %}
[github-actions](github-actions/)
{% endcontent-ref %}

{% content-ref url="gitlab-pipelines.md" %}
[gitlab-pipelines.md](gitlab-pipelines.md)
{% endcontent-ref %}

{% content-ref url="aws-codebuild.md" %}
[aws-codebuild.md](aws-codebuild.md)
{% endcontent-ref %}

{% content-ref url="spacelift.md" %}
[spacelift.md](spacelift.md)
{% endcontent-ref %}

