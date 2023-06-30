---
Download Help Link: https://go.microsoft.com/fwlink/?linkid=2238183
Help Version: 3.0.16
Locale: en-US
Module Guid: e4e0bda1-0703-44a5-b70d-8fe704cd0643
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: v3-beta22
ms.date: 06/30/2023
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/powershellget?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Microsoft.PowerShell.PSResourceGet
---
# Microsoft.PowerShell.PSResourceGet Module

## Description

PowerShellGet is a module with commands for discovering, installing, updating and publishing
PowerShell artifacts like Modules, DSC Resources, Role Capabilities, and Scripts.

This documentation covers the latest preview version **Microsoft.PowerShell.PSResourceGet**
v0.5.22-beta22. This version is included in PowerShell 7.4.0-preview.4 and newer releases.

> [!IMPORTANT]
> Windows PowerShell 5.1 comes with version 1.0.0.1 of **PowerShellGet** preinstalled. This version
> of PowerShellGet has a limited features and doesn't support the updated capabilities of the
> PowerShell Gallery. To be supported, you must update to the latest version.

## Microsoft.PowerShell.PSResourceGet Cmdlets

### [Find-PSResource](Find-PSResource.md)
Searches for packages from a repository (local or remote), based on a name or other package
properties.

### [Get-InstalledPSResource](Get-InstalledPSResource.md)
Returns modules and scripts installed on the machine via **PowerShellGet**.

### [Get-PSResourceRepository](Get-PSResourceRepository.md)
Finds and returns registered repository information.

### [Get-PSScriptFileInfo](Get-PSScriptFileInfo.md)
The cmdlet creates a new script file, including metadata about the script.

### [Install-PSResource](Install-PSResource.md)
Installs resources from a registered repository.

### [New-PSScriptFile](New-PSScriptFile.md)
The cmdlet creates a new script file, including metadata about the script.

### [Publish-PSResource](Publish-PSResource.md)
Publishes a specified module from the local computer to PSResource repository.

### [Register-PSResourceRepository](Register-PSResourceRepository.md)
Registers a repository for PowerShell resources.

### [Save-PSResource](Save-PSResource.md)
Saves resources (modules and scripts) from a registered repository onto the machine.

### [Set-PSResourceRepository](Set-PSResourceRepository.md)
Sets information for a registered repository.

### [Test-PSScriptFile](Test-PSScriptFile.md)
Tests the comment-based metadata in a `.ps1` file to ensure it's valid for publication.

### [Uninstall-PSResource](Uninstall-PSResource.md)
Uninstalls a resource that was installed using **PowerShellGet**.

### [Unregister-PSResourceRepository](Unregister-PSResourceRepository.md)
Removes a registered repository from the local machine.

### [Update-ModuleManifest](Update-ModuleManifest.md)
Updates a module manifest file.

### [Update-PSResource](Update-PSResource.md)
Downloads and installs the newest version of a package already installed on the local machine.

### [Update-PSScriptFileInfo](Update-PSScriptFileInfo.md)
This cmdlet updates the comment-based metadata in an existing script `.ps1` file.
