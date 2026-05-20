---
description: This article explains the purpose and history of PowerShellGet.
ms.date: 05/20/2026
ms.topic: overview
title: Package management for PowerShell
---
# Package management for PowerShell

Microsoft provides three package management tools for PowerShell:

- The Microsoft.PowerShell.PSResourceGet module - shipped originally in PowerShell 7.4.0
- The PowerShellGet and PackageManagement modules - shipped originally in Windows PowerShell
  5.0
- The NuGet module used by the Package Manager Console of Visual Studio

This documentation covers the PowerShellGet, PackageManagement, and
Microsoft.PowerShell.PSResourceGet modules. These modules contain cmdlets for discovering,
installing, updating, and publishing PowerShell packages from the [PowerShell Gallery][04]. These
packages can contain artifacts such as Modules, DSC Resources, and Scripts. The
Microsoft.PowerShell.PSResourceGet module replaces the PowerShellGet and PackageManagement modules.

> [!NOTE]
> The NuGet module contains cmdlets for discovering and installing packages from the NuGet Gallery
> for use with Visual Studio projects. For information about the NuGet module, see the
> [NuGet module][01] reference in the Visual Studio documentation.

Supported versions:

- Current releases
  - Microsoft.PowerShell.PSResourceGet 1.2.0 - a standalone module that doesn't depend on the
    PowerShellGet or PackageManagement modules
  - PowerShellGet 2.2.5 with PackageManagement 1.4.8.1
- Preview releases
  - Microsoft.PowerShell.PSResourceGet 1.3.0-preview1 - adds many new features. For more
    information, see [What's new in PSResourceGet][06] in the GitHub repository.

For best results, use the latest version of the Microsoft.PowerShell.PSResourceGet module.

> [!IMPORTANT]
> The 1.0.0.1 version of PowerShellGet that ships in Windows PowerShell 5.1 is no longer supported.
> To be supported, you must update to the latest version. For more information, see
> [Install a package manager for PowerShell][05].

## Enhanced support for the Microsoft Artifact Registry

Support for the Microsoft Artifact Registry was added in Microsoft.PowerShell.PSResourceGet v1.1.0.
Beginning with Microsoft.PowerShell.PSResourceGet v1.3.0-preview1, the Microsoft Artifact Registry
is a default repository alongside the PSGallery repository. Use the following command to register
the Microsoft Artifact Registry repository with the default settings:

```powershell
Register-PSResourceRepository -MicrosoftArtifactRegistry
```

By default, the Microsoft Artifact Registry repository is registered as a Trusted repository with a
higher priority than the PSGallery repository. For more information, see
[Register-PSResourceRepository][07].

## See also

- [Install a package manager for PowerShell][05]
- [PowerShellGet][03] cmdlet reference
- [Microsoft.PowerShell.PSResourceGet][02] cmdlet reference

<!-- link references -->
[01]: /nuget/reference/powershell-reference
[02]: /powershell/module/microsoft.powershell.psresourceget
[03]: /powershell/module/powershellget
[04]: https://www.powershellgallery.com
[05]: install-powershellget.md
[06]: psresourceget-release-notes.md
[07]: xref:Microsoft.PowerShell.PSResourceGet.Register-PSResourceRepository
