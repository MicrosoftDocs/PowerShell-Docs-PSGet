---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 3.0.22-beta22
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/find-command?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-Command
---

# Find-Command

## SYNOPSIS
Finds PowerShell commands in modules.

## SYNTAX

### All

```
Find-Command [[-Name] <String[]>] [-ModuleName <String>] [-MinimumVersion <String>]
 [-MaximumVersion <String>] [-RequiredVersion <String>] [-AllVersions] [-AllowPrerelease]
 [-Tag <String[]>] [-Filter <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-Repository <String[]>] [<CommonParameters>]
```

## DESCRIPTION

The `Find-Command` cmdlet finds PowerShell commands such as cmdlets, aliases, functions, and
workflows. `Find-Command` searches modules in registered repositories.

This is a proxy cmdlet for the `Find-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Find-PSResource](../Microsoft.PowerShell.PSResourceGet/Find-PSResource.md).

## EXAMPLES

### Example 1: Find a command by name

`Find-Command` can use the name of a command to locate the module in a repository. It's possible
that a command name exists in multiple **ModuleNames**.

```powershell
Find-Command -Repository PSGallery -Name Get-TargetResource
```

```Output
Name                  Version    ModuleName                      Repository
----                  -------    ----------                      ----------
Get-TargetResource    3.1.0.0    xPowerShellExecutionPolicy      PSGallery
Get-TargetResource    1.0.0      xInternetExplorerHomePage       PSGallery
Get-TargetResource    1.2.0.0    SystemLocaleDsc                 PSGallery
```

`Find-Command` uses the **Repository** parameter to search the **PSGallery**. The **Name** parameter
specifies the command `Get-TargetResource`.

### Example 2: Find commands by name and install the module

`Find-Command` can locate the command and module, then send the object to `Install-Module`. If a
command is included in multiple modules, use the `Find-Command` cmdlets **ModuleName** parameter.
Otherwise, modules might be installed that you didn't want to install.

```powershell
Find-Command -Name Get-TargetResource -Repository PSGallery -ModuleName SystemLocaleDsc |
    Install-Module
Get-InstalledModule
```

```Output
Version   Name               Repository   Description
-------   ----               ----------   -----------
1.2.0.0   SystemLocaleDsc    PSGallery    This DSC Resource allows configuration of the Windows...
```

`Find-Command` uses the **Name** parameter to specify the command `Get-TargetResource`. The
**Repository** parameter searches the **PSGallery**. The **ModuleName** parameter specifies the
module you want to install, **SystemLocaleDsc**. The object is sent down the pipeline to
`Install-Module` and the module is installed. After the installation finishes, you can use
`Get-InstalledModule` to display the results.

### Example 3: Find a command and save its module

```powershell
Find-Command -Name Invoke-ScriptAnalyzer -Repository PSGallery |
    Save-Module -Path C:\Test\Modules -Verbose
```

```Output
VERBOSE: Downloading 'https://www.powershellgallery.com/api/v2/package/PSScriptAnalyzer/1.18.0'.
VERBOSE: Completed downloading 'https://www.powershellgallery.com/api/v2/package/PSScriptAnalyzer/1.18.0'.
VERBOSE: Completed downloading 'PSScriptAnalyzer'.
VERBOSE: Module 'PSScriptAnalyzer' was saved successfully to path 'C:\Test\Modules\PSScriptAnalyzer\1.18.0'.
```

`Find-Command` uses the **Name** and **Repository** parameters to search for the command
`Invoke-ScriptAnalyzer` in the **PSGallery** repository. The object is sent down the pipeline to
`Save-Module`. The **Path** parameter determines the location to save the module. **Verbose** is an
optional parameter, but displays status output in the PowerShell console. The verbose output is
beneficial for troubleshooting.

## PARAMETERS

### -AllowPrerelease

Includes modules marked as a prerelease in the results.

The proxy cmdlet maps this parameter to the **Prerelease** parameter of `Find-PSResource`.

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

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

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

### -Filter

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumVersion

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MinimumVersion

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModuleName

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

Specifies the command name to search for in a repository. Use commas to separate an array of command
names.

The proxy cmdlet maps this parameter to the **CommandName** parameter of `Find-PSResource`.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Proxy

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

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

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

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

### -Repository

Specifies the repository to search for commands. Use commas to separate an array of repository
names. The default is all repositories.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredVersion

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Tag

The proxy cmdlet ignores this parameter since it's not supported by the **CommandNameParameterSet**
of `Find-PSResource`.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

## OUTPUTS

### PSGetCommandInfo

`Find-Command` outputs a **PSGetCommandInfo** object.

## NOTES

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`

## RELATED LINKS

[Get-InstalledModule](Get-InstalledModule.md)

[Install-Module](Install-Module.md)

[Save-Module](Save-Module.md)

[Select-Object](xref:Microsoft.PowerShell.Utility.Select-Object)

[Uninstall-Module](Uninstall-Module.md)
