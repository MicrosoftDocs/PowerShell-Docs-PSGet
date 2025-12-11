---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/reset-psresourcerepository?view=powershellget-3.x&WT.mc_id=ps-gethelp
ms.date: 12/10/2025
schema: 2.0.0
ms.custom: 1.2.0-p5
title: Reset-PSResourceRepository
---
# Reset-PSResourceRepository

## SYNOPSIS

Creates a new default `PSRepositories.xml` file with PSGallery registered.

## SYNTAX

```
Reset-PSResourceRepository [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

This command resets the repository store by creating a store file with PSGallery registered. First
it creates a new temporary file and only replaces the old file if creation succeeds. If creation
fails, the old file is restored.

Use this command to replace a corrupted repository store or to restore the default repository
configuration.

This command was added in Microsoft.PowerShell.PSResourceGet v1.2.0-preview5.

## EXAMPLES

### Example 1 - Reset the repository store and display the results

```powershell
Reset-PSResourceRepository -PassThru
```

```Output
Name            Uri                                      Trusted Priority IsAllowedByPolicy
----            ---                                      ------- -------- -----------------
PSGallery       https://www.powershellgallery.com/api/v2 True    50       True
```

## PARAMETERS

### -PassThru

By default, the command does not generate any output. When specified, the command displays the
results of the reset operation.

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

### -Confirm

Prompts you for confirmation before running the cmdlet.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

Shows what would happen if the cmdlet runs. The cmdlet isn't run.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see
[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### None

## OUTPUTS

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSRepositoryInfo

## NOTES

## RELATED LINKS
