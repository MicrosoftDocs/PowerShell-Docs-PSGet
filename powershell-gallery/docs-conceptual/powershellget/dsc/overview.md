---
description: >-
  Learn about the Microsoft Desired State Configuration (DSC) v3 resources that ship with
  Microsoft.PowerShell.PSResourceGet, what they can manage, and how DSC discovers them.
ms.date: 09/12/2026
ms.topic: overview
title: Manage PowerShell packages with Microsoft DSC
---
# Manage PowerShell packages with Microsoft DSC

Beginning with version 1.3.0-preview1, the **Microsoft.PowerShell.PSResourceGet** module ships two
[Microsoft Desired State Configuration (DSC)][01] v3 resources. You can use the resources to
declare which package repositories a machine should have registered and which PowerShell packages
should be installed from them. DSC then compares that declaration with the actual state of the
machine and installs, updates, or removes packages to match.

The resources are command-based DSC resources. They run in PowerShell 7 and call the same cmdlets
you use interactively, such as `Register-PSResourceRepository` and `Install-PSResource`.

## Available resources

| Resource type                                           | Manages                                                                        |
|:--------------------------------------------------------|:-------------------------------------------------------------------------------|
| [Microsoft.PowerShell.PSResourceGet/Repository][02]     | A registered package repository: name, URI, trust, priority, and API type      |
| [Microsoft.PowerShell.PSResourceGet/PSResourceList][03] | A list of packages that should, or shouldn't, be installed from one repository |

The **Repository** resource supports the `get`, `set`, `delete`, and `export` operations. The
**PSResourceList** resource supports the `get`, `set`, `test`, `export`, and `whatIf` operations.
For more information about what each operation does, see [DSC resource operations][04].

## Choose how to use the resources

You can use the resources in two ways:

- **Invoke a resource directly** with the `dsc resource` commands. Use this approach to inspect the
  state of a single repository or package list, to try a resource before adding it to a
  configuration, or to script a one-off change. For more information, see
  [Invoke the PSResourceGet DSC resources directly][05].
- **Declare the resources in a configuration document** and apply the document with the
  `dsc config` commands. Use this approach to describe the complete package state of a machine,
  combine the resources with other DSC resources, and preview changes with `--what-if`. For more
  information, see [Manage packages with a DSC configuration document][06].

Any tool that hosts DSC v3 can also use the resources. For example, you can reference the resource
types from a [WinGet configuration file][07] that uses the `dscv3` processor.

## Prerequisites

- **PowerShell 7.2 or later.** The resources run in `pwsh`, which must be discoverable through the
  `PATH` environment variable. Windows PowerShell 5.1 isn't supported.
- **Microsoft.PowerShell.PSResourceGet 1.3.0-preview1 or later.** Install the module from the
  PowerShell Gallery:

  ```powershell
  Install-PSResource -Name Microsoft.PowerShell.PSResourceGet -Prerelease
  ```

  For more information, see [Install a package manager for PowerShell][08].
- **Microsoft DSC 3.2 or later.** DSC 3.2 added the discovery extension that finds resources
  packaged in PowerShell modules. For installation instructions, see [Install DSC][09].

## How DSC discovers the resources

You don't need to add the module folder to the `PATH` environment variable. DSC ships an extension,
`Microsoft.PowerShell/Discover`, that searches the folders in the `PSModulePath` environment
variable for DSC resource manifests. Because `Install-PSResource` installs the module to a folder in
`PSModulePath`, DSC finds the resources as soon as you install the module.

The resource manifests and the script that implements the resources are stored in the root of the
module folder:

```Output
Microsoft.PowerShell.PSResourceGet/
└── 1.3.0/
    ├── Microsoft.PowerShell.PSResourceGet.psd1
    ├── psresourceget.ps1
    ├── psresourcelist.dsc.resource.json
    └── repository.dsc.resource.json
```

Verify that DSC can find the resources:

```powershell
dsc resource list Microsoft.PowerShell.PSResourceGet/*
```

```Output
Type                                               Kind      Version  Capabilities
----------------------------------------------------------------------------------
Microsoft.PowerShell.PSResourceGet/PSResourceList  Resource  0.0.1    gsw-t--e-
Microsoft.PowerShell.PSResourceGet/Repository      Resource  0.0.1    gs---d-e-
```

The **Capabilities** column shows `g` for `get`, `s` for `set`, `w` for `whatIf`, `t` for `test`,
`d` for `delete`, and `e` for `export`. The preceding output omits the **RequireAdapter** and
**Description** columns for readability.

If the command doesn't return the resources, check the following:

- **`pwsh` is discoverable through `PATH`.** DSC only runs the discovery extension when it can find
  PowerShell 7. It skips the extension without reporting an error.
- **The module is installed for PowerShell 7.** The extension ignores the Windows PowerShell module
  folders in `PSModulePath`. Run
  `Get-InstalledPSResource -Name Microsoft.PowerShell.PSResourceGet` in `pwsh` to confirm where the
  module is installed.
- **The installed version is 1.3.0-preview1 or later.** Earlier versions don't include the resource
  manifests.

To confirm that the extension is available, run `dsc extension list`. For more information, see
[dsc extension list][10].

> [!NOTE]
> DSC runs the resources from the module folder that contains the manifest it discovered, not from
> the version of **Microsoft.PowerShell.PSResourceGet** imported in your session. When more than one
> installed version ships the resources, DSC lists the resource type once and doesn't guarantee
> which version it selects. Uninstall the versions you don't want DSC to use.

## How the resources map to cmdlets

| Operation on the resource                | Cmdlets the resource calls                                                                     |
|:-----------------------------------------|:-----------------------------------------------------------------------------------------------|
| **Repository** get and export            | `Get-PSResourceRepository`                                                                     |
| **Repository** set                       | `Register-PSResourceRepository`, `Set-PSResourceRepository`, `Unregister-PSResourceRepository` |
| **Repository** delete                    | `Unregister-PSResourceRepository`                                                              |
| **PSResourceList** get, test, and export | `Get-PSResourceRepository`, `Get-InstalledPSResource`                                          |
| **PSResourceList** set                   | `Install-PSResource`, `Uninstall-PSResource`                                                   |

Because the resources call the cmdlets directly, they honor the same settings as an interactive
session. For example, a repository that requires credentials must have a persisted credential
configured before DSC can install from it. For more information, see
[How to add credentials to repositories with PSResourceGet][11].

## See also

- [Invoke the PSResourceGet DSC resources directly][05]
- [Manage packages with a DSC configuration document][06]
- [Microsoft.PowerShell.PSResourceGet/Repository][02]
- [Microsoft.PowerShell.PSResourceGet/PSResourceList][03]
- [What's new in PSResourceGet][12]

<!-- link references -->
[01]: /powershell/dsc/overview?view=dsc-3.0&preserve-view=true
[02]: reference/repository.md
[03]: reference/psresourcelist.md
[04]: /powershell/dsc/concepts/resources/operations?view=dsc-3.0&preserve-view=true
[05]: how-to/invoke-resources.md
[06]: how-to/configuration-documents.md
[07]: /windows/package-manager/configuration/
[08]: ../install-powershellget.md
[09]: /powershell/dsc/install?view=dsc-3.0&preserve-view=true
[10]: /powershell/dsc/reference/cli/extension/list?view=dsc-3.0&preserve-view=true
[11]: ../how-to/credential-persistence.md
[12]: ../psresourceget-release-notes.md
