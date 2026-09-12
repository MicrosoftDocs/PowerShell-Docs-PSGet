---
description: >-
  Learn about the Microsoft Desired State Configuration (DSC) v3 resources that ship with
  Microsoft.PowerShell.PSResourceGet, what they can manage, and how to make them available to DSC.
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

| Resource type                                    | Manages                                                       |
|:-------------------------------------------------|:--------------------------------------------------------------|
| [Microsoft.PowerShell.PSResourceGet/Repository][02]     | A registered package repository: name, URI, trust, priority, and API type |
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
- **Microsoft DSC 3.0 or later.** For installation instructions, see [Install DSC][09].

## Make the resources discoverable

DSC discovers command-based resources by searching the folders in the `PATH` environment variable,
or in the `DSC_RESOURCE_PATH` environment variable when it's defined, for files with the
`.dsc.resource.json` suffix. The resource manifests and the script that implements the resources
are stored in the root of the module folder:

```Output
Microsoft.PowerShell.PSResourceGet/
└── 1.3.0/
    ├── Microsoft.PowerShell.PSResourceGet.psd1
    ├── psresourceget.ps1
    ├── psresourcelist.dsc.resource.json
    └── repository.dsc.resource.json
```

Add the folder for the installed module version to `PATH` so that DSC can find the manifests. The
following commands add the newest installed version for the current session:

```powershell
$module = Get-Module -Name Microsoft.PowerShell.PSResourceGet -ListAvailable |
    Sort-Object -Property Version -Descending |
    Select-Object -First 1
$env:PATH += [System.IO.Path]::PathSeparator + $module.ModuleBase
```

To make the change permanent, add the folder to the `PATH` environment variable for your user or
the machine. Alternatively, set the `DSC_RESOURCE_PATH` environment variable to the module folder.
When `DSC_RESOURCE_PATH` is defined, DSC only searches the folders it lists. For more information,
see [Environment variables][10] in the `dsc` command reference.

Verify that DSC can find the resources:

```powershell
dsc resource list Microsoft.PowerShell.PSResourceGet/*
```

```Output
Type                                               Kind      Version  Capabilities
----------------------------------------------------------------------------------
Microsoft.PowerShell.PSResourceGet/PSResourceList  Resource  0.0.1    gs-wt-e-
Microsoft.PowerShell.PSResourceGet/Repository      Resource  0.0.1    gs---de-
```

The **Capabilities** column shows `g` for `get`, `s` for `set`, `w` for `whatIf`, `t` for `test`,
`d` for `delete`, and `e` for `export`. The table view omits the description column for
readability.

> [!NOTE]
> The resources use the version of **Microsoft.PowerShell.PSResourceGet** stored in the same folder
> as the resource manifests, even when a different version of the module is already imported in
> your session. Keep the folder on `PATH` pointed at the version you want DSC to use.

## How the resources map to cmdlets

| Operation on the resource                  | Cmdlets the resource calls                                                     |
|:-------------------------------------------|:-------------------------------------------------------------------------------|
| **Repository** get and export              | `Get-PSResourceRepository`                                                     |
| **Repository** set                         | `Register-PSResourceRepository`, `Set-PSResourceRepository`, `Unregister-PSResourceRepository` |
| **Repository** delete                      | `Unregister-PSResourceRepository`                                              |
| **PSResourceList** get, test, and export   | `Get-PSResourceRepository`, `Get-PSResource`                                   |
| **PSResourceList** set                     | `Install-PSResource`, `Uninstall-PSResource`                                   |

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
[10]: /powershell/dsc/reference/cli/index?view=dsc-3.0&preserve-view=true#environment-variables
[11]: ../how-to/credential-persistence.md
[12]: ../psresourceget-release-notes.md
