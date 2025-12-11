---
description: This article explains the purpose and history of PowerShellGet
ms.date: 05/22/2025
ms.topic: overview
title: Package management for PowerShell
---
# Package management for PowerShell

Microsoft provides three package management tools for PowerShell:

- The **Microsoft.PowerShell.PSResourceGet** module - shipped originally in PowerShell 7.4.0
- The **PowerShellGet** and **PackageManagement** modules - shipped originally in Windows PowerShell
  5.0
- The **NuGet** module used by the Package Manager Console of Visual Studio

This documentation covers the **PowerShellGet**, **PackageManagement**, and
**Microsoft.PowerShell.PSResourceGet** modules. These modules contain cmdlets for discovering,
installing, updating, and publishing PowerShell packages from the [PowerShell Gallery][04]. These
packages can contain artifacts such as Modules, DSC Resources, and Scripts. The
**Microsoft.PowerShell.PSResourceGet** module replaces the **PowerShellGet** and
**PackageManagement** modules.

> [!NOTE]
> The **NuGet** module contains cmdlets for discovering and installing packages from the NuGet
> Gallery for use with Visual Studio projects. For information about the **NuGet** module, see the
> [NuGet module][01] reference in the Visual Studio documentation.

Supported versions:

- Current releases
  - **Microsoft.PowerShell.PSResourceGet** 1.1.1 - a standalone module that doesn't depend on the
    **PowerShellGet** or **PackageManagement** modules
  - **PowerShellGet** 2.2.5 with **PackageManagement** 1.4.8.1
- Preview releases
  - **Microsoft.PowerShell.PSResourceGet** 1.2.0-preview5 - adds many new features. For more
    information, see [What's new in PSResourceGet][06] in the GitHub repository.

## Version history

For best results, use the latest version of the **Microsoft.PowerShell.PSResourceGet** module.

- **Microsoft.PowerShell.PSResourceGet** v1.2.0-preview5 - shipped in **PowerShell 7.6-preview.6**
- **Microsoft.PowerShell.PSResourceGet** v1.1.1 - shipped in **PowerShell 7.6-preview.4**
- **Microsoft.PowerShell.PSResourceGet** v1.1.0 - shipped in **PowerShell 7.5.0**
- **Microsoft.PowerShell.PSResourceGet** 1.0.6 - released to the PowerShell Gallery on 10-Oct-2024
- **Microsoft.PowerShell.PSResourceGet** 1.0.5 - shipped in **PowerShell 7.5-preview.3**
- **Microsoft.PowerShell.PSResourceGet** 1.0.4.1 - shipped in **PowerShell 7.4.2**
- **Microsoft.PowerShell.PSResourceGet** 1.0.2 - released to the PowerShell Gallery on 06-Feb-2024
- **PowerShell 7.4.0** ships with **Microsoft.PowerShell.PSResourceGet** 1.0.1, **PowerShellGet**
  2.2.5, and **PackageManagement** 1.4.8.1
- **PowerShell 7.0.4**, **PowerShell 7.1.1**, and higher shipped with **PowerShellGet** 2.2.5 and
  **PackageManagement** 1.4.7.
- **PowerShell 7.0.0** shipped with **PowerShellGet** 2.2.3 and **PackageManagement** 1.4.6.
- **PowerShell 6.0.0** shipped with **PowerShellGet** 1.6.0 and **PackageManagement** 1.1.7.
- **Windows PowerShell 5.1** comes with version 1.0.0.1 of **PowerShellGet** and
  **PackageManagement** preinstalled.

  > [!IMPORTANT]
  > The 1.0.0.1 version of PowerShellGet has limited features and must be updated to work properly
  > with the PowerShell Gallery. To be supported, you must update to the latest version. For upgrade
  > instructions, see [Update PowerShellGet for Windows PowerShell 5.1][07].

## See also

- [Install PowerShellGet][05]
- [PowerShellGet][03] cmdlet reference
- [Microsoft.PowerShell.PSResourceGet][02] cmdlet reference

<!-- link references -->
[01]: /nuget/reference/powershell-reference
[02]: /powershell/module/microsoft.powershell.psresourceget
[03]: /powershell/module/powershellget
[04]: https://www.powershellgallery.com
[05]: install-powershellget.md
[06]: psresourceget-release-notes.md
[07]: update-powershell-51.md
