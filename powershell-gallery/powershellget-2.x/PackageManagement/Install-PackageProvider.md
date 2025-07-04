---
external help file: Microsoft.PowerShell.PackageManagement.dll-Help.xml
Locale: en-US
Module Name: PackageManagement
ms.date: 06/16/2025
online version: https://learn.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Install-PackageProvider
---
# Install-PackageProvider

## SYNOPSIS
Installs one or more Package Management package providers.

## SYNTAX

### PackageBySearch (Default)

```
Install-PackageProvider [-Name] <String[]> [-RequiredVersion <String>] [-MinimumVersion <String>]
 [-MaximumVersion <String>] [-Credential <PSCredential>] [-Scope <String>] [-Source <String[]>]
 [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-AllVersions] [-Force] [-ForceBootstrap]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### PackageByInputObject

```
Install-PackageProvider [-Scope <String>] [-InputObject] <SoftwareIdentity[]> [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-AllVersions] [-Force] [-ForceBootstrap] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## DESCRIPTION

The `Install-PackageProvider` cmdlet installs matching Package Management providers that are
available in package sources registered with **PowerShellGet**. By default, this includes modules
available in the Windows PowerShell Gallery with the **PackageManagement** tag. The
**PowerShellGet** Package Management provider is used for finding providers in these repositories.

This cmdlet also installs matching Package Management providers that are available using the Package
Management bootstrapping application.

## EXAMPLES

### Example 1: Install a package provider from the PowerShell Gallery

This command installs the GistProvider package provider from the PowerShell Gallery.

```powershell
Install-PackageProvider -Name "GistProvider" -Verbose
```

### Example 2: Install a specified version of a package provider

This example installs a specified version of the NuGet package provider.

The first command finds all versions of the package provider named NuGet.
The second command installs a specified version of the NuGet package provider.

```powershell
Find-PackageProvider -Name "NuGet" -AllVersions
Install-PackageProvider -Name "NuGet" -RequiredVersion "2.8.5.216" -Force
```

You only need to install the NuGet package provider if you are running PackageManagement v1.1.0.0 in
Windows PowerShell. Newer versions of PowerShellGet and PackageManagement include the NuGet package
provider by default.

### Example 3: Find a provider and install it

This example uses `Find-PackageProvider` and the pipeline to search for the Gist provider and
install it.

```powershell
Find-PackageProvider -Name "GistProvider" | Install-PackageProvider -Verbose
```

### Example 4: Install a provider to the current user's module folder

This command installs a package provider to `$env:LOCALAPPDATA\PackageManagement\ProviderAssemblies`
so that only the current user can use it.

```powershell
Install-PackageProvider -Name GistProvider -Verbose -Scope CurrentUser
```

## PARAMETERS

### -AllVersions

Indicates that this cmdlet installs all available versions of the package provider. By default,
`Install-PackageProvider` only returns the highest available version.

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

### -Credential

Specifies a user account that has permission to install package providers.

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: PackageBySearch
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

Indicates that this cmdlet forces all actions with this cmdlet that can be forced. Currently, this
means the **Force** parameter acts the same as the **ForceBootstrap** parameter.

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

### -ForceBootstrap

Indicates that this cmdlet automatically installs the package provider.

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

### -InputObject

Specifies a **SoftwareIdentity** object. Use the `Find-PackageProvider` cmdlet to obtain a
**SoftwareIdentity** object to pipe into `Install-PackageProvider`.

```yaml
Type: Microsoft.PackageManagement.Packaging.SoftwareIdentity[]
Parameter Sets: PackageByInputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -MaximumVersion

Specifies the maximum allowed version of the package provider that you want to install. If you do
not add this parameter, `Install-PackageProvider` installs the highest available version of the
provider.

```yaml
Type: System.String
Parameter Sets: PackageBySearch
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MinimumVersion

Specifies the minimum allowed version of the package provider that you want to install. If you do
not add this parameter, `Install-PackageProvider` installs the highest available version of the
package that also satisfies any requirement specified by the *MaximumVersion* parameter.

```yaml
Type: System.String
Parameter Sets: PackageBySearch
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

Specifies one or more package provider module names. Separate multiple package names with commas.
Wildcard characters aren't supported.

```yaml
Type: System.String[]
Parameter Sets: PackageBySearch
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Proxy

Specifies a proxy server for the request, rather than connecting directly to the Internet resource.

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyCredential

Specifies a user account that has permission to use the proxy server that's specified by the
**Proxy** parameter.

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

### -RequiredVersion

Specifies the exact allowed version of the package provider that you want to install. If you don't
add this parameter, `Install-PackageProvider` installs the highest available version of the provider
that also satisfies any maximum version specified by the **MaximumVersion** parameter.

```yaml
Type: System.String
Parameter Sets: PackageBySearch
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Scope

Specifies the installation scope of the provider. The acceptable values for this parameter
are:

- **AllUsers** - installs providers in a location that's accessible to all users of the computer.
  By default, this is **$env:ProgramFiles\PackageManagement\ProviderAssemblies.**

- **CurrentUser** - installs providers in a location where they're only accessible to the current
  user. By default, this is **$env:LOCALAPPDATA\PackageManagement\ProviderAssemblies.**

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: CurrentUser, AllUsers

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Source

Specifies one or more package sources. Use the `Get-PackageSource` cmdlet to get a list of available
package sources.

```yaml
Type: System.String[]
Parameter Sets: PackageBySearch
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
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
[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### Microsoft.PackageManagement.Packaging.SoftwareIdentity

You can pipe a **SoftwareIdentity** object to this cmdlet. Use `Find-PackageProvider` to get a
**SoftwareIdentity** object that can be piped into `Install-PackageProvider`.

## OUTPUTS

## NOTES

> [!IMPORTANT]
> As of April 2020, the PowerShell Gallery no longer supports Transport Layer Security (TLS)
> versions 1.0 and 1.1. If you aren't using TLS 1.2 or higher, you will receive an error when
> trying to access the PowerShell Gallery. Use the following command to ensure you are using TLS
> 1.2:
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> For more information, see the
> [announcement](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) in the
> PowerShell blog.

## RELATED LINKS

[Find-PackageProvider](Find-PackageProvider.md)

[Get-PackageProvider](Get-PackageProvider.md)

[Import-PackageProvider](Import-PackageProvider.md)
