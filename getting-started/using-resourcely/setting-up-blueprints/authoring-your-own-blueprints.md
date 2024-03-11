---
description: >-
  Create cloud infrastructure templates tailored specifically for your company
  and teams
---

# Authoring Your Own Blueprints

Blueprints are templated Terraform files that streamline creating similar configurations. They use tags as placeholders, enhancing reusability and customizability across diverse setups.&#x20;

During resource creation, the blueprint is rendered into the final Terraform configuration with tags substituted by the values provided by a developer. Resourcely will open a PR containing the ready-to-apply, customized Terraform config.

## Quickstart

The blueprint templating syntax consists of variable tags, section blocks, and other metadata.

**Variable Tag**: A placeholder `{{ name }}` that gets replaced with its respective value during rendering.

**Section Block**: A block of content enclosed in opening and closing section tags `{{# name }}...{{/ name }}`. During rendering, the content is repeated once for each element of the respective value (which must be a list).

**Tag Parameters**: Arguments to tags are specified using the pipe `|` character: `{{ name | desc: "Name of project" | required: true }}`.

**Special Variable**: `{{ __guid }}` is a special variable that generates a single unique value during rendering, useful for ensuring unique resource names across multiple renderings.

**Frontmatter**: The blueprint may start with a YAML frontmatter section, specifying both variable objects (as alternative means of declaring tag parameters) and constant objects (defining internal constant values).

### Example

For a practical understanding of the Blueprint templating language, consider the following example that creates an AWS EC2 instance with a corresponding security group with arbitrary ingress and egress rules.

{% code title="example.tft" overflow="wrap" lineNumbers="true" %}
```
---
constants:
  __name: "{{ name }}_{{ __guid }}"
variables:
  instance_type:
    desc: "Type of AWS instance"
    suggest: "t2.micro"
  ingress_rules.cidr_blocks:
    suggest: ["0.0.0.0/0"]
  egress_rules.cidr_blocks:
    suggest: ["0.0.0.0/0"]
---
resource "aws_instance" "{{ __name }}" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = {{ instance_type | order: 2 }}

  tags = {
    Name = {{ name | type: string | order: 1 }}
  }

  vpc_security_group_ids = [aws_security_group.{{ __name }}_sg.id]
}

resource "aws_security_group" "{{ __name }}_sg" {
  name        = {{ name }}
  description = "Security group for {{ name }}"

  {{# ingress_rules | desc: "List of ingress rules" | order: 3 }}
  ingress {
    from_port   = {{ ingress_rules.port_range.low  | default: 22 }}
    to_port     = {{ ingress_rules.port_range.high | default: 22 }}
    protocol    = {{ ingress_rules.protocol        | default: "tcp" }}
    cidr_blocks = {{ ingress_rules.cidr_blocks }}
  }
  {{/ ingress_rules }}

  {{# egress_rules | desc: "List of egress rules" | order: 4 }}
  egress {
    from_port   = {{ egress_rules.port_range.low  | default: 22 }}
    to_port     = {{ egress_rules.port_range.high | default: 22 }}
    protocol    = {{ egress_rules.protocol        | default: "tcp" }}
    cidr_blocks = {{ egress_rules.cidr_blocks }}
  }
  {{/ egress_rules }}
}
```
{% endcode %}

From this blueprint, the Resourcely Portal generates a form that makes it easy for a developer to supply the required values and render the resulting Terraform configuration.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Syntax

### Variable Tags

Variable tags are inputs that are replaced by respective values during rendering. The syntax for variable tags is `{{ name }}`.

```
resource "aws_s3_bucket" "bucket" {
  bucket = "{{ bucket_name }}"
  acl    = "private"
}
```

### Section Tags

Section tags enclose content that can be repeated during rendering. A `{{# name }}` tag opens a repeated section and a corresponding `{{/ name }}` closes it. Section tags can be nested to allow complex structures.

