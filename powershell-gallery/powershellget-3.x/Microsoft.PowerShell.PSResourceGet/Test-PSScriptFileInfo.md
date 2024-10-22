---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.1.0-rc1
10/22/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/test-psscriptfileinfo?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Test-PSScriptFileInfo

## SYNOPSIS

Tests the comment-based metadata in a `.ps1` file to ensure it's valid for publication.

## SYNTAX

```
Test-PSScriptFileInfo [-Path] <String> [<CommonParameters>]
```

## DESCRIPTION

This cmdlet tests the comment-based metadata in a `.ps1` file to ensure it's valid for publication
to a repository.

## EXAMPLES

### Example 1: Test a valid script

This example creates a new script file then runs `Test-PSScriptFile` to validate the metadata
in the script.

```powershell
New-PSScriptFileInfo -Path "C:\MyScripts\test_script.ps1" -Description "this is a test script"
Test-PSScriptFileInfo -Path "C:\MyScripts\test_script.ps1"
True
```

### Example 2: Test an invalid script (missing Author)

This example runs the `Test-PSScriptFile` cmdlet against a script file. The script is missing
the required **Author** metadata. The cmdlet writes a warning message and returns `$false`.
`Get-Content` is used to view the contents of the script file.

```powershell
Test-PSScriptFileInfo -Path "C:\MyScripts\invalid_test_script.ps1"
Get-Content "C:\MyScripts\invalid_test_script.ps1"
```

```Output
WARNING: The .ps1 script file passed in wasn't valid due to: PSScript file is missing the required
Author property
False
<#PSScriptInfo

.VERSION 1.0.0.0

.GUID 7ec4832e-a4e1-562b-8a8c-241e535ad7d7

.AUTHOR

.COMPANYNAME

.COPYRIGHT

.TAGS

.LICENSEURI

.PROJECTURI

.ICONURI

.EXTERNALMODULEDEPENDENCIES

.REQUIREDSCRIPTS

.EXTERNALSCRIPTDEPENDENCIES

.RELEASENOTES

.PRIVATEDATA

#>

<#

.DESCRIPTION
this is a test script

#>
```

## PARAMETERS

### -Path

The path to `.ps1` script file.

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

### System.Boolean

## NOTES

The `New-PSScriptFileInfo` and `Update-PSScriptFileInfo` cmdlets place the `#requires` statements
for required modules between the `<#PSScriptInfo` and comment-based help blocks of the help file.
The `Get-PSScriptFileInfo` expects `#requires` statements to be placed somewhere before the
comment-based help block. Any `#requires` statements placed after the comment-based help block are
ignored by `Get-PSScriptFileInfo` and `Publish-PSResource`.

## RELATED LINKS

[Get-PSScriptFileInfo](Get-PSScriptFileInfo.md)

[New-PSScriptFileInfo](New-PSScriptFileInfo.md)

[Update-PSScriptFileInfo](Update-PSScriptFileInfo.md)
