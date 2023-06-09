---
description: This article explains the purpose and history of PowerShellGet
ms.date: 06/09/2023
title: The PowerShellGet module
---
# The PowerShellGet module

The **PowerShellGet** module contains cmdlets for discovering, installing, updating, and publishing
PowerShell packages from the [PowerShell Gallery][01]. These packages can contain artifacts such as
Modules, DSC Resources, and Scripts.

Supported versions:

- Current release - **PowerShellGet** 2.2.5 with **PackageManagement** 1.4.8.1
- Preview release - **Microsoft.PowerShell.PSResourceGet** 0.5.0-beta22

## Version history

- **Windows PowerShell 5.1** comes with version 1.0.0.1 of **PowerShellGet** and
  **PackageManagement** preinstalled.

  > [!IMPORTANT]
  > The 1.0.0.1 version of PowerShellGet has limited features and must be updated to work properly
  > with the PowerShell Gallery. To be supported, you must update to the latest version. For upgrade
  > instructions, see [Installing PowerShellGet on Windows][03].

- **PowerShell 6.0.0** shipped with **PowerShellGet** 1.6.0 and **PackageManagement** 1.1.7.
- **PowerShell 7.0.0** shipped with **PowerShellGet** 2.2.3 and **PackageManagement** 1.4.6.
- **PowerShell 7.0.4**, **PowerShell 7.1.1**, and higher shipped with **PowerShellGet** 2.2.5 and
  **PackageManagement** 1.4.7.
- **PowerShell 7.4.0-preview.2** shipped with **PowerShellGet** 2.2.5 and **PackageManagement**
  1.4.8.1.
- Future versions of **PowerShell 7.4.0** will ship with **Microsoft.PowerShell.PSResourceGet**.

  > [!NOTE]
  > **Microsoft.PowerShell.PSResourceGet** is a standalone module and no longer depends on the
  > **PackageManagement** module.

For best results, use the latest version of the **Microsoft.PowerShell.PSResourceGet** module.

## See also

- [Install PowerShellGet][02]
- [PowerShellGet][04] cmdlet reference
- [Microsoft.PowerShell.PSResourceGet][05] cmdlet reference

<!-- link references -->
[01]: https://www.powershellgallery.com
[02]: install-powershellget.md
[03]: update-powershell-51.md
[04]: /powershell/module/powershellget
[05]: /powershell/module/microsoft.powershell.psresourceget
