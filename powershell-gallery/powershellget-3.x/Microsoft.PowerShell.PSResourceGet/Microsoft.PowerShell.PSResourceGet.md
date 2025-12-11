---
Download Help Link: https://go.microsoft.com/fwlink/?linkid=2238183
Help Version: 3.0.24260
Locale: en-US
Module Guid: e4e0bda1-0703-44a5-b70d-8fe704cd0643
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.2.0-p5
ms.date: 12/10/2025
schema: 2.0.0
title: Microsoft.PowerShell.PSResourceGet
---
# Microsoft.PowerShell.PSResourceGet Module

## Description

PSResourceGet is a module with commands for discovering, installing, updating and publishing
PowerShell artifacts like Modules, DSC Resources, Role Capabilities, and Scripts.

This documentation covers the latest version **Microsoft.PowerShell.PSResourceGet** v1.2.0-preview5.
For a list of differences between versions, see
[What's new in PSResourceGet?](/powershell/gallery/powershellget/psresourceget-release-notes).

> [!IMPORTANT]
> Windows PowerShell 5.1 comes with version 1.0.0.1 of **PowerShellGet** preinstalled. This version
> of PowerShellGet has a limited features and doesn't support the updated capabilities of the
> PowerShell Gallery. To install PSResourceGet, you must first update to the latest version of
> PowerShellGet. For more information, see
> [Update PowerShellGet for Windows PowerShell 5.1](/powershell/gallery/powershellget/update-powershell-51).

## Microsoft.PowerShell.PSResourceGet Cmdlets

### [Compress-PSResource](Compress-PSResource.md)

Compresses a specified folder containing module or script resources into a `.nupkg` file.

### [Find-PSResource](Find-PSResource.md)

Searches for packages from a repository (local or remote), based on a name or other package properties.

### [Get-InstalledPSResource](Get-InstalledPSResource.md)

Returns modules and scripts installed on the machine via **PowerShellGet**.

### [Get-PSResourceRepository](Get-PSResourceRepository.md)

Finds and returns registered repository information.

### [Get-PSScriptFileInfo](Get-PSScriptFileInfo.md)

Returns the metadata for a script.

### [Import-PSGetRepository](Import-PSGetRepository.md)

Finds the repositories registered with PowerShellGet and registers them for PSResourceGet.

### [Install-PSResource](Install-PSResource.md)

Installs resources from a registered repository.

### [New-PSScriptFileInfo](New-PSScriptFileInfo.md)

The cmdlet creates a new script file, including metadata about the script.

### [Publish-PSResource](Publish-PSResource.md)

Publishes a specified module from the local computer to PSResource repository.

### [Register-PSResourceRepository](Register-PSResourceRepository.md)

Registers a repository for PowerShell resources.

### [Reset-PSResourceRepository](Reset-PSResourceRepository.md)

Creates a new default `PSRepositories.xml` file with PSGallery registered.

### [Save-PSResource](Save-PSResource.md)

Saves resources (modules and scripts) from a registered repository onto the machine.

### [Set-PSResourceRepository](Set-PSResourceRepository.md)

Sets information for a registered repository.

### [Test-PSScriptFileInfo](Test-PSScriptFileInfo.md)

Tests the comment-based metadata in a `.ps1` file to ensure it's valid for publication.

### [Uninstall-PSResource](Uninstall-PSResource.md)

Uninstalls a resource that was installed using **PowerShellGet**.

### [Unregister-PSResourceRepository](Unregister-PSResourceRepository.md)

Removes a registered repository from the local machine.

### [Update-PSModuleManifest](Update-PSModuleManifest.md)

Updates a module manifest file.

### [Update-PSResource](Update-PSResource.md)

Downloads and installs the newest version of a package already installed on the local machine.

### [Update-PSScriptFileInfo](Update-PSScriptFileInfo.md)

This cmdlet updates the comment-based metadata in an existing script `.ps1` file.
