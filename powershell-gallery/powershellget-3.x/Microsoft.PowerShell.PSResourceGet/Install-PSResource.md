---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.1.1
ms.date: 03/17/2025
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/install-psresource?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Install-PSResource

## SYNOPSIS

Installs resources from a registered repository.

## SYNTAX

### NameParameterSet (Default)

```
Install-PSResource [-Name] <String[]> [-Version <String>] [-Prerelease] [-Repository <String[]>]
 [-Credential <PSCredential>] [-Scope <ScopeType>] [-TemporaryPath <String>] [-TrustRepository]
 [-Reinstall] [-Quiet] [-AcceptLicense] [-NoClobber] [-SkipDependencyCheck] [-AuthenticodeCheck]
 [-PassThru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InputObjectParameterSet

```
Install-PSResource [-Repository <String[]>] [-Credential <PSCredential>] [-Scope <ScopeType>]
 [-TemporaryPath <String>] [-TrustRepository] [-Reinstall] [-Quiet] [-AcceptLicense] [-NoClobber]
 [-SkipDependencyCheck] [-AuthenticodeCheck] [-PassThru] [-InputObject] <PSResourceInfo[]> [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### RequiredResourceFileParameterSet

```
Install-PSResource [-Credential <PSCredential>] [-Scope <ScopeType>] [-TemporaryPath <String>]
 [-TrustRepository] [-Reinstall] [-Quiet] [-AcceptLicense] [-NoClobber] [-SkipDependencyCheck]
 [-AuthenticodeCheck] [-PassThru] -RequiredResourceFile <String> [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### RequiredResourceParameterSet

```
Install-PSResource [-Credential <PSCredential>] [-Scope <ScopeType>] [-TemporaryPath <String>]
 [-TrustRepository] [-Reinstall] [-Quiet] [-AcceptLicense] [-NoClobber] [-SkipDependencyCheck]
 [-AuthenticodeCheck] [-PassThru] -RequiredResource <Object> [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## DESCRIPTION

This cmdlet installs resources from a registered repository to an installation path on a machine. By
default, the cmdlet doesn't return any object. Other parameters allow you to specify the repository,
scope, and version for a resource, and suppress license prompts.

This cmdlet combines the functions of the `Install-Module` and `Install-Script` cmdlets from
**PowerShellGet** v2.

`Install-PSResource` doesn't load the newly installed module into the current session. You must
import the new version or start a new session to use the updated module. For more information, see
[Import-Module](xref:Microsoft.PowerShell.Core.Import-Module).

> [!NOTE]
> `Install-PSResource` doesn't install dependent resources from repositories that use the NuGet v3
> protocol. You must install the dependent resources individually. We intend to add this feature in
> a future release.

## EXAMPLES

### Example 1

Installs the latest stable (non-prerelease) version of the **Az** module from the PowerShell
Gallery.

```powershell
Install-PSResource Az -Repository PSGallery
```

The Az module is a meta-module that includes all the Az PowerShell modules as dependencies. This
command installs the Az module and all its dependencies.

### Example 2

Installs the latest stable **Az** module within the between versions `7.3.0` and `8.3.0`.

```powershell
Install-PSResource Az -Version '[7.3.0, 8.3.0]'
```

### Example 3

Installs the latest stable version of the **Az** module. When the **Reinstall** parameter is used,
the cmdlet writes over any previously installed version.

```powershell
Install-PSResource Az -Reinstall
```

### Example 4

Installs the PSResources specified in the psd1 file.

```powershell
Install-PSResource -RequiredResourceFile myRequiredModules.psd1
```

### Example 5

Installs the PSResources specified in the hashtable.

```powershell
Install-PSResource -RequiredResource  @{
    TestModule = @{
        version = '[0.0.1,1.3.0]'
        repository = 'PSGallery'
      }
    TestModulePrerelease = @{
        version = '[0.0.0,0.0.5]'
        repository = 'PSGallery'
        prerelease = 'true'
    }
    TestModule99 = @{}
}
```

## PARAMETERS

### -AcceptLicense

Specifies that the resource should accept any request to accept the license agreement. This
suppresses prompting if the module mandates that a user accept the license agreement.

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

Validates Authenticode signatures and catalog files on Windows.

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
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -NoClobber

Prevents installing a package that contains cmdlets that already exist on the machine.

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

When specified, includes prerelease versions in search results returned.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameParameterSet
Aliases: IsPrerelease

Required: False
Position: Named
Default value: False
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Quiet

Suppresses installation progress bar.

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

### -Reinstall

Installs the latest version of a module even if the latest version is already installed. The
installed version is overwritten. This allows you to repair a damaged installation of the module.

If an older version of the module is installed, the new version is installed side-by-side in a new
version-specific folder.

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
repositories using this sort order. `Install-PSResource` installs the first matching package from
the sorted list of repositories.

The parameter supports the `*` wildcard character. If you specify multiple repositories, all names
must include or omit the wildcard character. You can't specify a mix of names with and without
wildcards.

```yaml
Type: System.String[]
Parameter Sets: NameParameterSet, InputObjectParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -RequiredResource

A hashtable or JSON string that specifies resources to install. Wildcard characters aren't allowed.
See the [NOTES](#notes) section for a description of the file formats.

```yaml
Type: System.Object
Parameter Sets: RequiredResourceParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RequiredResourceFile

Path to a `.psd1` or `.json` that specifies resources to install. Wildcard characters aren't
allowed. See the [NOTES](#notes) section for a description of the file formats.

```yaml
Type: System.String
Parameter Sets: RequiredResourceFileParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Scope

Specifies the installation scope. Accepted values are:

- `CurrentUser`
- `AllUsers`

The default scope is `CurrentUser`, which doesn't require elevation for install.

The `AllUsers` scope installs modules in a location accessible to all users of the computer. For
example:

- `$env:ProgramFiles\PowerShell\Modules`

The `CurrentUser` installs modules in a location accessible only to the current user of the
computer. For example:

- `$home\Documents\PowerShell\Modules`

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

Specifies the path to temporarily install the resource before actual installation. If no temporary
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
Parameter Sets: NameParameterSet
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

The module defines `isres` as an alias for `Install-PSResource`.

The **RequiredResource** and **RequiredResourceFile** parameters are used to find **PSResource**
objects matching specific criteria. You can specify the search criteria using a hashtable or a JSON
object. For the **RequiredResourceFile** parameter, the hashtable is stored in a `.psd1` file and
the JSON object is stored in a `.json` file. For more information, see
[about_PSResourceGet](./about/about_PSResourceGet.md).

## RELATED LINKS

[Package versioning](/nuget/concepts/package-versioning#version-ranges)

[Uninstall-PSResource](Uninstall-PSResource.md)
