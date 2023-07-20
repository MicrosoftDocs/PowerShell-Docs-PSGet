---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: v3-beta22
ms.date: 07/19/2023
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/get-psscriptfileinfo?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Get-PSScriptFileInfo

## SYNOPSIS

Returns the metadata for a script.

## SYNTAX

```
Get-PSScriptFileInfo [-Path] <String> [<CommonParameters>]
```

## DESCRIPTION

This cmdlet searches for a PowerShell script located on the machine and returns the script metadata
information.

## EXAMPLES

### Example 1

This example returns the metadata for the script `MyScript.ps1`.

```powershell
Get-PSScriptFileInfo -Path '.\Scripts\MyScript.ps1'
```

```Output
Name      Version Author              Description
----      ------- ------              -----------
MyScript  1.0.0.0 dev@microsoft.com   This script is a test script for PowerShellGet…
```

## PARAMETERS

### -Path

Specifies the path to the resource.

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

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see
[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### None

## OUTPUTS

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSScriptFileInfo

## NOTES

The `New-PSScriptFileInfo` and `Update-PSScriptFileInfo` cmdlets place the `#requires` statements
for required modules between the `<#PSScriptInfo` and comment-based help blocks of the help file.
The `Get-PSScriptFileInfo` expects `#requires` statements to be placed somewhere before the
comment-based help block. Any `#requires` statements placed after the comment-based help block are
ignored by `Get-PSScriptFileInfo` and `Publish-PSResource`.

## RELATED LINKS

[New-PSScriptFileInfo](New-PSScriptFileInfo.md)

[Update-PSScriptFileInfo](Update-PSScriptFileInfo.md)

[Test-PSScriptFileInfo](Test-PSScriptFileInfo.md)
