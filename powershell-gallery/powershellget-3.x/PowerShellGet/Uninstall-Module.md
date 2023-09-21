---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 3.0.22-beta22
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/uninstall-module?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Uninstall-Module
---

# Uninstall-Module

## SYNOPSIS
Uninstalls a module.

## SYNTAX

### NameParameterSet (Default)

```
Uninstall-Module [-Name] <String[]> [-MinimumVersion <String>] [-RequiredVersion <String>]
 [-MaximumVersion <String>] [-AllVersions] [-Force] [-AllowPrerelease] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### InputObject

```
Uninstall-Module [-InputObject] <PSObject[]> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

The `Uninstall-Module` cmdlet uninstalls a specified module from the local computer. You can't
uninstall a module if other modules depend on it or the module wasn't installed with the
`Install-Module` cmdlet.

This is a proxy cmdlet for the `Uninstall-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Uninstall-PSResource](../Microsoft.PowerShell.PSResourceGet/Uninstall-PSResource.md).

## EXAMPLES

### Example 1: Uninstall a module

This example uninstalls a module.

```powershell
Uninstall-Module -Name SpeculationControl
```

`Uninstall-Module` uses the **Name** parameter to specify the module to uninstall from the local
computer.

### Example 2: Use the pipeline to uninstall a module

In this example, the pipeline is used to uninstall a module.

```powershell
Get-InstalledModule -Name SpeculationControl | Uninstall-Module
```

`Get-InstalledModule` uses the **Name** parameter to specify the module. The object is sent down the
pipeline to `Uninstall-Module` and is uninstalled.

## PARAMETERS

### -AllowPrerelease

Allows you to uninstall a module marked as a prerelease.

The proxy cmdlet maps this parameter to the **Prerelease** parameter of `Uninstall-PSResource`

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AllVersions

Specifies that you want to include all available versions of a module. You can't use the
**AllVersions** parameter with the **MinimumVersion**, **MaximumVersion**, or **RequiredVersion**
parameters.

The proxy cmdlet transforms this parameter to `-Version *` before calling `Uninstall-PSResource`.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

The proxy cmdlet ignores this parameter since it's not supported by `Uninstall-PSResource`.

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

### -InputObject

Accepts a **PSRepositoryItemInfo** object. For example, output `Get-InstalledModule` to a variable
and use that variable as the **InputObject** argument.

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -MaximumVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Uninstall-PSResource`.

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MinimumVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Uninstall-PSResource`.

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

Specifies an array of module names to uninstall.

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -RequiredVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Uninstall-PSResource`.

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Confirm

Prompts you for confirmation before running the `Uninstall-Module`.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

Shows what would happen if `Uninstall-Module` runs. The cmdlet isn't run.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.String[]

### System.Management.Automation.PSObject[]

### System.String

## OUTPUTS

### System.Object

## NOTES

## RELATED LINKS

[Find-Module](Find-Module.md)

[Get-InstalledModule](Get-InstalledModule.md)

[Publish-Module](Publish-Module.md)

[Save-Module](Save-Module.md)

[Update-Module](Update-Module.md)

