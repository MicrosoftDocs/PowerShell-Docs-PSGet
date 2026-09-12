---
description: Microsoft.PowerShell.PSResourceGet/PSResourceList DSC resource reference documentation
ms.date: 09/12/2026
ms.topic: reference
title: Microsoft.PowerShell.PSResourceGet/PSResourceList
---
# Microsoft.PowerShell.PSResourceGet/PSResourceList

## Synopsis

Manage the PowerShell packages installed from a repository with
**Microsoft.PowerShell.PSResourceGet**.

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
    type: Microsoft.PowerShell.PSResourceGet/PSResourceList
    properties:
      # Required properties
      repositoryName: string
      # Instance properties
      trustedRepository: boolean
      resources:
        - name: string
          version: string
          scope: CurrentUser | AllUsers
          preRelease: boolean
          _exist: boolean
```

## Description

The `Microsoft.PowerShell.PSResourceGet/PSResourceList` resource enables you to idempotently
manage the packages installed from a single repository. An instance of the resource describes one
repository and the list of packages that should, or shouldn't, be installed from it. The resource
can:

- Report whether each package in the list is installed and which version is installed.
- Install packages that are missing or whose installed version doesn't satisfy the requested
  version.
- Uninstall packages that shouldn't be installed.
- Report what it would install or uninstall without changing the machine.
- Export the installed packages on the machine, grouped by repository.

The resource wraps the `Get-PSResource`, `Install-PSResource`, and `Uninstall-PSResource` cmdlets.
Packages are modules or scripts and are installed to the same locations the cmdlets use, so they
are available to every PowerShell session for the selected scope.

> [!NOTE]
> This resource is installed with the **Microsoft.PowerShell.PSResourceGet** module. To use it, the
> folder containing the module must be discoverable by DSC. For more information, see
> [Make the resources discoverable][01].

## Requirements

- PowerShell 7.2 or later must be available as `pwsh` in the `PATH` environment variable.
- **Microsoft.PowerShell.PSResourceGet** 1.3.0-preview1 or later must be installed.
- The repository named in `repositoryName` must be registered for the user that runs `dsc`. You
  can register it in the same configuration document with the
  [Microsoft.PowerShell.PSResourceGet/Repository][02] resource.
- To install packages with the `AllUsers` scope, `dsc` must run in an elevated process.
- Installing packages requires access to the repository. Private repositories must have a
  persisted credential configured. For more information, see
  [How to add credentials to repositories with PSResourceGet][03].

## Capabilities

The resource has the following capabilities:

- `get` - You can use the resource to retrieve the installed state of the packages in the list.
- `set` - You can use the resource to install and uninstall packages so that the machine matches
  the list.
- `whatIf` - The resource reports how it would change the machine during a **Set** operation in
  what-if mode.
- `test` - The resource implements its own test and reports whether every package in the list is
  in the desired state.
- `export` - You can use the resource to enumerate every installed package on the machine.

For more information about resource capabilities, see [DSC resource capabilities][04].

## Examples

1. [Invoke the PSResourceGet DSC resources directly][05] - Shows how to get, test, set, and export
   package lists with the `dsc resource` commands.
1. [Manage packages with a DSC configuration document][06] - Shows how to declare repositories and
   packages in a configuration document and preview changes with `--what-if`.

## Properties

The following list describes the properties for the resource.

- **Required properties:** <a id="required-properties"></a> The following properties are always
  required when defining an instance of the resource.

  - [repositoryName](#repositoryname) - The repository to install the packages from.

- **Instance properties:** <a id="instance-properties"></a> The following properties are optional.
  They define the desired state for an instance of the resource.

  - [trustedRepository](#trustedrepository) - Whether to install from the repository even when
    it isn't trusted.
  - [resources](#resources) - The list of packages to manage.

- **Read-only properties:** <a id="read-only-properties"></a> The resource returns the following
  properties, but they aren't configurable. For more information about read-only properties, see
  the "Read-only resource properties" section in [DSC resource properties][07].

  - [_inDesiredState](#_indesiredstate) - Whether the list is in the desired state.

### repositoryName

```yaml
Type       : string
IsRequired : true
IsKey      : true
IsReadOnly : false
```

Defines the name of the registered repository to install the packages from. The resource only
considers packages whose installation metadata records this repository. A package with the same
name that was installed from a different repository is reported as not installed.

When the repository isn't registered, the **Get** and **Test** operations report every package in
the list as not installed, and the **Set** operation fails with exit code `2`.

### trustedRepository

```yaml
Type         : boolean
IsRequired   : false
IsKey        : false
IsReadOnly   : false
DefaultValue : false
```

Defines whether the resource can install packages from the repository when the repository isn't
registered as trusted. When the value is `true`, the resource installs packages as if you passed
the **TrustRepository** parameter to `Install-PSResource`. When the value is `false` and the
repository isn't trusted, the **Set** operation fails with exit code `3` instead of installing
anything.

This property doesn't change the trust setting of the repository. To trust a repository
permanently, use the [Microsoft.PowerShell.PSResourceGet/Repository][02] resource.

### resources

```yaml
Type              : array
IsRequired        : false
IsKey             : false
IsReadOnly        : false
ItemsMinimumCount : 0
```

Defines the list of packages to manage. Each entry is an object that describes one package. The
**Get** and **Test** operations return one entry for every entry you define, in the same order.
The **Export** operation returns one entry for every installed package.

Each entry in `resources` has the following properties:

- [name](#name) - The name of the package.
- [version](#version) - The version or version range of the package.
- [scope](#scope) - Where the package is installed.
- [repositoryName](#resourcesrepositoryname) - The repository the package was installed from.
- [preRelease](#prerelease) - Whether to install prerelease versions.
- [_exist](#_exist) - Whether the package should be installed.
- [_metadata](#_metadata) - Messages returned in what-if mode.

#### name

```yaml
Type       : string
IsRequired : true
IsKey      : true
IsReadOnly : false
```

Defines the name of the package. The value is compared without regard to case. The same package
name can appear more than once in the list when each entry has a different `version` or
`preRelease` value, for example to install a stable and a prerelease version side by side.

#### version

```yaml
Type       : [string, 'null']
IsRequired : false
IsKey      : false
IsReadOnly : false
```

Defines the version or version range of the package, using the [NuGet version range syntax][08].
When you don't define this property, the resource installs the latest version and treats any
installed version as satisfying the desired state.

The resource uses the value in two ways:

- The **Get** and **Test** operations check whether an installed version _satisfies_ the value as
  a NuGet version range. A bare version like `2.0.0` is treated as the range `[2.0.0, )`, so any
  installed version equal to or newer than `2.0.0` satisfies it.
- The **Set** operation passes the value to the **Version** parameter of `Install-PSResource`. A
  bare version like `2.0.0` installs exactly that version.

To pin a package to an exact version for both comparison and installation, use the exact range
syntax `[2.0.0]`. The following table shows common values.

| Value            | Get and Test treat it as     | Set installs                   |
|:-----------------|:-----------------------------|:-------------------------------|
| _not defined_    | Any installed version        | The latest version             |
| `2.0.0`          | `2.0.0` or newer             | Exactly `2.0.0`                |
| `[2.0.0]`        | Exactly `2.0.0`              | Exactly `2.0.0`                |
| `[2.0.0, 3.0.0)` | `2.0.0` up to, not including, `3.0.0` | The newest version in the range |
| `[2.0.0, )`      | `2.0.0` or newer             | The newest version             |

When more than one version of the package is installed, the **Get** operation returns the version
that satisfies the range. When no installed version satisfies the range, the **Get** operation
returns the installed version it found with `_exist` set to `false`, so you can see which version
is on the machine. For prerelease versions, the returned value includes the prerelease label, like
`2.0.0-preview1`.

#### scope

```yaml
Type         : [string, 'null']
IsRequired   : false
IsKey        : false
IsReadOnly   : false
ValidValues  : [CurrentUser, AllUsers]
DefaultValue : CurrentUser
```

Defines the scope to install the package in. `CurrentUser` installs the package to the module or
script path for the current user. `AllUsers` installs the package to the shared path for every
user and requires an elevated process. The default value is `CurrentUser`.

The **Get** operation searches both scopes and returns the scope where it found the package.
Packages installed for the current user are found before packages installed for all users.

#### resources.repositoryName

```yaml
Type       : [string, 'null']
IsRequired : false
IsKey      : false
IsReadOnly : false
```

The name of the repository the package was installed from. The **Get** and **Export** operations
return this property for every installed package. You don't need to define it in the desired state.
When you do, the value must match the [repositoryName](#repositoryname) of the list, otherwise the
**Test** operation reports the package as out of the desired state.

#### preRelease

```yaml
Type         : boolean
IsRequired   : false
IsKey        : false
IsReadOnly   : false
DefaultValue : false
```

Defines whether the resource may install a prerelease version of the package. When the value is
`true`, the resource installs the package as if you passed the **Prerelease** parameter to
`Install-PSResource`. Combine this property with a `version` range that includes prerelease
versions, like `[3.0.0-preview1, )`, to install a specific prerelease.

The **Get** and **Export** operations return `true` for this property when the installed version
is a prerelease version.

#### _exist

```yaml
Type         : boolean
IsRequired   : false
IsKey        : false
IsReadOnly   : false
DefaultValue : true
```

The `_exist` canonical resource property determines whether the package should be installed. When
the value is `true`, the **Set** operation installs the package if it's missing or if the installed
version doesn't satisfy `version`. When the value is `false`, the **Set** operation uninstalls the
package if it's installed. The default value is `true`.

The **Get** operation returns `false` for this property when the package isn't installed from the
repository or when no installed version satisfies `version`.

#### _metadata

```yaml
Type       : object
IsRequired : false
IsKey      : false
IsReadOnly : true
```

This property is returned for entries the resource would change during a **Set** operation invoked
in what-if mode. For other operations, and for entries that are already in the desired state, the
return data doesn't include this property.

`_metadata` has the following properties:

- **whatIf** - An array of strings. Each string describes an action the resource would take, like
  `Would install resource 'PSScriptAnalyzer' version '1.24.0'` or
  `Would uninstall resource 'Pester'`. When the resource would install the latest version, the
  message reports the version as `latest`.

### _inDesiredState

```yaml
Type       : boolean
IsRequired : false
IsKey      : false
IsReadOnly : true
```

Returned by the **Test** operation. The value is `true` when every entry in `resources` is in the
desired state. An entry is in the desired state when its installed state matches `_exist`, the
installed version satisfies `version`, and the installed `scope` and `repositoryName` match the
values you defined.

The resource also returns this property for every entry in `resources`. Only the top-level value
is meaningful. The per-entry value is always `false`.

When the list is in the desired state, the `actualState` returned by the **Test** operation
contains the installed packages. When the list isn't in the desired state, the `actualState`
contains the entries you defined, with default values filled in, rather than the installed
packages. To see which versions are installed in that case, use the **Get** operation.

## Instance validating schema

The following snippet contains the JSON Schema that validates an instance of the resource. The
validating schema only includes schema keywords that affect how the instance is validated. All
non-validating keywords are omitted.

```json
{
  "type": "object",
  "additionalProperties": false,
  "required": ["repositoryName"],
  "properties": {
    "repositoryName": { "type": ["string", "null"] },
    "trustedRepository": { "type": "boolean", "default": false },
    "resources": {
      "type": "array",
      "minItems": 0,
      "items": {
        "type": "object",
        "additionalProperties": false,
        "required": ["name"],
        "properties": {
          "name": { "type": ["string", "null"] },
          "version": { "type": ["string", "null"] },
          "scope": { "type": ["string", "null"], "enum": ["CurrentUser", "AllUsers"] },
          "repositoryName": { "type": ["string", "null"] },
          "preRelease": { "type": "boolean", "default": false },
          "_exist": { "type": "boolean", "default": true },
          "_inDesiredState": { "type": "boolean", "default": true },
          "_metadata": {
            "type": "object",
            "readOnly": true,
            "additionalProperties": false,
            "properties": {
              "whatIf": { "type": "array", "items": { "type": "string" } }
            }
          }
        }
      }
    },
    "_inDesiredState": { "type": "boolean", "default": true }
  }
}
```

## Exit codes

The resource returns the following exit codes from operations:

- [0](#exit-code-0) - Success
- [1](#exit-code-1) - Error
- [2](#exit-code-2) - Repository not found
- [3](#exit-code-3) - Repository not trusted
- [4](#exit-code-4) - Could not install one or more packages
- [12](#exit-code-12) - Unknown operation

### Exit code 0

Indicates the resource operation completed without errors.

### Exit code 1

Indicates the resource operation failed with an unhandled error. The resource writes a JSON error
message to stderr with the details. Common causes include invalid input JSON or a cmdlet that
raised a terminating error, for example when `Uninstall-PSResource` can't remove a package
because another module depends on it.

### Exit code 2

Indicates the **Set** operation couldn't install packages because no repository with the name
defined in `repositoryName` is registered. Register the repository, for example with the
[Microsoft.PowerShell.PSResourceGet/Repository][02] resource, and retry the operation.

### Exit code 3

Indicates the **Set** operation couldn't install packages because the repository isn't trusted and
`trustedRepository` isn't `true`. Set `trustedRepository` to `true` in the desired state, or trust
the repository with the [Microsoft.PowerShell.PSResourceGet/Repository][02] resource.

### Exit code 4

Indicates the **Set** operation failed to install at least one package. The resource writes the
error from `Install-PSResource` to stderr. Common causes include a package name or version that
doesn't exist in the repository, a repository that requires credentials, and network errors. The
resource stops at the first package that fails to install. Packages that were uninstalled or
installed before the failure remain changed.

### Exit code 12

Indicates the resource was invoked with an operation it doesn't recognize. This exit code doesn't
occur when you invoke the resource through DSC.

## See also

- [Microsoft.PowerShell.PSResourceGet/Repository][02]
- [Manage PowerShell packages with Microsoft DSC][09]
- [Install-PSResource][10]
- [Uninstall-PSResource][11]
- [Get-PSResource][12]

<!-- link references -->
[01]: ../overview.md#make-the-resources-discoverable
[02]: repository.md
[03]: ../../how-to/credential-persistence.md
[04]: /powershell/dsc/concepts/resources/capabilities?view=dsc-3.0&preserve-view=true
[05]: ../how-to/invoke-resources.md
[06]: ../how-to/configuration-documents.md
[07]: /powershell/dsc/concepts/resources/properties?view=dsc-3.0&preserve-view=true#read-only-resource-properties
[08]: /nuget/concepts/package-versioning?tabs=semver20sort#version-ranges
[09]: ../overview.md
[10]: xref:Microsoft.PowerShell.PSResourceGet.Install-PSResource
[11]: xref:Microsoft.PowerShell.PSResourceGet.Uninstall-PSResource
[12]: xref:Microsoft.PowerShell.PSResourceGet.Get-PSResource
