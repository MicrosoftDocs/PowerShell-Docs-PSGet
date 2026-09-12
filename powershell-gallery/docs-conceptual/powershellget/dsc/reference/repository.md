---
description: Microsoft.PowerShell.PSResourceGet/Repository DSC resource reference documentation
ms.date: 09/12/2026
ms.topic: reference
title: Microsoft.PowerShell.PSResourceGet/Repository
---
# Microsoft.PowerShell.PSResourceGet/Repository

## Synopsis

Manage the package repositories registered for **Microsoft.PowerShell.PSResourceGet**.

## Metadata

```yaml
Version    : 0.0.1
Kind       : resource
Tags       : [linux, windows, macos, powershell, nuget]
Author     : Microsoft
```

## Instance definition syntax

```yaml
resources:
  - name: <instance name>
    type: Microsoft.PowerShell.PSResourceGet/Repository
    properties:
      # Required properties
      name: string
      uri: string # Required unless _exist is false
      # Instance properties
      trusted: boolean
      priority: integer
      repositoryType: Unknown | V2 | V3 | Local | NugetServer | ContainerRegistry
      _exist: boolean
```

## Description

The `Microsoft.PowerShell.PSResourceGet/Repository` resource enables you to idempotently manage
the repositories that **Microsoft.PowerShell.PSResourceGet** installs packages from. The resource
can:

- Register a repository that doesn't exist.
- Update the URI, trust setting, priority, or API type of an existing repository.
- Unregister a repository.
- Export every registered repository as a configuration document.

The resource wraps the `Get-PSResourceRepository`, `Register-PSResourceRepository`,
`Set-PSResourceRepository`, and `Unregister-PSResourceRepository` cmdlets. It manages the same
repository store those cmdlets use, so changes made with the resource are visible to interactive
sessions and the other way around.

> [!NOTE]
> This resource is installed with the **Microsoft.PowerShell.PSResourceGet** module. To use it, the
> folder containing the module must be discoverable by DSC. For more information, see
> [Make the resources discoverable][01].

## Requirements

- PowerShell 7.2 or later must be available as `pwsh` in the `PATH` environment variable.
- **Microsoft.PowerShell.PSResourceGet** 1.3.0-preview1 or later must be installed.
- The repository store is per user. The resource manages the repositories for the user account
  that runs `dsc`.

## Capabilities

The resource has the following capabilities:

- `get` - You can use the resource to retrieve the actual state of a repository.
- `set` - You can use the resource to enforce the desired state for a repository.
- `delete` - You can use the resource to unregister a repository.
- `export` - You can use the resource to enumerate every registered repository.

This resource uses the synthetic test functionality of DSC to determine whether an instance is in
the desired state. DSC compares each property you define in the desired state with the value the
resource returns from the **Get** operation. For more information about resource capabilities, see
[DSC resource capabilities][02].

> [!TIP]
> The resource returns the `uri` property in the normalized form that
> `Get-PSResourceRepository` reports. For example, a URI without a path gets a trailing slash. To
> avoid a synthetic test that reports the repository as out of the desired state, define `uri` in
> the desired state exactly as `Get-PSResourceRepository` returns it.

## Examples

1. [Invoke the PSResourceGet DSC resources directly][03] - Shows how to get, set, delete, and
   export repositories with the `dsc resource` commands.
1. [Manage packages with a DSC configuration document][04] - Shows how to register a repository
   and install packages from it in a single configuration document.

## Properties

The following list describes the properties for the resource.

