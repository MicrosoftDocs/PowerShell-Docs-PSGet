---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.1.1
ms.date: 10/31/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/save-psresource?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Save-PSResource

## SYNOPSIS

Saves resources (modules and scripts) from a registered repository onto the machine.

## SYNTAX

### IncludeXmlParameterSet (Default)

```
Save-PSResource [-Name] <String[]> [-Version <String>] [-Prerelease] [-Repository <String[]>]
 [-Credential <PSCredential>] [-IncludeXml] [-Path <String>] [-TemporaryPath <String>]
 [-TrustRepository] [-PassThru] [-SkipDependencyCheck] [-AuthenticodeCheck] [-Quiet]
 [-AcceptLicense] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### AsNupkgParameterSet

```
Save-PSResource [-Name] <String[]> [-Version <String>] [-Prerelease] [-Repository <String[]>]
 [-Credential <PSCredential>] [-AsNupkg] [-Path <String>] [-TemporaryPath <String>]
 [-TrustRepository] [-PassThru] [-SkipDependencyCheck] [-AuthenticodeCheck] [-Quiet]
 [-AcceptLicense] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObjectParameterSet

```
Save-PSResource [-Repository <String[]>] [-Credential <PSCredential>] [-AsNupkg] [-IncludeXml]
 [-Path <String>] [-TemporaryPath <String>] [-TrustRepository] [-PassThru]
 [-InputObject] <PSResourceInfo[]> [-SkipDependencyCheck] [-AuthenticodeCheck] [-Quiet]
 [-AcceptLicense] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

This cmdlet combines the functionality of the `Save-Module` and `Save-Script` cmdlets from
**PowerShellGet** v2. `Save-PSResource` downloads a resource from a registered repository to a
specific path on the local machine. By default, the resource is saved in the unpacked or installed
format. The scripts or modules could be run from the saved location. There is also an option to
download the resource in `.nupkg` format.

## EXAMPLES

### Example 1

Downloads the **Az** module from the highest priority repository and saves it to the current
location.

```powershell
Save-PSResource -Name Az
```

### Example 2

Downloads the **Az** module from the PowerShell Gallery and saves it to the current location.

```powershell
Save-PSResource -Name Az -Repository PSGallery
```

### Example 3

Downloads the **Az** module from the highest priority repository and saves it in `.nupkg` format to
the current location.

```powershell
Save-PSResource Az -AsNupkg
```

### Example 4

Downloads the **Az** module from the highest priority repository and includes the **PowerShellGet**
XML metadata file.

```powershell
Save-PSResource Az -IncludeXML
```

## PARAMETERS

### -AcceptLicense

For modules that require a license, automatically accepts the license agreement during installation.

This parameter was added in PSResourceGet 1.1.0-rc1.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsNupkg

Saves the resource as a `.nupkg` file.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AsNupkgParameterSet, InputObjectParameterSet
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -AuthenticodeCheck

Validates the resource's signed files and catalog files on Windows.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

Optional credentials used when accessing a repository.

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IncludeXml

Includes the PowerShellGet metadata XML used to verify that PowerShellGet has installed a module.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: IncludeXmlParameterSet, InputObjectParameterSet
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

Used for pipeline input.

```yaml
Type: Microsoft.PowerShell.PSResourceGet.UtilClasses.PSResourceInfo[]
Parameter Sets: InputObjectParameterSet
Aliases: ParentResource

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Name

The name of one or more resources to install.

```yaml
Type: System.String[]
Parameter Sets: IncludeXmlParameterSet, AsNupkgParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

When specified, outputs a **PSResourceInfo** object for the saved resource.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

Specifies the path to save the resource to. If no path is provided, the resource is saved in the
current directory.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: current directory
Accept pipeline input: False
Accept wildcard characters: False
```

### -Prerelease

When specified, includes prerelease versions in search results returned.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: IncludeXmlParameterSet, AsNupkgParameterSet
Aliases: IsPrerelease

Required: False
Position: Named
Default value: False
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Quiet

Suppresses the progress bar output.

This parameter was added in PSResourceGet 1.1.0-rc1.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Repository

Specifies one or more repository names to search. If not specified, search includes all registered
repositories, in priority order (highest first), until a repository is found that contains the
package. Repositories are sorted by priority then by name. Lower **Priority** values have a higher
precedence.

When searching for resources across multiple repositories, the **PSResourceGet** cmdlets search the
repositories using this sort order. `Save-PSResource` saves the first matching package from the
sorted list of repositories.

The parameter supports the `*` wildcard character. If you specify multiple repositories, all names
must include or omit the wildcard character. You can't specify a mix of names with and without
wildcards.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -SkipDependencyCheck

Skips the check for resource dependencies. Only found resources are installed. No resources of the
found resource are installed.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -TemporaryPath

Specifies the path to temporarily install the resource before saving. If no temporary path is
provided, the resource is temporarily installed in the current user's temporary folder.

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

### -TrustRepository

Suppress prompts to trust repository. The prompt to trust repository only occurs if the repository
isn't configured as trusted.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Version

Specifies the version of the resource to be returned. The value can be an exact version or a version
range using the NuGet versioning syntax.

For more information about NuGet version ranges, see
[Package versioning](/nuget/concepts/package-versioning#version-ranges).

PowerShellGet supports all but the _minimum inclusive version_ listed in the NuGet version range
documentation. Using `1.0.0.0` as the version doesn't yield versions 1.0.0.0 and higher (minimum
inclusive range). Instead, the value is considered to be the required version. To search for a
minimum inclusive range, use `[1.0.0.0, ]` as the version range.

```yaml
Type: System.String
Parameter Sets: IncludeXmlParameterSet, AsNupkgParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Confirm

Prompts you for confirmation before running the cmdlet.

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

Shows what would happen if the cmdlet runs. The cmdlet isn't run.

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
-WarningAction, and -WarningVariable. For more information, see
[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.String[]

### System.String

### System.Management.Automation.SwitchParameter

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSResourceInfo[]

## OUTPUTS

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSResourceInfo

By default, the cmdlet doesn't return any objects. When the **PassThru** parameter is used, the
cmdlet outputs a **PSResourceInfo** object for the saved resource.

## NOTES

## RELATED LINKS
