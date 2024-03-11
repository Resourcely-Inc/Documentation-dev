---
description: Require review(s) for code changes which violate guardrail requirements
---

# Guardrails in Action

Resourcely runs a CI job within your Source Code Management (SCM) platform in order to block Pull Requests that violate guardrails from being merged. The blocked Pull-Request is assigned an appropriate reviewer.

See the guardrails in action for your SCM platform:

{% content-ref url="github-actions.md" %}
[github-actions.md](github-actions.md)
{% endcontent-ref %}

{% content-ref url="gitlab-pipelines.md" %}
[gitlab-pipelines.md](gitlab-pipelines.md)
{% endcontent-ref %}