- **Required properties:** <a id="required-properties"></a> The following properties are always
  required when defining an instance of the resource.

  - [name](#name) - The name of the repository.
  - [uri](#uri) - The location of the repository. Required unless `_exist` is `false`.

- **Instance properties:** <a id="instance-properties"></a> The following properties are optional.
  They define the desired state for an instance of the resource.

  - [trusted](#trusted) - Whether packages can be installed from the repository without a prompt.
  - [priority](#priority) - The search order of the repository relative to other repositories.
  - [repositoryType](#repositorytype) - The API type of the repository.
  - [_exist](#_exist) - Whether the repository should be registered.

### name

```yaml
Type       : string
IsRequired : true
IsKey      : true
IsReadOnly : false
```

Defines the name of the repository. The name identifies the repository in the repository store and
is the value you pass to the **Repository** parameter of cmdlets like `Install-PSResource`. The
name must be unique. The value is compared without regard to case.

### uri

```yaml
Type       : [string, 'null']
IsRequired : true (when _exist is true) / false (when _exist is false)
IsKey      : false
IsReadOnly : false
Format     : uri
```

Defines the location of the repository. The value can be an HTTPS URL, a file system path, or the
URL of a container registry. For more information about the repository types that
**Microsoft.PowerShell.PSResourceGet** supports, see [PSResourceGet supported repositories][05].

When `_exist` is `false`, this property is optional. The **Get** operation returns `null` for this
property when the repository isn't registered.

### trusted

```yaml
Type       : boolean
IsRequired : false
IsKey      : false
IsReadOnly : false
```

Defines whether the repository is trusted. When a repository is trusted, `Install-PSResource`
installs packages from it without prompting for confirmation. When you register a repository with
the resource and don't define this property, the repository is registered as untrusted.

The [Microsoft.PowerShell.PSResourceGet/PSResourceList][06] resource can only install packages
from an untrusted repository when its `trustedRepository` property is `true`.

### priority

```yaml
Type                  : integer
IsRequired            : false
IsKey                 : false
IsReadOnly            : false
InclusiveMinimumValue : 0
InclusiveMaximumValue : 100
```

Defines the priority of the repository. When a cmdlet searches more than one repository, it
searches repositories with a lower priority value first. When you register a repository with the
resource and don't define this property, the repository gets the default priority of `50`.

### repositoryType

```yaml
Type        : string
IsRequired  : false
IsKey       : false
IsReadOnly  : false
ValidValues : [Unknown, V2, V3, Local, NugetServer, ContainerRegistry]
```

Defines the API type of the repository. The value maps to the **ApiVersion** parameter of
`Register-PSResourceRepository` and `Set-PSResourceRepository`. When you don't define this
property, **Microsoft.PowerShell.PSResourceGet** detects the API type from the URI.

The following table describes the valid values.

| Value               | Description                                                            |
|:--------------------|:-----------------------------------------------------------------------|
| `V2`                | A NuGet v2 API feed, like the PowerShell Gallery.                      |
| `V3`                | A NuGet v3 API feed.                                                   |
| `Local`             | A folder on the file system or a network share.                        |
| `NugetServer`       | A NuGet.Server instance.                                               |
| `ContainerRegistry` | An OCI container registry, like Azure Container Registry or the Microsoft Artifact Registry. |
| `Unknown`           | Returned by the **Get** operation when the repository isn't registered. Don't use this value in the desired state. |

### _exist

```yaml
Type         : boolean
IsRequired   : false
IsKey        : false
IsReadOnly   : false
DefaultValue : true
```

The `_exist` canonical resource property determines whether the repository should be registered.
When the value is `true`, the resource registers the repository if it isn't registered and updates
it if it is. When the value is `false`, the resource unregisters the repository if it's
registered. The default value is `true`.

The **Get** operation returns `false` for this property when no repository with the specified
`name` is registered. In that case the resource returns `null` for `uri`, `false` for `trusted`,
`0` for `priority`, and `Unknown` for `repositoryType`.

## Instance validating schema

The following snippet contains the JSON Schema that validates an instance of the resource. The
validating schema only includes schema keywords that affect how the instance is validated. All
non-validating keywords are omitted.

```json
{
  "type": "object",
  "additionalProperties": false,
  "allOf": [
    {
      "if": {
        "properties": { "_exist": { "const": false } }
      },
      "then": { "required": ["name"] },
      "else": { "required": ["name", "uri"] }
    }
  ],
  "properties": {
    "name": { "type": "string" },
    "uri": { "type": ["string", "null"], "format": "uri" },
    "trusted": { "type": "boolean" },
    "priority": { "type": "integer", "minimum": 0, "maximum": 100 },
    "repositoryType": {
      "type": "string",
      "enum": ["Unknown", "V2", "V3", "Local", "NugetServer", "ContainerRegistry"]
    },
    "_exist": { "type": "boolean", "default": true }
  }
}
```

## Exit codes

The resource returns the following exit codes from operations:

- [0](#exit-code-0) - Success
- [1](#exit-code-1) - Error
- [12](#exit-code-12) - Unknown operation

### Exit code 0

Indicates the resource operation completed without errors.

### Exit code 1

Indicates the resource operation failed. The resource writes a JSON error message to stderr with
the details. Common causes include:

- The **Get** operation was invoked without input. The `name` property is required, so you must
  pass the desired state with the `--input` or `--file` option.
- The **Delete** operation was invoked with `_exist` set to `true`.
- A cmdlet raised a terminating error, for example when the value of `uri` isn't a valid URI.

### Exit code 12

Indicates the resource was invoked with an operation it doesn't recognize. This exit code doesn't
occur when you invoke the resource through DSC.

## See also

- [Microsoft.PowerShell.PSResourceGet/PSResourceList][06]
- [Manage PowerShell packages with Microsoft DSC][07]
- [Register-PSResourceRepository][08]
- [Set-PSResourceRepository][09]
- [PSResourceGet supported repositories][05]

<!-- link references -->
[01]: ../overview.md#make-the-resources-discoverable
[02]: /powershell/dsc/concepts/resources/capabilities?view=dsc-3.0&preserve-view=true
[03]: ../how-to/invoke-resources.md
[04]: ../how-to/configuration-documents.md
[05]: ../../supported-repositories.md
[06]: psresourcelist.md
[07]: ../overview.md
[08]: xref:Microsoft.PowerShell.PSResourceGet.Register-PSResourceRepository
[09]: xref:Microsoft.PowerShell.PSResourceGet.Set-PSResourceRepository
