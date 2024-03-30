---
description: The PowerShellGet module provides commands to interact with the PowerShell Gallery, the central repository for PowerShell modules, scripts, and DSC resources.
ms.date: 11/16/2022
ms.topic: overview
title: PowerShellGet and the PowerShell Gallery
---
# PowerShellGet and the PowerShell Gallery

The [PowerShell Gallery][06] is the central repository for PowerShell content. In it, you can find
PowerShell scripts, modules containing PowerShell cmdlets and Desired State Configuration (DSC)
resources. Some of these packages are authored by Microsoft, and others are authored by the
PowerShell community.

The [PowerShellGet][07] module contains cmdlets for discovering, installing, updating, and publishing
PowerShell packages from the PowerShell Gallery. These packages can contain artifacts such as
Modules, DSC Resources, Role Capabilities, and Scripts. Make sure you have the latest version of
**PowerShellGet** and **PackageManagement** installed.

The [Microsoft.PowerShell.PSResourceGet][08] module replaces the **PowerShellGet** and
**PackageManagement** modules. **Microsoft.PowerShell.PSResourceGet** version 1.0.1 ships in
PowerShell 7.4.0. The latest version is available in the PowerShell Gallery and can be installed on
any supported version of PowerShell.

For more information see:

- [Install PowerShellGet][01]
- [Update PowerShellGet for Windows PowerShell][02]

## Getting Started with the PowerShell Gallery

Check out the [Getting Started][03] page for more information on how to use **PowerShellGet**
commands with the Gallery. You can also run `Update-Help -Module PowerShellGet -Force` to install
local help for these commands.

## Giving feedback

Provide feedback and report issues in the [GitHub repository][04]. To see the current status of the
PowerShell Gallery services, see the [PowerShell Gallery Status][05] page on GitHub.

<!-- link references -->
[01]: powershellget/install-powershellget.md
[02]: powershellget/update-powershell-51.md
[03]: getting-started.md
[04]: https://aka.ms/psgallery-issues
[05]: https://aka.ms/psgallery-status
[06]: https://www.powershellgallery.com
[07]: /powershell/module/powershellget
[08]: /powershell/module/microsoft.powershell.psresourceget

