---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.1.0-rc2
ms.date: 10/31/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/compress-psresource?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Compress-PSResource

## SYNOPSIS

Compresses a specified folder containing module or script resources into a `.nupkg` file.

## SYNTAX

```
Compress-PSResource [-Path] <String> [-DestinationPath] <String> [-PassThru]
 [-SkipModuleManifestValidate] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

This cmdlet compresses a specified folder containing module or script resources into a `.nupkg`
file. isolates the pack feature in the `Publish-PSResource` cmdlet. This allows you to sign the
`.nupkg` file before publishing it to a repository. You can publish the final `.nupkg` file using
the **NupkgPath** parameter of `Publish-PSResource`.

This command was added in v1.1.0-preview2 of **Microsoft.PowerShell.PSResourceGet**.

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

Pass the full path of the nupkg through the pipeline.

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

Skips validating the module manifest before creating the `.nupkg` file.

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

### System.IO.FileSystemInfo

By default, this command doesn't write any output to the pipeline. When you use the **PassThru**
parameter, it returns a **FileSystemInfo** object for the new `.nupkg` file.

## NOTES

The module defines `cmres` as an alias for `Compress-PSResource`.

This cmdlet allows for publishing nuspec dependencies into ACR.

## RELATED LINKS

[Publish-PSResource](Publish-PSResource.md)
