---
Download Help Link: https://aka.ms/powershell74-help
Help Version: 7.3.0.0
Locale: en-US
Module Guid: 1d73a601-4a6c-43c5-ba3f-619b18bbb404
Module Name: PowerShellGet
ms.custom: v3-beta22
ms.date: 06/09/2023
schema: 2.0.0
title: PowerShellGet
---
# PowerShellGet Module

## Description

This documentation covers version 3.0.0-beta22 of the **PowerShellGet** module. This module is
provided for compatibility with **PowerShellGet** v2.x. The cmdlets in this version of the module
are proxy cmdlets that call the equivalent cmdlets in the **Microsoft.PowerShell.PSResourceGet**
module.

The proxy cmdlets provide a compatibility layer for scripts that use the version 2.x cmdlets. In
most cases, the scripts continue to work without modification. However, there are some differences
in behavior between the version 2.x cmdlets and the version 3.x cmdlets. Some parameters for the
version 2.x cmdlets are not supported by the version 3.x cmdlets. The proxy cmdlets silently discard
unsupported parameters, transform some parameters, and pass other parameters through to the
equivalent cmdlets in the **Microsoft.PowerShell.PSResourceGet** module.

For more information about the **Microsoft.PowerShell.PSResourceGet** module, see
[about_PSResourceGet](../microsoft.powershell.psresourceget/about/about_psresourceget.md).

> [!IMPORTANT]
> Windows PowerShell 5.1 comes with version 1.0.0.1 of **PowerShellGet** preinstalled. This version
> of PowerShellGet has a limited features and doesn't support the updated capabilities of the
> PowerShell Gallery. To be supported, you must update to the latest version.

## PowerShellGet Cmdlets

### [Find-Command](Find-Command.md)
Finds PowerShell commands in modules.

### [Find-DscResource](Find-DscResource.md)
Finds a DSC resource.

### [Find-Module](Find-Module.md)
Finds modules in a repository that match specified criteria.

### [Find-RoleCapability](Find-RoleCapability.md)
Finds role capabilities in modules.

### [Find-Script](Find-Script.md)
Finds a script.

### [Get-InstalledModule](Get-InstalledModule.md)
Gets a list of modules on the computer that were installed by PowerShellGet.

### [Get-InstalledScript](Get-InstalledScript.md)
Gets an installed script.

### [Get-PSRepository](Get-PSRepository.md)
Gets PowerShell repositories.

### [Install-Module](Install-Module.md)
Downloads one or more modules from a repository, and installs them on the local computer.

### [Install-Script](Install-Script.md)
Installs a script.

### [New-ScriptFileInfo](New-ScriptFileInfo.md)
Creates a script file with metadata.

### [Publish-Module](Publish-Module.md)
Publishes a specified module from the local computer to an online gallery.

### [Publish-Script](Publish-Script.md)
Publishes a script.

### [Register-PSRepository](Register-PSRepository.md)
Registers a PowerShell repository.

### [Save-Module](Save-Module.md)
Saves a module and its dependencies on the local computer but doesn't install the module.

### [Save-Script](Save-Script.md)
Saves a script.

### [Set-PSRepository](Set-PSRepository.md)
Sets values for a registered repository.

### [Test-ScriptFileInfo](Test-ScriptFileInfo.md)
Validates a comment block for a script.

### [Uninstall-Module](Uninstall-Module.md)
Uninstalls a module.

### [Uninstall-Script](Uninstall-Script.md)
Uninstalls a script.

### [Unregister-PSRepository](Unregister-PSRepository.md)
Unregisters a repository.

### [Update-Module](Update-Module.md)
Downloads and installs the newest version of specified modules from an online gallery to the local computer.

### [Update-ModuleManifest](Update-ModuleManifest.md)
Updates a module manifest file.

### [Update-Script](Update-Script.md)
Updates a script.

### [Update-ScriptFileInfo](Update-ScriptFileInfo.md)
Updates information for a script.
