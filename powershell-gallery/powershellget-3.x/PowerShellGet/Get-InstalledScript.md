---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 3.0.22-beta22
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/get-installedscript?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-InstalledScript
---
# Get-InstalledScript

## SYNOPSIS
Gets an installed script.

## SYNTAX

```
Get-InstalledScript [[-Name] <String[]>] [-MinimumVersion <String>] [-RequiredVersion <String>]
 [-MaximumVersion <String>] [-AllowPrerelease] [<CommonParameters>]
```

## DESCRIPTION

The `Get-InstalledScript` cmdlet gets installed scripts for **CurrentUser** and **AllUsers** scopes.

This is a proxy cmdlet for the `Get-InstalledPSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Get-InstalledPSResource](../Microsoft.PowerShell.PSResourceGet/Get-InstalledPSResource.md).

## EXAMPLES

### Example 1: Get all installed scripts

```powershell
Get-InstalledScript
```

This command gets all installed scripts.

### Example 2: Get installed scripts by name

```powershell
Get-InstalledScript -Name "Required-Scri*"
```

```Output
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Required-Script1                    Script     local1               Description for the Required-Script1 script
2.5        Required-Script2                    Script     local1               Description for the Required-Script2 script
2.5        Required-Script3                    Script     local1               Description for the Required-Script3 script
```

This command gets scripts where the name begins with `Required-Scri`.

## PARAMETERS

### -AllowPrerelease

Includes in the results scripts marked as a prerelease.

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

Specifies an array of names of scripts to get. Wildcards are accepted.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
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
-WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.String[]

### System.String

## OUTPUTS

### System.Object

## NOTES

## RELATED LINKS

[Install-Script](Install-Script.md)

[Uninstall-Script](Uninstall-Script.md)
