---
description: SSO Integration Requirements
---

# Single-Sign-On (SSO)

Single-Sign-On (SSO) is a method of authentication that allows users to securely authenticate with multiple applications and websites using a single set of credentials.

{% hint style="info" %}
If you have any issues with SSO integration, please [reach out to request assistance](https://resourcely.atlassian.net/servicedesk/customer/portal/1).
{% endhint %}

## Auth0

Resourcely uses [Auth0](https://auth0.com/) internally for our Single Sign-On (SSO) system. In order to onboard you to our platform, we need to know which SSO system you use. This will help us understand how to establish a connection with Auth0.

Depending on which SSO Identity Providers you use, typically we require the following to complete the Enterprise connection with [Auth0](https://auth0.com/docs/authenticate/identity-providers/enterprise-identity-providers):

* **Domain:** The domain refers to the URL or domain name associated with your SSO Provider organization's account.
* **Client ID:** a public identifier that uniquely identifies a client application registered with an authorization server.
* **Client Secret:** a confidential secret that is known only to the client application and the authorization server. It should be kept secret and never exposed in client-side code or public configurations.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-24 at 2.44.20 PM (1).png" alt=""><figcaption><p>Supported Enterprise Connections</p></figcaption></figure>

For more information please check out Auth0 [docs](https://auth0.com/docs/authenticate/protocols).

### Okta Integration

To enable the integration using the Auth0 native Okta integration [OIDC](https://auth0.com/docs/authenticate/identity-providers/enterprise-identity-providers/okta#create-an-okta-oidc-application), we only require the following information:

* **Okta Domain:** The domain refers to the URL or domain name associated with your SSO Provider organization's account

{% hint style="info" %}
Your Okta domain looks like this:

* `example.oktapreview.com`
* `example.okta.com`
* `example.okta-emea.com`
{% endhint %}

* **Client ID:** a public identifier that uniquely identifies a client application registered with an authorization server
* **Client Secret:** a confidential secret that is known only to the client application and the authorization server. It should be kept secret and never exposed in client-side code or public configurations.
* **Callback URL:** URLs that are invoked after the authentication process.

{% hint style="info" %}
The Resourcely callback URL is [https://resourcely.us.auth0.com/login/callback](https://resourcely.us.auth0.com/login/callback)
{% endhint %}

* **Sign-Out Redirect URI (optional):** Redirects users with an alternative logout

{% hint style="info" %}
The Resourcely Sign-Out Redirect URI is [https://resourcely.us.auth0.com/v2/logout](https://resourcely.us.auth0.com/v2/logout)
{% endhint %}



<figure><img src="../../.gitbook/assets/Screenshot 2023-08-24 at 2.44.44 PM.png" alt=""><figcaption><p>New Okta Workforce Connection Screen</p></figcaption></figure>

You can use this Terraform resource to set up the integration if you manage Okta through Terraform:

```yaml
resource "okta_app_oauth" "resourcely" {
  label       = "Resourcely"
  type        = "web"
  grant_types = ["authorization_code"]
  redirect_uris = ["<https://resourcely.us.auth0.com/login/callback>"]
  groups_claim {
    type        = "FILTER"
    filter_type = "STARTS_WITH"
    name        = "groups"
    value       = "resourcely"
  }
}

resource "okta_group" "resourcely_admins_group" {
  name        = "resourcely-admins"
  description = "Resourcely admins"
  lifecycle {
    prevent_destroy = true
  }
  custom_profile_attributes = jsonencode({
    "DepartmentGroup" = okta_group.group{NO}.name
  })
}
```
