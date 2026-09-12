---
description: >-
  Learn how to invoke the Microsoft.PowerShell.PSResourceGet DSC resources one at a time with the
  dsc resource commands to inspect, test, change, and export repositories and packages.
ms.date: 09/12/2026
ms.topic: how-to
title: Invoke the PSResourceGet DSC resources directly
---
# Invoke the PSResourceGet DSC resources directly

The `dsc resource` commands invoke a single DSC resource with the desired state you pass on the
command line. Use them to inspect what's on a machine, to check whether a machine already matches
what you want, or to make a change without writing a configuration document. This article shows
each operation the [Repository][01] and [PSResourceList][02] resources support.

## Prerequisites

- Complete the steps in [Make the resources discoverable][03] so that `dsc resource list`
  returns both resources.
- Run the commands in PowerShell 7. The examples build the input JSON with hashtables and
  `ConvertTo-Json`, which keeps the quoting readable.

## Inspect a resource

Use `dsc resource schema` to see the properties a resource accepts. The output is the JSON Schema
that DSC validates your input against.

```powershell
dsc resource schema --resource Microsoft.PowerShell.PSResourceGet/PSResourceList
```

## Work with repositories

### Get the state of a repository

Pass the name of the repository to the **Get** operation. The resource returns the registered
settings, or `_exist: false` when no repository with that name is registered.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/Repository'
$instance = @{ name = 'PSGallery' } | ConvertTo-Json -Compress

dsc resource get --resource $type --input $instance
```

```yaml
actualState:
  name: PSGallery
  uri: https://www.powershellgallery.com/api/v2
  trusted: false
  priority: 50
  repositoryType: V2
  _exist: true
```

> [!NOTE]
> The **Get** operation requires input. Running the command without `--input` or `--file` exits
> with a non-zero code and an error that explains the `name` property is required.

### Register or update a repository

The **Set** operation registers the repository if it doesn't exist and updates it if it does. Only
the properties you define are changed.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/Repository'
$instance = @{
    name           = 'ContosoModules'
    uri            = 'https://pkgs.contoso.com/nuget/v3/index.json'
    trusted        = $true
    priority       = 40
    repositoryType = 'V3'
} | ConvertTo-Json -Compress

dsc resource set --resource $type --input $instance
```

```yaml
beforeState:
  name: ContosoModules
  uri: null
  trusted: false
  priority: 0
  repositoryType: Unknown
  _exist: false
afterState:
  name: ContosoModules
  uri: https://pkgs.contoso.com/nuget/v3/index.json
  trusted: true
  priority: 40
  repositoryType: V3
  _exist: true
changedProperties:
- uri
- trusted
- priority
- repositoryType
- _exist
```

To change a single setting, such as trusting a repository you already registered, define `name`,
`uri`, and the property you want to change.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/Repository'
$instance = @{
    name    = 'PSGallery'
    uri     = 'https://www.powershellgallery.com/api/v2'
    trusted = $true
} | ConvertTo-Json -Compress

dsc resource set --resource $type --input $instance
```

### Unregister a repository

Use the **Delete** operation, or the **Set** operation with `_exist: false`, to unregister a
repository. Both operations succeed when the repository is already unregistered.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/Repository'
$instance = @{ name = 'ContosoModules'; _exist = $false } | ConvertTo-Json -Compress

dsc resource delete --resource $type --input $instance
```

### Export the registered repositories

The **Export** operation returns a configuration document that declares every registered
repository. Save the output to capture the repository setup of a machine so you can apply it to
another one.

```powershell
dsc resource export --resource Microsoft.PowerShell.PSResourceGet/Repository |
    Out-File -FilePath ./repositories.dsc.yaml
```

```yaml
$schema: https://aka.ms/dsc/schemas/v3/bundled/config/document.json
resources:
- name: Microsoft.PowerShell.PSResourceGet/Repository-0
  type: Microsoft.PowerShell.PSResourceGet/Repository
  properties:
    name: PSGallery
    uri: https://www.powershellgallery.com/api/v2
    trusted: false
    priority: 50
    repositoryType: V2
    _exist: true
- name: Microsoft.PowerShell.PSResourceGet/Repository-1
  type: Microsoft.PowerShell.PSResourceGet/Repository
  properties:
    name: MAR
    uri: https://mcr.microsoft.com
    trusted: true
    priority: 40
    repositoryType: ContainerRegistry
    _exist: true
```

