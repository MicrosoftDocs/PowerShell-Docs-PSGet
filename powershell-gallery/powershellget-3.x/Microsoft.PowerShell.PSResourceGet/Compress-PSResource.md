---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.0.5
ms.date: 05/17/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/compress-psresource?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Compress-PSResource

## SYNOPSIS

Compresses a specified module from the local computer to a Nupkg ready to be signed and published.

## SYNTAX

```
Compress-PSResource [-Path] <String> [-DestinationPath] <String> [-PassThru] [-WhatIf]
 [-Confirm] [<CommonsParameters>]
```

## DESCRIPTION

This cmdlet isolates the pack feature in the Publish-PSResource cmdlet. This cmdlet takes the path
to a PowerShell Module and packs it into a nupkg file saved at the DestinationPath. This allows
users to sign the nupkg file. To publish the nupkg use the -NupkgPath parameter with
Publish-PSResource.

## EXAMPLES

### Example 1

This example compresses the module **TestModule** and saves te nupkg to the DestinationPath.

```powershell
Compress-PSResource -Path C:\TestModule -DestinationPath C:\NupkgDestination
```

## PARAMETERS

### -DestinationPath

Path to save the compressed resource.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

Pass the full path of the nupkg through the pipeline

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

### -Path

Path to the resource to be compressed.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipModuleManifestValidate

Skips validating the module manifest before publishing.

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

Shows what would happen if the cmdlet runs.
The cmdlet is not run.

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

### System.Object

## NOTES

The module defines `cmres` as an alias for `Compress-PSResource`.

This cmdlet allows for publishing nuspec dependencies into ACR

## RELATED LINKS
