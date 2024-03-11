---
description: >-
  This guide will walk you through configuring .resourcely.yaml in your
  Terraform repository.
---

# Configuring Resourcely.yaml

## What is .resourcely.yaml?

`.resourcely.yaml` is a config file in repos to which Resourcely Create can emit Terraform. It provides metadata about the structure of the Terraform config within the repo:

* paths to the `config roots` in the Repo. These are where we can emit Terraform.
* for config roots that are instantiated in multiple environments, what these environments are and where the environment-specific variable values (`*.tfvars`) are stored.

Resourcely Create will use this metadata to help developers

* place their Terraform config in the correct paths
* provide values for all environments&#x20;
* without them needing to be aware of the repository or Terraform structure themselves.

## Where to place .resourcely.yaml?

The `.resourcely.yaml` files may be placed anywhere within a repo. Multiple such files are allowed, primarily to support large mono-repos where subdirectories are owned by different teams.

The only restriction is that `.resourcely.yaml` files cannot be nested. No subdirectories below a `.resourcely.yaml` may contain a `.resourcely.yaml`.

## .resourcely.yaml Structure

There are a variety of ways that Terraform config can be structured, including:

* no-envs: one config root, no environment, one state file
* multiple-envs: one config root, `*.tfvars` file per environment, state file per environment
  * There’s an alternative flavor of this where the `.tfvars` file is not checked in. Instead the runner injects it somehow - by writing a `.tfvars` file, by writing a `.tf` file, or by adding `-var 'key=val'` to the command line.
* common-module: shared module with most config, config root per environment that instantiates this module (and any env-specific resources) (coming soon)

Rather than trying to define one config format that can describe all of these variations, we’ll support various types.

```
version: "2"

# Each .resourcely.yaml file manages all config roots in or below its directory.
# 
# - A subdirectory of a .resourcely.yaml directory may not contain its own
#   .resourcely.yaml file.
#
# - All the paths and files referenced in a .resourcely.yaml must be in or below
#   its directory. References cannot "escape" that directory. 

# List of directories that contain Terraform configuration.
#
# In the Resourcely Pull Request UI, a developer will pick one of these
# locations to put their new resources in.
terraform_config_roots:
  
  - # Name that will be shown in the Resourcely Pull Request UI
    # dropdown menu.
    name: <string>

    # (optional)
    #
    # Longer description for the config root. If developers may have trouble
    # selecting the correct config root, use this text to help guide them.
    description: <string>

    # (optional)
    #
    # Instructs Resourcely to ignore this config root. It will not be
    # shown in the Resourcely Pull Request UI dropdown menu.
    skip: true | false

    # Type of config root. Each type has its own set of 
    # configuration options.
    # 
    # Allowed Values:
    # simple        - the config root is a Terraform root module itself.
    # common-module - the config root is Terraform module that is instantiated
    #                 by other root modules.
    #
    # Environments:
    # It is common to apply a Terraform config to multiple environments, each
    # with its own Terraform state file.
    #
    # `simple` config roots use Terraform variables and per-env *.tfvars files
    # for environment-specific values. The config contains references to 
    # Terraform variables. The per-env *.tfvars files define the values of
    # those variables in each env.
    #
    # `common-module` config roots use module inputs and separate root modules
    # for environment-specific values. The module config contains references to
    # module inputs (Terraform variables).  Each env has separate root module
    # that instantiates the common module. These module instantiation blocks set
    # the values for the inputs in each env.
    type: simple | common-module

    # Path to the config root, the directory containing the *.tf
    # files.
    #
    # Relative to the location of this .resourcely.yaml file
    # 
    # If the config root is the same directory, specify
    #     path: .
    #
    # Example: projectA
    path: <string>

    # Name of the file in the `path` directory in which to place
    # new resources by default.
    #
    # Developers may pick a different file, but will be defaulted to this one.
    #
    # Example: main.tf
    default_file: <string>

    # Name of the file in the `path` directory in which to place new 
    # variable declarations for new environment-specific values.
    # 
    # For `common-module` configs, these variables are the module inputs.
    #
    # Example: variables.tf
    var_file: <string>

    # (optional)
    #
    # List of the environments that this config root supports.
    #
    # If this list is missing or empty, developers will not be allowed
    # to use environment-specific values.
    #
    # If non-empty, a developers will be allowed to use environment-specific
    # values. For any environment variable, they will have to supply a value 
    # for each environment in this list.
    environments:

      - # Name of the environment.
        #
        # This name is used for two purposes.
        # 1. It is shown in the UI to the developers.
        # 2. It is used as the value for `context.environment` in guardrails
        #    and blueprints.
        name: <string>

        # (`simple` config roots only)
        #
        # Name of the file in which to place the variables definitions
        # for this environment.
        #
        # Relative to the config root `path` directory.
        #
        # Example: envs/dev.tfvars
        tfvars_file: <string>

        # (`common-module` config roots only)
        # 
        # Path to the root module that instantiates the parent common module(s).
        # 
        # Relative to the location of this .resourcely.yaml file
        # 
        # Example: root-modules/app/dev
        root_module_path: <string>
```

### Examples

```
# Repo with one Terraform root module.
#
# Assume the repo is stuctured like
#
# network.tf
# main.tf
# providers.tf
# terraform.tf 

version: "2"

terraform_config_roots:
  - name: "Project A"
    type: simple
    path: .
    default_file: main.tf
```

```
# Repo with one Terraform root module that uses tfvars 
# to instantiante in multiple environments.
#
# Assume the repo is stuctured like
#
# network.tf
# main.tf
# providers.tf
# terraform.tf 
# variables.tf
# envs/
#   | dev.tfvars
#   | stage.tfvars
#   | envs/prod.tfvars

version: "2"

terraform_config_roots:
  - name: "Project B"
    type: simple
    path: .
    default_file: main.tf
    var_file: variables.tf
    environments:
      - name: dev
        tfvars_file: envs/dev.tfvars
      - name: stage
        tfvars_file: envs/stage.tfvars
      - name: prod
        tfvars_file: envs/prod.tfvars
```

```

# Repo with multiple Terraform root modules.
#
# Assume the repo is stuctured like
#
# projectA/
#   | network.tf
#   | main.tf
#   | providers.tf
#   | terraform.tf 
# projectB/
#   | network.tf
#   | main.tf
#   | providers.tf
#   | terraform.tf 
#   | variables.tf
#   | envs/
#   |   | dev.tfvars
#   |   | stage.tfvars
#   |   | prod.tfvars

version: "2"

terraform_config_roots:
  - name: "Project A"
    type: simple
    path: projectA
    default_file: main.tf

  - name: "Project B"
    type: simple
    path: projectB
    default_file: main.tf
    var_file: variables.tf
    environments:
      - name: dev
        tfvars_file: envs/dev.tfvars
      - name: stage
        tfvars_file: envs/stage.tfvars
      - name: prod
        tfvars_file: envs/prod.tfvars
```

## How to create resourcely.yml

You can use the [scaffolding](https://github.com/Resourcely-Inc/scaffolding-github-actions/blob/main/.resourcely.yaml) we created to get started with the basic `resourcely.yaml` file.&#x20;
