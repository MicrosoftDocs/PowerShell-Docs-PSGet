---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 2.9.0-preview
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/get-installedmodule?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-InstalledModule
---
# Get-InstalledModule

## SYNOPSIS
Gets a list of modules on the computer that were installed by PowerShellGet.

## SYNTAX

```
Get-InstalledModule [[-Name] <String[]>] [-MinimumVersion <String>] [-RequiredVersion <String>]
 [-MaximumVersion <String>] [-AllVersions] [-AllowPrerelease] [<CommonParameters>]
```

## DESCRIPTION

The `Get-InstalledModule` cmdlet gets PowerShell modules that are installed on a computer using
PowerShellGet. To see all modules installed on the system, use the `Get-Module -ListAvailable`
command.

This is a proxy cmdlet for the `Get-InstalledPSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Get-InstalledPSResource](../Microsoft.PowerShell.PSResourceGet/Get-InstalledPSResource.md).

## EXAMPLES

### Example 1: Get all installed modules

```powershell
Get-InstalledModule
```

```Output
Version    Name                                Type       Repository     Description
-------    ----                                ----       ----------     -----------
2.0.0      PSGTEST-UploadMultipleVersionOfP... Module     GalleryINT     Module for DAC functionality
1.3.5      AzureAutomationDebug                Module     PSGallery      Module for debugging Azure Automation runbooks, emulating AA native cmdlets
1.0.1      AzureRM.Automation                  Module     PSGallery      Microsoft Azure PowerShell - Automation service cmdlets for Azure Resource Manager
```

This command gets all installed modules.

### Example 2: Get specific versions of a module

```powershell
Get-InstalledModule -Name "AzureRM.Automation" -MinimumVersion 1.0 -MaximumVersion 2.0
```

```Output
Version    Name                                Type       Repository     Description
-------    ----                                ----       ----------     -----------
1.0.1      AzureRM.Automation                  Module     PSGallery      Microsoft Azure PowerShell - Automation service cmdlets for Azure Resource Manager
```

This command gets versions of the AzureRM.Automation module from version 1.0 through version 2.0.

## PARAMETERS

### -AllowPrerelease

Includes in the results modules marked as a prerelease.

The proxy cmdlet maps this parameter to the **Prerelease** parameter of `Get-InstalledPSResource`.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AllVersions

The proxy cmdlet transforms this parameter to `-Version *` before calling `Get-InstalledPSResource`.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Get-InstalledPSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MinimumVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Get-InstalledPSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

Specifies an array of names of modules to get.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -RequiredVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Get-InstalledPSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](/powershell/module/Microsoft.PowerShell.Core/About/about_CommonParameters).

## INPUTS

### System.String[]

### System.String

## OUTPUTS

### System.Management.Automation.PSCustomObject

## NOTES

## RELATED LINKS

