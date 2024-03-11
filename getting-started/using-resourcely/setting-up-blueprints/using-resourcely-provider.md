---
description: Provider is a logical abstraction of an upstream API
---

# Using Resourcely Provider

Resourcely's Terraform provider is a powerful tool that lets you manage your blueprint and global context definitions as code. Here's how to do that step by step.

#### **Step 1: Set Up the Provider**

In your Terraform configuration file, start by defining the required provider:

```
terraform {
  required_providers {
    resourcely = {
      source = "registry.terraform.io/Resourcely-Inc/resourcely"
    }
  }
}

```

This section ensures you're using the Resourcely Terraform provider from the official Terraform Registry.

#### **Step 2: Authentication**

Before you can run any Terraform commands to manage resources with Resourcely, you must authenticate with the service. The Resourcely provider expects an authentication token to be provided through the **`RESOURCELY_AUTH_TOKEN`** environment variable.

On Unix-like systems (Linux, MacOS), set the environment variable using:

```bash
export RESOURCELY_AUTH_TOKEN="<your_auth_token>"
```

On Windows (Command Prompt):

```bash
set RESOURCELY_AUTH_TOKEN=<your_auth_token>
```

On Windows (PowerShell):

```powershell
$env:RESOURCELY_AUTH_TOKEN="<your_auth_token>"
```

Replace **`<your_auth_token>`** with the actual authentication token provided by Resourcely. You can generate the token under `settings` → `Generate API token` → `New access token`

#### **Step 3: Verify Authentication**

Before proceeding with the rest of the Terraform commands, ensure the environment variable is set correctly:

```bash
echo $RESOURCELY_AUTH_TOKEN
```

This should print out the token value. (For security reasons, avoid doing this in a shared environment or where the output can be logged or captured.)

#### **Step 4: Continue with Terraform**

Now that you're authenticated, you can proceed with the rest of the Terraform operations, as detailed in the previous steps:

1. Define the provider.
2. Define blueprints.
3. Define global context.
4. Initialize and apply configurations with **`terraform init`** and **`terraform apply`**.

#### **Important Notes:**

* Make sure to secure your auth token. Avoid hardcoding it in scripts or Terraform configurations.
* In a CI/CD environment, consider using secure environment variable mechanisms or secrets management tools to set the **`RESOURCELY_AUTH_TOKEN`**.
* Remember to review Resourcely's documentation periodically to be aware of any changes to the authentication mechanism or any other provider-specific details.

#### **Blueprints**

Blueprints are templated Terraform files that streamline creating similar configurations. They use tags as placeholders, enhancing reusability and customizability across diverse setups. During resource creation, the blueprint is rendered into final Terraform configuration with tags substituted by the values provided by a developer. Resourcely will open a PR containing the ready-to-apply, customized Terraform config.

#### Authoring Resourcely Terraform Blueprint

For more detailed steps on authoring your own blueprints, please refer to the Resourcely documentation: [Authoring Your Own Blueprints](https://docs.resourcely.com/getting-started/using-resourcely/setting-up-blueprints/authoring-your-own-blueprints/).

**Step 1**: create a new file named `basic.tft` using your preferred text editor or IDE.

```
---
constants:
  __name: "{{ bucket }}_{{ __guid }}"
---

resource "aws_s3_bucket" "{{ __name }}" {
  bucket = "{{ bucket }}"
}

resource "aws_s3_bucket_public_access_block" "{{ __name }}" {
    bucket = aws_s3_bucket.{{ __name }}.id

    block_public_acls       = true
    block_public_policy     = true
    ignore_public_acls      = true
    restrict_public_buckets = true
}

resource "aws_s3_bucket_ownership_controls" "{{ __name }}" {
  bucket = aws_s3_bucket.{{ __name }}.id

  rule {
    object_ownership = "BucketOwnerEnforced"
  }
}

resource "aws_s3_bucket_versioning" "{{ __name }}" {
    bucket = aws_s3_bucket.{{ __name }}.id
    versioning_configuration {
        status = "{{ versioning_configuration_status }}"
    }
}
```

**Step 2**: Define the blueprint for the AWS S3 bucket referencing the Blueprint in `main.tf`:

```
resource "resourcely_blueprint" "aws_s3_basic" {
  name           = "Basic S3 bucket"
  description    = "Basic bucket with configurable versioning"
  cloud_provider = "PROVIDER_AMAZON"
  categories     = ["BLUEPRINT_BLOB_STORAGE"]
  labels         = ["s3", "private"]
  
  excluded_context_question_series = [
    resourcely_context_question.foo.series_id, 
    resourcely_context_question.bar.series_id,
    resourcely_context_question.environment.series_id,
  ]
  
  guidance       = <<-EOT
  Use this when you want a basic private bucket.

  Options:
   - Bucket name
   - Versioning (enabled/disabled)

  Properties:
   - Public access block
   - Objects are bucket owner enforced
  EOT
 content = file("basic.tft")
}
```

You can use the **`cloud_provider`** attribute to specify the cloud provider for the blueprint. Possible values are:

* "PROVIDER\_AMAZON"
* "PROVIDER\_GOOGLE"
* "PROVIDER\_GITHUB"
* "PROVIDER\_AZURE"
* "PROVIDER\_JUMPCLOUD"
* "PROVIDER\_OTHER"

The **`categories`** attribute indicates what type of blueprint it is, using values like **`BLUEPRINT_BLOB_STORAGE`**, **`BLUEPRINT_NETWORKING`**, etc.

if you don't specify `excluded_context_question_series` then blueprint gets all global context questions by default.

#### **Global Context**

Global Contexts are context-prompting questionnaires used to gather data from developers before provisioning a resource. Global Contexts are designed to gather and store insightful data related to the resource that will be generated.

For example, to define an environment questions in `main.tf`:

```
resource "resourcely_context_question" "environment" {
  prompt               = "Environment?"
  label                = "Environment"
  qtype                = "QTYPE_SINGLE_SELECT"
  scope                = "SCOPE_TENANT"
  blueprint_categories = [
    "BLUEPRINT_BLOB_STORAGE", "BLUEPRINT_NETWORKING",
    "BLUEPRINT_DATABASE", "BLUEPRINT_COMPUTE",
    "BLUEPRINT_SERVERLESS_COMPUTE", "BLUEPRINT_ASYNC_PROCESSING",
    "BLUEPRINT_CONTAINERIZATION", "BLUEPRINT_LOGS_AND_METRICS"
  ]
  answer_choices       = [
    { label : "Production" },
    { label : "Non-production" },
    { label : "Sandbox" }
  ]
}
```

This global context will prompt users to select an environment (Production, Non-production, or Sandbox) when deploying blueprints from the mentioned categories.

You can use adjust the qtype according to the question's nature.

* “QTYPE\_TEXT”
* “QTYPE\_SINGLE\_SELECT”
* “QTYPE\_MULTI\_SELECT “

#### **Step 4: Apply Changes**

After you've defined your global context and blueprints, you can apply the changes:

```bash
terraform init
terraform apply
```

This will initialize the provider and apply your configurations, creating or updating the resources in Resourcely based on your Terraform code.

#### **Step 5: Manage and Version Control**

With the above setup, you can manage your blueprints and global context in version control (e.g., Git) to track changes, collaborate with teammates, and maintain a history of modifications.

Remember, this is a basic guide to get started. Depending on your requirements, you might have to delve deeper into the Resourcely provider's documentation to leverage more advanced functionalities.
