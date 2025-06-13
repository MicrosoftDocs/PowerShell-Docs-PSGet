---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.1.1
ms.date: 10/31/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/update-psresource?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Update-PSResource

## SYNOPSIS

Downloads and installs the newest version of a package already installed on the local machine.

## SYNTAX

```
Update-PSResource [[-Name] <String[]>] [-Version <String>] [-Prerelease] [-Repository <String[]>]
 [-Scope <ScopeType>] [-TemporaryPath <String>] [-TrustRepository] [-Credential <PSCredential>]
 [-Quiet] [-AcceptLicense] [-Force] [-PassThru] [-SkipDependencyCheck] [-AuthenticodeCheck]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

`Update-PSResource` downloads and installs the newest version of a package already installed on the
local machine. This cmdlet replaces the `Update-Module` and `Update-Script` cmdlets from
**PowerShellGet** v2. The new version of the resource is installed side-by-side with previous
versions in a new versioned folder.

By default, `Update-PSResource` installs the latest version of the package and any of its
dependencies without deleting the older versions installed. There is no command to uninstall older
versions of a package. You must manually delete the files and folders of the older versions.

`Update-PSResource` doesn't load the updated module into the current session. You must import the
new version or start a new session to use the updated module. For more information, see
[Import-Module](xref:Microsoft.PowerShell.Core.Import-Module).

## EXAMPLES

### Example 1

In this example, the user already has the **TestModule** package installed and they update the
package.

```powershell
Get-PSResource -Name "TestModule"
```

```Output
Name                                    Version                         Prerelease   Description
----                                    -------                         ----------   -----------
TestModule                              1.2.0                                        test
```

```powershell
Update-PSResource -Name "TestModule"
```

```Output
Name                                    Version                         Prerelease   Description
----                                    -------                         ----------   -----------
TestModule                              1.3.0                                        test
TestModule                              1.2.0                                        test
```

## PARAMETERS

### -AcceptLicense

For resources that require a license, **AcceptLicense** automatically accepts the license agreement
during the update.

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

### -AuthenticodeCheck

Validates signed files and catalog files on Windows.

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

Specifies optional credentials used when accessing a private repository.

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

### -Force

When specified, bypasses checks for **TrustRepository** and **AcceptLicense** and updates the
package.

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

### -Name

Specifies the name of one or more resources to update. Wildcards are supported but NuGet only
accepts the `*` character. NuGet doesn't support wildcard searches of local (file-based)
repositories.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: "*"
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
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

### -Prerelease

When specified, allows updating to a prerelease version.

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

### -Quiet

Suppresses progress information.

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
repositories using this sort order. `Update-PSResource` updates the first matching package from the
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
Accept pipeline input: False
Accept wildcard characters: True
```

### -Scope

Specifies the installation scope. Accepted values are:

- `CurrentUser`
- `AllUsers`

The default scope is `CurrentUser`, which doesn't require elevation.

```yaml
Type: Microsoft.PowerShell.PSResourceGet.UtilClasses.ScopeType
Parameter Sets: (All)
Aliases:
Accepted values: CurrentUser, AllUsers

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipDependencyCheck

Skips the check for resource dependencies. This means that only named resources are updated.

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

Specifies the path to temporarily install the resource before actual installatoin. If no temporary
path is provided, the resource is temporarily installed in the current user's temporary folder.

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
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
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

## OUTPUTS

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSResourceInfo

By default, the cmdlet doesn't return any objects. When the **PassThru** parameter is used, the
cmdlet outputs a **PSResourceInfo** object for the saved resource.

## NOTES

The module defines `udres` as an alias for `Update-PSResource`.

## RELATED LINKS

[Install-PSResource](Install-PSResource.md)
