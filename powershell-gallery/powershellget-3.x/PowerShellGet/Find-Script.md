---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 3.0.22-beta22
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/find-script?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-Script
---
# Find-Script

## SYNOPSIS
Finds a script.

## SYNTAX

```
Find-Script [[-Name] <String[]>] [-MinimumVersion <String>] [-MaximumVersion <String>]
 [-RequiredVersion <String>] [-AllVersions] [-IncludeDependencies] [-Filter <String>] [-Tag <String[]>]
 [-Includes <String[]>] [-Command <String[]>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-Repository <String[]>] [-Credential <PSCredential>] [-AllowPrerelease] [<CommonParameters>]
```

## DESCRIPTION

The `Find-Script` cmdlet finds a specified script in registered repositories.

This is a proxy cmdlet for the `Find-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Find-PSResource](../Microsoft.PowerShell.PSResourceGet/Find-PSResource.md).

## EXAMPLES

### Example 1: Find a script by name

```powershell
Find-Script -Name "Start-WFContosoServer"
```

This command find the script named `Start-WFContosoServer`.

### Example 2: Find a script by name, required version, and from a specified repository

```powershell
Find-Script -Name "Required-Script2" -RequiredVersion 2.0 -Repository "LocalRepo01"
```

This command finds a script by name and required version in the LocalRepo01 repository.

### Example 3: Find a script in the specified version range

```
Find-Script -Name "Required-Script2" -MinimumVersion 2.1 -MaximumVersion 2.5 -Repository "LocalRepo1"
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Required-Script2                    Script     LocalRepo1           Description for the Required-Script2 script
```

This command finds all versions of RequiredScript2 between versions 2.1 and 2.5 in the LocalRepo1
respository.

### Example 4: Find all versions of a script

```powershell
Find-Script -Name "Required-Script02" -AllVersions
```

```Output
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
1.0        Required-Script2                    Script     LocalRepo1           Description for the Required-Script2 script
1.5        Required-Script2                    Script     LocalRepo1           Description for the Required-Script2 script
2.0        Required-Script2                    Script     LocalRepo1           Description for the Required-Script2 script
2.5        Required-Script2                    Script     LocalRepo1           Description for the Required-Script2 script
```

This command finds all versions of Required-Script02.

### Example 5: Find a script and its dependencies

```powershell
Find-Script -Name "Script-WithDependencies1" -IncludeDependencies -Repository "LocalRepo1"
```

```Output
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
1.0        Script-WithDependencies1            Script     LocalRepo1           Description for the Script-WithDependencies1 script
2.0        RequiredModule3                     Script     LocalRepo1           RequiredModule3 module
2.5        Required-Script1                    Script     LocalRepo1           Description for the Required-Script1 script
2.5        Required-Script2                    Script     LocalRepo1           Description for the Required-Script2 script
```

This command finds a script and its dependencies.

### Example 6: Find scripts with the specified tag

```powershell
Find-Script -Tag "Tag1" -Repository "LocalRepo1"
```

```Output
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
1.0        Fabrikam-ClientScript               Script     LocalRepo1           Description for the Fabrikam-ClientScript script
```

This command finds scripts that have the tag Tag1 in the LocalRepo1 repository

### Example 7: Find scripts with specified command name

```powershell
Find-Script -Command Test-FunctionFromScript_Required-Script3 -Repository "LocalRepo1"
```

```Output
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Required-Script3                    Script     LocalRepo1           Description for the Required-Script3 script
```

This command finds a script that contains the specified command name.

### Example 8: Find scripts using wildcards

```powershell
Find-Script -Name "Required-Script*" -Repository "LocalRepo1"
Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Required-Script1                    Script     local1               Description for the Required-Script1 script
2.5        Required-Script2                    Script     local1               Description for the Required-Script2 script
2.5        Required-Script3                    Script     local1               Description for the Required-Script3 script
```

This command uses the wildcard character (`*`) to find scripts that begin with Required-Script.

## PARAMETERS

### -AllowPrerelease

Includes in the results scripts marked as a prerelease.

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

Indicates that this operation finds all script versions.

The proxy cmdlet transforms this parameter to `-Version *` before calling `Find-PSResource`.

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

### -Command

The proxy cmdlet ignores this parameter since it's not supported by the **NameParameterSet** of
`Find-PSResource`.

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

### -Credential

Specifies a user account that has permission to access the repository to search for scripts.

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

### -Filter

The proxy cmdlet ignores this parameter since it's not supported by the **NameParameterSet** of
`Find-PSResource`.

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

### -IncludeDependencies

Indicates that this operation gets all scripts that are dependent upon the script specified in the
**Name** parameter.

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

### -Includes

The proxy cmdlet ignores this parameter since it's not supported by the **NameParameterSet** of
`Find-PSResource`.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:
Accepted values: Function, Workflow

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumVersion

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Find-PSResource`.

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
with the **Version** parameter of `Find-PSResource`.

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

Specifies an array of names of scripts to find.

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

### -Proxy

The proxy cmdlet ignores this parameter since it's not supported by the **NameParameterSet** of
`Find-PSResource`.

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

The proxy cmdlet ignores this parameter since it's not supported by the **NameParameterSet** of
`Find-PSResource`.

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

Specifies the friendly name of a repository that has been registered by running
`Register-PSRepository`.

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

The proxy cmdlet uses the value of this parameter to create a NuGet version search string for use
with the **Version** parameter of `Find-PSResource`.

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

### -Tag

Specifies an array of tags.

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

### System.String[]

### System.String

### System.Uri

### System.Management.Automation.PSCredential

## OUTPUTS

### PSRepositoryItemInfo

## NOTES

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`

## RELATED LINKS

[Install-Script](Install-Script.md)

[Publish-Script](Publish-Script.md)

[Save-Script](Save-Script.md)

[Uninstall-Script](Uninstall-Script.md)

[Update-Script](Update-Script.md)
