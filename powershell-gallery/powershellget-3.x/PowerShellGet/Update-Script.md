---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: v3-beta22
ms.date: 06/09/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/update-script?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-Script
---

# Update-Script

## SYNOPSIS
Updates a script.

## SYNTAX

### All

```
Update-Script [[-Name] <String[]>] [-RequiredVersion <String>] [-MaximumVersion <String>]
 [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-Credential <PSCredential>] [-Force]
 [-AllowPrerelease] [-AcceptLicense] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

The `Update-Script` cmdlet updates a script that is installed on the local computer. The updated
script is downloaded from the same repository as the installed version.

This is a proxy cmdlet for the `Update-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Update-PSResource](../Microsoft.PowerShell.PSResourceGet/Update-PSResource.md).

## EXAMPLES

### Example 1: Update the specified script

This example updates an installed script and displays the updated version.

```powershell
Update-Script -Name UpdateManagement-Template -RequiredVersion 1.1
Get-InstalledScript -Name UpdateManagement-Template
```

```Output
Version   Name                       Repository   Description
-------   ----                       ----------   -----------
1.1       UpdateManagement-Template  PSGallery    This is a template script for Update Management...
```

`Update-Script` uses the **Name** parameter to specify the script to update. The **RequiredVersion**
parameter specifies the script version. `Get-InstalledScript` displays the updated version of the
script.

## PARAMETERS

### -AcceptLicense

Automatically accept the license agreement during installation if the package requires it.

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

### -AllowPrerelease

Allows you to update a script with the newer script marked as a prerelease.

The proxy cmdlet maps this parameter to the **Prerelease** parameter of `Update-PSResource`.

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

### -Credential

Specifies a user account that has permission to update a script.

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Force

Forces `Update-Script` to run without asking for user confirmation.

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
with the **Version** parameter of `Update-PSResource`.

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

Specifies one script name or an array of script names to update.

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

### -PassThru

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

### -Proxy

The proxy cmdlet ignores this parameter since it's not supported by `Update-PSResource`.

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProxyCredential

The proxy cmdlet ignores this parameter since it's not supported by `Update-PSResource`.

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -RequiredVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Update-PSResource`.

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

### -Confirm

Prompts you for confirmation before running `Update-Script`.

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

Shows what would happen if `Update-Script` runs. The cmdlet isn't run.

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

### System.String

### System.Uri

### System.Management.Automation.PSCredential

## OUTPUTS

### System.Object

## NOTES

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`

## RELATED LINKS

[Find-Script](Find-Script.md)

[Install-Script](Install-Script.md)

[Publish-Script](Publish-Script.md)

[Save-Script](Save-Script.md)

[Uninstall-Script](Uninstall-Script.md)
