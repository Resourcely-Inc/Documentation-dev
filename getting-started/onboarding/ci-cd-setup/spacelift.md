---
description: CI/CD Automation
---

# 🌌 Spacelift

You can integrate Resourcely with Spacelift to automatically evaluate your Terraform plans and policies on every pull request, and provide feedback directly within your pull/merge request. To set up Resourcely with Spacelift, you must perform the following steps:

* Verifying Prerequisites
* Storing the Resourcely API Token
* Storing the Github Personal Access Token
* Integrate Resourcely CLI into your Plan Hooks

## Verify Prerequisites

This document assumes that you have a pre-existing Spacelift project configured. If you do not, you can follow their Getting Started steps here: [https://docs.spacelift.io/getting-started](https://docs.spacelift.io/getting-started)

## Storing the Resourcely API Token

The Resourcely CLI needs access to a Resourcely API key at build time so it can report findings in your Terraform plans. Spacelift allows you to store these secrets in each stack as an Environment.

1. In the Spacelift console navigate to the Stack you want to integrate with Resourcely.
2. Click 'Edit'.
3. With 'Environment Variable' selected, type `RESOURCELY_API_TOKEN` as the key.
4. Paste the API Token generated from the Resourcely portal as the value.
5. Click "Secret" to save this variable as a secret (this will prevent it from being exposed in stdout).

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-08 at 3.43.09 PM.png" alt=""><figcaption></figcaption></figure>

## Storing the Github Personal Access Token

We need a Github Personal Access Token in order for Resourcely to map your Pull Request URL when your guardrails are evaluated. For details on how to generate a Personal Access Token, you can view the following docs: [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

1. In the Spacelift console navigate to the Stack you want to integrate with Resourcely.
2. Click 'Edit'.
3. With 'Environment Variable' selected, type `GH_TOKEN` as the key.
4. Paste the Personal Access Token generated from Github as the value.
5. Click "Secret" to save this variable as a secret (this will prevent it from being exposed in stdout).

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-08 at 3.41.16 PM (1).png" alt=""><figcaption></figcaption></figure>

## Integrate Resourcely CLI into your Plan Hooks

Spacelift allows you to specify Hooks that you can run at different stages of your pipeline. We setup the Resourcely CLI to run after the Plan runs, which allows us to evaluate our guardrails against your planned changes. Use the following steps to setup your Post-Plan flow with Resourcely:

1. In your desired Stack, navigate to Hooks
2. Click 'Planning'
3. Navigate to 'After'
4. Individually paste the following commands:

```shellscript
LATEST_RELEASE_TAG=$(curl -s -I https://github.com/cli/cli/releases/latest | awk -F '/' '/^location/ {print  substr($NF, 1, length($NF)-1)}')
curl -s -L -O https://github.com/cli/cli/releases/download/$LATEST_RELEASE_TAG/gh_${LATEST_RELEASE_TAG#v}_linux_amd64.tar.gz > /dev/null && tar -zxf gh_${LATEST_RELEASE_TAG#v}_linux_amd64.tar.gz && mv gh_${LATEST_RELEASE_TAG#v}_linux_amd64/bin/gh .
export GH_PR_URL=$(./gh api https://api.github.com/repos/$TF_VAR_spacelift_repository/commits/$TF_VAR_spacelift_commit_sha/pulls | jq -r '.[0].html_url')
terraform show -json spacelift.plan > spacelift.json
LATEST_RELEASE_TAG=$(curl -s -I https://github.com/Resourcely-Inc/resourcely-container-registry/releases/latest | awk -F '/' '/^location/ {print  substr($NF, 1, length($NF)-1)}')
curl -s -L -O https://github.com/Resourcely-Inc/resourcely-container-registry/releases/download/$LATEST_RELEASE_TAG/resourcely-cli-${LATEST_RELEASE_TAG}-linux-amd64.tar.gz > /dev/null && tar xvzf resourcely-cli-${LATEST_RELEASE_TAG}-linux-amd64.tar.gz
[ -z "$GH_PR_URL" ] && export RESOURCELY_EVALUATE_DRY_RUN=true || export RESOURCELY_EVALUATE_DRY_RUN=false
export RESOURCELY_EVALUATE_CHANGE_REQUEST_URL=$GH_PR_URL && export RESOURCELY_EVALUATE_CHANGE_REQUEST_SHA=$TF_VAR_spacelift_commit_sha
./resourcely-cli evaluate --vcs github spacelift.json
```

Now Resourcely should be integrated into your Terraform flow, and we will alert your PRs with any violations to the Guardrails you've configured.