The section tag is given a list of values during rendering. The enclosed content is repeated for each value in the list. Nested tags can reference the current element by using the section name.

```
{{# iam_users }}
resource "aws_iam_user" "user_{{ iam_users.__index }}" {
  name = "{{ iam_users.name }}"
  path = "/system/"
}
{{/ iam_users }}

```

### Dotted Syntax in Tag Names

Dotted syntax in tag names allows for accessing nested properties of an input object supplied for rendering. When values are passed into the blueprint for rendering, they form a structure that mirrors the structure of the tags in the blueprint.

This becomes even more useful when dealing with repeated sections. For instance, consider a repeated section `{{# users }}...{{/ users }}` that creates multiple users. Inside this section, you might have tags like `{{ users.name }}` and `{{ users.email }}`. Here, the users list will be an array of objects, with each object having properties name and email.

### Tag Parameters

Tag parameters are additional metadata for each tag. They can be declared inline inside a tag using the pipe (`|`) character:

```
{{ instance_type | desc: "AWS instance type" | required: true }}
```

They can also be declared in the front matter.

```
---
variables:
  instance_type:
    desc: "AWS instance type"
    required: true
---
{{ instance_type }}

```

The following tag parameters are supported. All are optional.

**type**

Defines the type of the input variable, such as string, number, or boolean. It helps in validating the input and may affect how the input is rendered or processed.

