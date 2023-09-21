---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 3.0.22-beta22
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/find-module?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-Module
---
# Find-Module

## SYNOPSIS
Finds modules in a repository that match specified criteria.

## SYNTAX

### All

```
Find-Module [[-Name] <string[]>] [-MinimumVersion <string>] [-MaximumVersion <string>]
 [-RequiredVersion <string>] [-AllVersions] [-IncludeDependencies] [-Filter <string>]
 [-Tag <string[]>] [-Includes <string[]>] [-DscResource <string[]>] [-RoleCapability <string[]>]
 [-Command <string[]>] [-Proxy <uri>] [-ProxyCredential <pscredential>] [-Repository <string[]>]
 [-Credential <pscredential>] [-AllowPrerelease] [<CommonParameters>]
```

## DESCRIPTION

The `Find-Module` cmdlet finds modules in a repository that match the specified criteria.
`Find-Module` returns a **PSRepositoryItemInfo** object for each module it finds. The objects can be
sent down the pipeline to cmdlets such as `Install-Module`.

This is a proxy cmdlet for the `Find-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Find-PSResource](../Microsoft.PowerShell.PSResourceGet/Find-PSResource.md).

## EXAMPLES

### Example 1: Find a module by name

This example finds a module in the default repository.

```powershell
Find-Module -Name PowerShellGet
```

```Output
Version   Name              Repository           Description
-------   ----              ----------           -----------
2.1.0     PowerShellGet     PSGallery            PowerShell module with commands for discovering...
```

The `Find-Module` cmdlet uses the **Name** parameter to specify the **PowerShellGet** module.

### Example 2: Find modules with similar names

This example uses the asterisk (`*`) wildcard to find modules with similar names.

```powershell
Find-Module -Name PowerShell*
```

```Output
Version   Name                            Repository    Description
-------   ----                            ----------    -----------
0.4.0     powershell-yaml                 PSGallery     Powershell module for serializing and...
2.1.0     PowerShellGet                   PSGallery     PowerShell module with commands for...
1.9       Powershell.Helper.Extension     PSGallery     # Powershell.Helper.Extension...
3.1       PowerShellHumanizer             PSGallery     PowerShell Humanizer wraps Humanizer...
4.0       PowerShellISEModule             PSGallery     a module that adds capability to the ISE
```

The `Find-Module` cmdlet uses the **Name** parameter with the asterisk (`*`) wildcard to find all
modules that contain **PowerShell**.

### Example 3: Find a module by minimum version

This example searches for a module's minimum version. If the repository contains a newer version of
the module, the newer version is returned.

```powershell
Find-Module -Name PowerShellGet -MinimumVersion 1.6.5
```

```Output
Version   Name             Repository     Description
-------   ----             ----------     -----------
2.1.0     PowerShellGet    PSGallery      PowerShell module with commands for discovering...
```

The `Find-Module` cmdlet uses the **Name** parameter to specify the **PowerShellGet** module. The
**MinimumVersion** specifies version **1.6.5**. `Find-Module` returns PowerShellGet version
**2.1.0** because it exceeds the minimum version and is the most current version.

### Example 4: Find a module by specific version

This example shows how to install a specific prerelease version of a module. Prerelease versions
have a format of `<version_number>-<prerelease_label>`.

```powershell
Find-Module PSReadLine -AllowPrerelease -RequiredVersion 2.2.4-beta1
```

```Output
Version        Name             Repository       Description
-------        ----             ----------       -----------
2.2.4-beta1    PSReadLine       PSGallery        Great command line editing in the PowerS…
```

### Example 5: Find a module in a specific repository

This example uses the **Repository** parameter to find a module in a specific repository.

```powershell
Find-Module -Name PowerShellGet -Repository PSGallery
```

```Output
Version   Name             Repository     Description
-------   ----             ----------     -----------
2.1.0     PowerShellGet    PSGallery      PowerShell module with commands for discovering...
```

The `Find-Module` cmdlet uses the **Name** parameter to specify the **PowerShellGet** module. The
**Repository** parameter specifies to search the **PSGallery** repository.

### Example 6: Find a module in multiple repositories

This example uses the `Register-PSRepository` to specify a repository. `Find-Module` uses the
repository to search for a module.

```powershell
Register-PSRepository -Name MySource -SourceLocation https://www.myget.org/F/powershellgetdemo/
Find-Module -Name Contoso* -Repository PSGallery, MySource
```

```Output
Repository    Version   Name             Description
----------    -------   ----             -----------
PSGallery     2.0.0.0   ContosoServer    Cmdlets and DSC resources for managing Contoso Server...
MySource      1.2.0.0   ContosoClient    Cmdlets and DSC resources for managing Contoso Client...
```

The `Register-PSRepository` cmdlet registers a new repository. The **Name** parameter assigns the
name **MySource**. The **SourceLocation** parameter specifies the repository's address.

The `Find-Module` cmdlet uses the **Name** parameter with the asterisk (`*`) wildcard to specify the
**Contoso** module. The **Repository** parameter specifies to search two repositories, **PSGallery**
and **MySource**.

### Example 7: Find a module that contains a DSC resource

This command returns modules that contain DSC resources. The **Includes** parameter has four
predefined functionalities that are used to search the repository. Use tab-complete to display the
four functionalities supported by the **Includes** parameter.

```powershell
Find-Module -Repository PSGallery -Includes DscResource
```

```Output
Version     Name                            Repository    Description
-------     ----                            ----------    -----------
2.7.0       Carbon                          PSGallery     Carbon is a PowerShell module...
8.5.0.0     xPSDesiredStateConfiguration    PSGallery     The xPSDesiredStateConfiguration module...
1.3.1       PackageManagement               PSGallery     PackageManagement (a.k.a. OneGet) is...
2.7.0.0     xWindowsUpdate                  PSGallery     Module with DSC Resources...
3.2.0.0     xCertificate                    PSGallery     This module includes DSC resources...
3.1.0.0     xPowerShellExecutionPolicy      PSGallery     This DSC resource can change the user...
```

The `Find-Module` cmdlet uses the **Repository** parameter to search the repository, **PSGallery**.
The **Includes** parameter specifies **DscResource**, which is a functionality that the parameter
can search for in the repository.

### Example 8: Find a module with a filter

In this example, to find modules, a filter is used to search the repository.

For a NuGet-based repository, the **Filter** parameter searches through the name, description, and
tags for the argument.

```powershell
Find-Module -Filter AppDomain
```

```Output
Version    Name              Repository           Description
-------    ----              ----------           -----------
1.0.0.0  AppDomainConfig     PSGallery            Manipulate AppDomain configuration...
1.1.0    ClassExplorer       PSGallery            Quickly search the AppDomain for classes...
```

The `Find-Module` cmdlet uses the **Filter** parameter to search the repository for **AppDomain**.

### Example 9: Find a module by tag

This example shows how to find modules by a tag. The `CrescendoBuilt` value is a tag that is
automatically added to modules created using the **Microsoft.PowerShell.Crescendo** module.

```powershell
Find-Module -Tag CrescendoBuilt
```

```Output
Version Name            Repository Description
------- ----            ---------- -----------
0.1.0   Foil            PSGallery  A PowerShell Crescendo wrapper for Chocolatey
0.3.1   Cobalt          PSGallery  A PowerShell Crescendo wrapper for WinGet
1.1.0   SysInternals    PSGallery  PowerShell cmdlets for SysInternal tools
0.0.4   Croze           PSGallery  A PowerShell Crescendo wrapper for Homebrew
0.0.2   AptPackage      PSGallery  PowerShell Crescendo-generated Module to query APT-Package Information
1.0.1   RoboCopy        PSGallery  PowerShell cmdlet for the official RoboCopy.exe
1.0.2   TShark          PSGallery  PowerShell cmdlet for tshark.exe
1.0.0   SpeedTestCLI    PSGallery  PowerShell cmdlets speedtest-cli
1.0.0   SpeedTest-CLI   PSGallery  PowerShell cmdlets for Internet Speed Test
1.0.2   Image2Text      PSGallery  PowerShell Images into ASCII art
0.1.1   Quser.Crescendo PSGallery  This module displays session information of users logged onto a local or remote m...
1.0.2   Takeown         PSGallery  Crescendo Powershell wrapper of takeown.exe
```

## PARAMETERS

### -AllowPrerelease

Includes in the results modules marked as a pre-release.

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

Specifies to include all versions of a module in the results. You cannot use the **AllVersions**
parameter with the **MinimumVersion**, **MaximumVersion**, or **RequiredVersion** parameters.

The proxy cmdlet transforms this parameter to the `-Version *` before calling `Find-PSResource`.

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

Specifies a user account that has rights to install a module for a specified package provider or
source.

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

### -DscResource

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

Indicates that this operation includes all modules that are dependent upon the module specified in
the **Name** parameter.

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
Accepted values: DscResource, Cmdlet, Function, RoleCapability

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

Specifies the names of modules to search for in the repository. A comma-separated list of module
names is accepted. Wildcards are accepted.

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

Use the **Repository** parameter to specify which repository to search for a module. Used when
multiple repositories are registered. Accepts a comma-separated list of repositories. To register a
repository, use `Register-PSRepository`. To display registered repositories, use `Get-PSRepository`.

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

### -RoleCapability

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

### -Tag

Specifies an array of tags. Example tags include **DesiredStateConfiguration**, **DSC**,
**DSCResourceKit**, or **PSModule**.

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

`Find-Module` creates **PSRepositoryItemInfo** objects that can be sent down the pipeline to cmdlets
such as `Install-Module`.

## NOTES

PowerShell includes the following aliases for `Find-Module`:

- All platforms:
  - `fimo`

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`


## RELATED LINKS

[Get-PSRepository](Get-PSRepository.md)

[Install-Module](Install-Module.md)

[Publish-Module](Publish-Module.md)

[Save-Module](Save-Module.md)

[Uninstall-Module](Uninstall-Module.md)

[Update-Module](Update-Module.md)

[Register-PSRepository](Register-PSRepository.md)
