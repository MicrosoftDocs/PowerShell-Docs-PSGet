---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 2.9.0-preview
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-PSRepository
---
# Set-PSRepository

## SYNOPSIS
Sets values for a registered repository.

## SYNTAX

```
Set-PSRepository [-Name] <String> [[-SourceLocation] <Uri>] [-PublishLocation <Uri>]
 [-ScriptSourceLocation <Uri>] [-ScriptPublishLocation <Uri>] [-Credential <PSCredential>]
 [-InstallationPolicy <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-PackageManagementProvider <String>] [<CommonParameters>]
```

## DESCRIPTION

The `Set-PSRepository` cmdlet sets values for a registered module repository. The settings are
persistent for the current user and apply to all versions of PowerShell installed for that user.

This is a proxy cmdlet for the `Set-PSResourceRepository` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Set-PSResourceRepository](../Microsoft.PowerShell.PSResourceGet/Set-PSResourceRepository.md).

## EXAMPLES

### Example 1: Set the installation policy for a repository

```powershell
Set-PSRepository -Name "myInternalSource" -InstallationPolicy Trusted
```

This command sets the installation policy for the **myInternalSource** repository to **Trusted**, so
that you are not prompted before installing modules from that source.

### Example 2: Set the source and publish locations for a repository

```powershell
Set-PSRepository -Name "myInternalSource" -SourceLocation 'https://someNuGetUrl.com/api/v2' -PublishLocation 'https://someNuGetUrl.com/api/v2/packages'
```

This command sets the source location and publish location for **myInternalSource** to the specified
URIs.

## PARAMETERS

### -Credential

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

### -InstallationPolicy

Specifies the installation policy. Valid values are: **Trusted**, **Untrusted**.

The proxy cmdlet transforms the value of this parameter to the **Trusted** parameter of
`Set-PSResourceRepository`.

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

Specifies the name of the repository.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PackageManagementProvider

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

### -Proxy

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

### -ScriptPublishLocation

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

### -ScriptSourceLocation

The proxy cmdlet ignores this parameter since it's not supported by `Set-PSResourceRepository`.

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

### -SourceLocation

Specifies the URI for discovering and installing modules from this repository. For example, for
NuGet-based repositories, the source location is similar to `https://someNuGetUrl.com/api/v2`.

The proxy cmdlet maps this parameter to the **Uri** parameter of `Set-PSResourceRepository`.

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
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

### System.String

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

[Register-PSRepository](Register-PSRepository.md)

[Unregister-PSRepository](Unregister-PSRepository.md)