{% hint style="info" %}
The type parameter is usually automatically inferred, so does not need to be explicitly included. See [Type Inference](authoring-your-own-blueprints.md#type-inference) for details.
{% endhint %}

**desc**

Description of the variable. This will be shown in the Resourcely Portal to assist the user creating a resource from the blueprint.

**required**

A boolean that, when true, specifies that the input variable must be provided. If a required variable is missing, it will prevent the blueprint from being instantiated.

**suggest**

Specifies an initial value for the input variable. This value will be pre-populated when the create form loads, but the user can change it.

{% hint style="warning" %}
Currently complex suggestions (lists and objects) can only be specified in the frontmatter, not inline. Primitive defaults can be specified inline as well.
{% endhint %}

**order**

Controls the order in which input fields appear in the Resourcely Portal form.

**group**

Specifies a grouping for the input fields in the Resourcely Portal form.

**global\_value**

Constrains the available inputs to only those referenced by a [Global Value](authoring-your-own-blueprints.md#global-values) The value here should be the `key` of the Global Value.

**advanced**

This will move this particular input to an "Advanced" section at the bottom of the Resourcely Portal Create form. This is usually used for optional inputs that require additional context only a power user will have.

### Type Inference

The `type` of most tags is inferred automatically from their context within the Terraform config. If a tag is used as the value for a Terraform attribute, Resourcely will use the expected type of that attribute. For instance, if a tag is used as the value for the bucket attribute of an `aws_s3_bucket` resource, the tags will be automatically inferred from the type of `aws_s3_bucket.bucket`.

However, there may be instances where automatic type inference is not possible or sufficient. In these cases, the type parameter can be explicitly specified. This is often necessary when the tag is used as an input to a Terraform module, as modules do not have a fixed structure that the templating engine can infer types from.

For instance, a tag with an explicit type parameter may look like

```
{{ module_input | type: string | desc: "Input for Terraform module" }}
```

Here, the `type: string` specification ensures that `module_input` will be processed as a string, even though its type cannot be inferred from the content.

### Type Syntax

The type syntax is an extension of the Terraform [type constraint syntax](https://developer.hashicorp.com/terraform/language/expressions/type-constraints). In addition to the types supported by Terraform, you can reference the type of resource property directly.

```
{{ foo | type: resource.aws_s3_bucket_versioniong.versioning_configuration.status }}
```

or

```
{{ foo | type: data.aws_ami.filter }}
```

This makes it possible to easily give exact types for module inputs without having to duplicate the entire typing of the underlying resource properties.

These "reference" types can be nested inside of list or object types, just like any other type.

### Module input defaults

If your blueprint invokes a module, you can specify each module input's default value in the blueprint. Resourcely will omit that property during rendering if the current value is equal to the default value. This can lead to substantially shorter module blocks, especially for modules with a large number of optional properties.

You can specify both **default** and **suggest** on one variable. Resourcely will initialize the create form using the **suggest** value and omit the property if the user enters the **default** value. This is useful with 3rd-party modules: if you don't like their default, Resourcely can suggest your preferred default.

If only **default** is specified, Resourcely will automatically **suggest** the **default** value. This lets the create form user see what the value will be, even though it won't appear in the rendered Terraform.

```
---
variables:
  ingress_rules:
    default: ["10.0.0.0/8"]
  egress_rules:
    default: []
    suggest: ["10.0.0.0/8"]
---
module "example" {
  source = "example-source"
  
  // The create form will be pre-populated with one ingress rule: "10.0.0.0/8".
  // If the user leaves it as-is, Resourcely will omit the ingress_rules property.
  ingress_rules = {{ ingress_rules }}
  
  // Here, default != suggest. The create form will be pre-populated with one egress
  // rule: "10.0.0.0/8". If the user removes it, Resourcely will omit the property.
  egress_rules = {{ egress_rules }}
}
```

### Constants and Special Variables

Variables beginning with double underscore (\_\_) are not exposed in the Resourcely Portal form, so are considered to be internal values.

Constants can be defined in the constants section of the frontmatter. This is useful for avoiding repetition in your template.

There are two special variables whose values are automatically supplied during rendering.

**\_\_guid**

`__guid` is a special constant that gets a unique value during rendering. This is useful for generating unique resource names across multiple renderings of the same blueprint.

**\_\_index**

Inside of a repeated section, `<name>.__index` is a special variable that points to the current element's index in the `<name>` list.

### Conventions

Common convention is to define a `__name` constant that incorporates the `__guid` variable. Use `{{ __name }}` as part of each Terraform block local name (resource, data source, module, etc.) to ensure that

1. resources created together share a name, for easy identification.
2. resource names will not conflict when the Blueprint is instantiated multiple times.

### Front matter

The blueprint can include a YAML frontmatter section to define variables and constants.

**Variables**

The `variables` object is an alternative way to specify the various tag parameters for the corresponding tag:

```
variables:
   foo:
     desc: "my foo value"
     required: false
   bar:
     default: true

```

**Constants**

The `constants` object defines variables with constant values that can be referenced in the blueprint. These constant values can themselves be simple templated strings.

```
---
constants:
   __name: "{{ name }}_{{ __guid }}"
---
resource "aws_s3_bucket" "{{ __name }}" {
   bucket = {{ bucket }}
}

```

### Global Values

Admins can create globally defined values that you can then use to restrict inputs to a Blueprint.

You can reference Global Values in Blueprints through either the Frontmatter definition, or inline.

The following example shows both cases. The `dept` tag parameter references the Global Values of `department` in the frontmatter, and the `ami` tag parameter references the Global Values of `approved_amis` inline.

```
---
variables:
  dept:
    global_value: department
---
resource "aws_instance" "instance" {
  ami = {{ ami | type: resource.aws_instance.instance.ami | global_value: approved_amis }}
  display_name = "{{ dept }}"
  // ...resource properties
}
```

Global Values can also be complex types (lists/structs). Since each Global Value has a schema for these complex types, you can dereference these types directly in the Blueprint.

In the following example, `os` references the preset `os_family`, which is a struct with keys `ami` and `os_type`.

```
---
variables:
  os: 
    preset: os_family
    default: ubuntu
  dept:
    preset: department
---
resource "aws_instance" "instance" {
  ami = {{ os.ami }}
  display_name = "{{ dept }}-{{ os.os_type }}"
  // ...more resource properties
}

resource "aws_licensemanager_association" "example" {
  count = {{ os.os_type }} == "windows" ? 1 : 0
  // ...windows licensing config
}
```