## Work with packages

Every **PSResourceList** operation takes the name of one repository and a list of packages. The
examples use the PowerShell Gallery, which is registered as `PSGallery` by default.

### Get the installed state of packages

The **Get** operation returns one entry for each package you list, in the same order. Installed
packages are returned with their installed version and scope. Packages that aren't installed are
returned with `_exist: false`.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/PSResourceList'
$instance = @{
    repositoryName = 'PSGallery'
    resources      = @(
        @{ name = 'PSScriptAnalyzer' }
        @{ name = 'Pester'; version = '[5.0.0, )' }
        @{ name = 'Microsoft.PowerShell.PlatyPS' }
    )
} | ConvertTo-Json -Compress -Depth 3

dsc resource get --resource $type --input $instance
```

```yaml
actualState:
  repositoryName: PSGallery
  resources:
  - name: PSScriptAnalyzer
    version: 1.24.0
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: false
    _exist: true
  - name: Pester
    version: 5.7.1
    scope: AllUsers
    repositoryName: PSGallery
    preRelease: false
    _exist: true
  - name: Microsoft.PowerShell.PlatyPS
    version: null
    scope: CurrentUser
    repositoryName: null
    preRelease: false
    _exist: false
```

When a package is installed but no installed version satisfies the `version` you asked for, the
resource returns the version it found and sets `_exist` to `false`. That tells you the package
needs to be updated rather than installed. For more information about how the resource interprets
`version`, see the [version][04] property.

### Test whether packages are in the desired state

The **Test** operation compares the list with the installed packages and reports the result in
`inDesiredState`. It doesn't change the machine.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/PSResourceList'
$instance = @{
    repositoryName = 'PSGallery'
    resources      = @(
        @{ name = 'PSScriptAnalyzer'; version = '[1.24.0]' }
        @{ name = 'Pester'; version = '[5.0.0, )' }
    )
} | ConvertTo-Json -Compress -Depth 3

dsc resource test --resource $type --input $instance
```

```yaml
desiredState:
  repositoryName: PSGallery
  resources:
  - name: PSScriptAnalyzer
    version: '[1.24.0]'
  - name: Pester
    version: '[5.0.0, )'
actualState:
  repositoryName: PSGallery
  resources:
  - name: PSScriptAnalyzer
    version: 1.24.0
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: false
    _exist: true
    _inDesiredState: false
  - name: Pester
    version: 5.7.1
    scope: AllUsers
    repositoryName: PSGallery
    preRelease: false
    _exist: true
    _inDesiredState: false
  trustedRepository: false
  _inDesiredState: true
inDesiredState: true
differingProperties: []
```

Only the top-level `_inDesiredState` value is meaningful. The resource returns the property for
every entry as well, but always as `false`. When the list isn't in the desired state, the
`actualState` echoes the entries you defined rather than the installed packages. Use the **Get**
operation when you need the installed versions. For more information, see the
[_inDesiredState][08] property.

### Install packages

The **Set** operation installs every package in the list that isn't installed, or whose installed
version doesn't satisfy `version`. The PowerShell Gallery isn't trusted by default, so the example
sets `trustedRepository` to `true`. Without it, the operation fails with exit code `3`.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/PSResourceList'
$instance = @{
    repositoryName    = 'PSGallery'
    trustedRepository = $true
    resources         = @(
        @{ name = 'PSScriptAnalyzer'; version = '[1.24.0]' }
        @{ name = 'Microsoft.PowerShell.PlatyPS'; version = '[1.0.0, 2.0.0)' }
        @{ name = 'Microsoft.WinGet.Client'; preRelease = $true }
    )
} | ConvertTo-Json -Compress -Depth 3

dsc resource set --resource $type --input $instance
```

```yaml
beforeState:
  repositoryName: PSGallery
  resources:
  - name: PSScriptAnalyzer
    version: 1.24.0
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: false
    _exist: true
  - name: Microsoft.PowerShell.PlatyPS
    version: null
    scope: CurrentUser
    repositoryName: null
    preRelease: false
    _exist: false
  - name: Microsoft.WinGet.Client
    version: null
    scope: CurrentUser
    repositoryName: null
    preRelease: false
    _exist: false
