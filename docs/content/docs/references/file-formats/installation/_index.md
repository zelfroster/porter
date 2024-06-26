---
title: Porter Installation File Format 1.0.2
description: The 1.0.2 file format for Porter Installation resources
layout: single
---

[Installations](/quickstart/desired-state/) can be defined in either json or yaml.
You can use this [json schema][inst-schema] to validate an installation file.

## Supported Versions

Below are schema versions for installations, and the corresponding Porter version that supports it.

| Schema Type  | Schema Version    | Porter Version |
|--------------|-------------------|----------------|
| Installation | [1.0.2](./1.0.2/) | v1.0.0-beta.1  |

Sometimes you may want to work with a different version of a resource than what is supported by Porter, especially when migrating from one version of Porter to another.
The [schema-check] configuration setting allows you to change how Porter behaves when the schemaVersion of a resource doesn't match Porter's supported version.

[schema-check]: /docs/configuration/configuration/#schema-check

## Example

Either the bundle digest, version, or tag must be specified.
When more than one is specified, Porter selects the most specific field available, preferring digest the most, then version, and then falling back to tag last.

```yaml
schemaType: Installation
schemaVersion: 1.0.2
name: myinstallation
namespace: staging
uninstalled: false
labels:
  team: marketing
  customer: bigbucks
bundle:
  repository: ghcr.io/getporter/examples/porter-hello
  # One of the following fields must be specified: digest, version, or tag
  digest: sha256:276b44be3f478b4c8d1f99c1925386d45a878a853f22436ece5589f32e9df384
  version: 0.2.0
  tag: latest
parameterSets:
  - myparams
credentialSets:
  - mycreds
parameters:
  log-level: 11
```

| Field             | Required | Description                                                                                                                  |
|-------------------|----------|------------------------------------------------------------------------------------------------------------------------------|
| schemaType        | false    | The type of document.                                                                                                        |
| schemaVersion     | true     | The version of the Installation schema used in this file.                                                                    |
| name              | true     | The name of the installation.                                                                                                |
| namespace         | false    | The namespace in which the installation is defined. Defaults to the empty (global) namespace.                                |
| uninstalled       | false    | Specifies if the installation should be uninstalled. Defaults to false.                                                      |
| labels            | false    | A set of key-value pairs associated with the installation.                                                                   |
| bundle            | true     | A reference to where the bundle is published                                                                                 |
| bundle.repository | true     | The repository where the bundle is published.                                                                                | 
| bundle.digest     | false*   | The bundle repository digest.                                                                                                |
| bundle.version    | false*   | The bundle version.                                                                                                          |
| bundle.tag        | false*   | The bundle tag. This is useful when you do not use Porter's convention of defaulting the bundle tag to the bundle version.   |
| parameterSets     | false    | A list of parameter set names.                                                                                               |
| credentialSets    | false    | A list of credential set names.                                                                                              |
| parameters        | false    | Additional parameter values to use with the installation. Overrides any parameters defined in the associated parameter sets. |

\* The bundle section requires a repository and one of the following fields: digest, version, or tag.

[inst-schema]: /schema/v1/installation.schema.json
