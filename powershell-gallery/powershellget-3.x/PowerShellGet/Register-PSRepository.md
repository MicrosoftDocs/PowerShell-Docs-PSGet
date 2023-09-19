---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 2.9.0-preview
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/register-psrepository?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-PSRepository
---
# Register-PSRepository

## SYNOPSIS
Registers a PowerShell repository.

## SYNTAX

### NameParameterSet (Default)

```
Register-PSRepository [-Name] <String> [-SourceLocation] <Uri> [-PublishLocation <Uri>]
 [-ScriptSourceLocation <Uri>] [-ScriptPublishLocation <Uri>] [-Credential <PSCredential>]
 [-InstallationPolicy <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-PackageManagementProvider <String>] [<CommonParameters>]
```

### PSGalleryParameterSet

```
Register-PSRepository [-Default] [-InstallationPolicy <String>] [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [<CommonParameters>]
```

## DESCRIPTION

The `Register-PSRepository` cmdlet registers the default repository for PowerShell modules. After a
repository is registered, you can reference it from the `Find-Module`, `Install-Module`, and
`Publish-Module` cmdlets. The registered repository becomes the default repository in `Find-Module`
and `Install-Module`.

Registered repositories are user-specific. They are not registered in a system-wide context.

This is a proxy cmdlet for the `Register-PSResourceRepository` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Register-PSResourceRepository](../Microsoft.PowerShell.PSResourceGet/Register-PSResourceRepository.md).

## EXAMPLES

### Example 1: Register a repository

```powershell
$parameters = @{
  Name = "myNuGetSource"
  SourceLocation = "https://www.myget.org/F/powershellgetdemo/api/v2"
  PublishLocation = "https://www.myget.org/F/powershellgetdemo/api/v2/Packages"
  InstallationPolicy = 'Trusted'
}
Register-PSRepository @parameters
Get-PSRepository
```

```Output
Name                SourceLocation          OneGetProvider       InstallationPolicy
----                --------------          --------------       ------------------
PSGallery           http://go.micro...      NuGet                Untrusted
myNuGetSource       https://myget.c...      NuGet                Trusted
```

The first command registers `https://www.myget.org/F/powershellgetdemo/` as a repository for the
current user. After myNuGetSource is registered, you can explicitly reference it when searching for,
installing, and publishing modules. Because the **PackageManagementProvider** parameter isn't
specified, the repository is not explicitly associated with a OneGet package provider, so
PowerShellGet polls available package providers and associates it with the NuGet provider.

The second command gets registered repositories and displays the results.

## PARAMETERS

### -Credential

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Default

Registers PowerShell Gallery as the default repository.

The proxy cmdlet transforms the value of this parameter to the **PSGallery** parameter of
`Register-PSResourceRepository`.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PSGalleryParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InstallationPolicy

Specifies the installation policy. Valid values are: Trusted, UnTrusted. The default value is
UnTrusted.

A repository's installation policy specifies PowerShell behavior when installing from that
repository. When installing modules from an UnTrusted repository, the user is prompted for
confirmation.

The proxy cmdlet transforms the value of this parameter to the **Trusted** parameter of
`Register-PSResourceRepository`.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Trusted, Untrusted

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

Specifies the name of the repository to register. You can use this name to specify the repository in
cmdlets such as `Find-Module` and `Install-Module`.

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PackageManagementProvider

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Proxy

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

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

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

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

### -PublishLocation

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptPublishLocation

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptSourceLocation

The proxy cmdlet ignores this parameter since it's not supported by `Register-PSResourceRepository`.

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SourceLocation

Specifies the URI for discovering and installing modules from this repository. A URI can be a NuGet
server feed (most common situation), HTTP, HTTPS, FTP or file location.

For example, for NuGet-based repositories, the source location is similar to
`https://someNuGetUrl.com/api/v2`.

The proxy cmdlet maps this parameter to the **Uri** parameter of `Register-PSResourceRepository`

```yaml
Type: System.Uri
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see
[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.Management.Automation.PSCredential

### System.Uri

## OUTPUTS

### System.Object

## NOTES

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`

## RELATED LINKS

[Get-PSRepository](Get-PSRepository.md)

[Set-PSRepository](Set-PSRepository.md)

[Unregister-PSRepository](Unregister-PSRepository.md)