afterState:
  repositoryName: PSGallery
  resources:
  - name: PSScriptAnalyzer
    version: 1.24.0
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: false
    _exist: true
  - name: Microsoft.PowerShell.PlatyPS
    version: 1.0.0
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: false
    _exist: true
  - name: Microsoft.WinGet.Client
    version: 1.11.400-beta
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: true
    _exist: true
changedProperties:
- resources
```

Packages that are already in the desired state, like `PSScriptAnalyzer` in the example, aren't
reinstalled. To install a package for every user on the machine, add `scope = 'AllUsers'` to its
entry and run `dsc` in an elevated process.

### Uninstall packages

Set `_exist` to `false` for a package to uninstall it. You can mix packages to install and packages
to uninstall in the same list.

```powershell
$type = 'Microsoft.PowerShell.PSResourceGet/PSResourceList'
$instance = @{
    repositoryName = 'PSGallery'
    resources      = @(
        @{ name = 'Microsoft.PowerShell.PlatyPS'; _exist = $false }
    )
} | ConvertTo-Json -Compress -Depth 3

dsc resource set --resource $type --input $instance
```

```yaml
beforeState:
  repositoryName: PSGallery
  resources:
  - name: Microsoft.PowerShell.PlatyPS
    version: 1.0.0
    scope: CurrentUser
    repositoryName: PSGallery
    preRelease: false
    _exist: true
afterState:
  repositoryName: PSGallery
  resources:
  - name: Microsoft.PowerShell.PlatyPS
    version: null
    scope: CurrentUser
    repositoryName: null
    preRelease: false
    _exist: false
changedProperties:
- resources
```

### Export the installed packages

The **Export** operation returns a configuration document with one **PSResourceList** instance for
each repository that packages were installed from. The export includes packages installed for the
current user and for all users.

```powershell
dsc resource export --resource Microsoft.PowerShell.PSResourceGet/PSResourceList |
    Out-File -FilePath ./packages.dsc.yaml
```

```yaml
$schema: https://aka.ms/dsc/schemas/v3/bundled/config/document.json
resources:
- name: Microsoft.PowerShell.PSResourceGet/PSResourceList-0
  type: Microsoft.PowerShell.PSResourceGet/PSResourceList
  properties:
    repositoryName: PSGallery
    resources:
    - name: PSScriptAnalyzer
      version: 1.24.0
      scope: CurrentUser
      repositoryName: PSGallery
      preRelease: false
      _exist: true
    - name: Pester
      version: 5.7.1
      scope: AllUsers
      repositoryName: PSGallery
      preRelease: false
      _exist: true
```

The exported versions are bare versions. Before you apply an exported document to another machine,
review the `version` values and decide whether to pin them with the `[<version>]` syntax or to
relax them to a range. For more information, see the [version][04] property.

## Troubleshoot

- **DSC reports that the resource type isn't found.** The module folder isn't on `PATH` or
  `DSC_RESOURCE_PATH` for the process that runs `dsc`. For more information, see
  [Make the resources discoverable][03].
- **The operation exits with code 2, 3, or 4.** The **PSResourceList** resource couldn't install
  packages. The exit code identifies the cause: the repository isn't registered, the repository
  isn't trusted, or `Install-PSResource` failed. For more information, see the
  [PSResourceList exit codes][05].
- **You need more detail about what the resource did.** Add `--trace-level debug` to the `dsc`
  command. The resource writes debug and trace messages for every step, including the cmdlets it
  calls and the version comparison it performs.

## See also

- [Manage packages with a DSC configuration document][06]
- [Manage PowerShell packages with Microsoft DSC][07]
- [dsc resource command reference][09]

<!-- link references -->
[01]: ../reference/repository.md
[02]: ../reference/psresourcelist.md
[03]: ../overview.md#make-the-resources-discoverable
[04]: ../reference/psresourcelist.md#version
[05]: ../reference/psresourcelist.md#exit-codes
[06]: configuration-documents.md
[07]: ../overview.md
[08]: ../reference/psresourcelist.md#_indesiredstate
[09]: /powershell/dsc/reference/cli/resource/index?view=dsc-3.0&preserve-view=true
