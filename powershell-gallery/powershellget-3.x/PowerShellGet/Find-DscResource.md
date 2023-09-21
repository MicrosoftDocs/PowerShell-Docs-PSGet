---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 3.0.22-beta22
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/find-dscresource?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Find-DscResource
---

# Find-DscResource

## SYNOPSIS
Finds Desired State Configuration (DSC) resources.

## SYNTAX

### All

```
Find-DscResource [[-Name] <String[]>] [-ModuleName <String>] [-MinimumVersion <String>]
 [-MaximumVersion <String>] [-RequiredVersion <String>] [-AllVersions] [-AllowPrerelease]
 [-Tag <String[]>] [-Filter <String>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>]
 [-Repository <String[]>] [<CommonParameters>]
```

## DESCRIPTION

The `Find-DscResource` cmdlet searches registered repositories to find DSC resources contained in
modules. By default `Find-DscResource` searches all registered repositories.

This is a proxy cmdlet for the `Find-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Find-PSResource](../Microsoft.PowerShell.PSResourceGet/Find-PSResource.md).

## EXAMPLES

### Example 1: Find a DSC resource by name

`Find-DscResource` locates DSC resources by name. Use commas to separate an array of resource names.

```powershell
Find-DscResource -Name xWebsite, xWebApplication, xWebSiteDefaults
```

```Output
Name               Version    ModuleName            Repository
----               -------    ----------            ----------
xWebApplication    2.6.0.0    xWebAdministration    PSGallery
xWebsite           2.6.0.0    xWebAdministration    PSGallery
xWebSiteDefaults   2.6.0.0    xWebAdministration    PSGallery
```

`Find-DscResource` uses the **Name** parameter to find the specified array of DSC resources.

### Example 2: Find a DSC resource and install it

`Find-DscResource` locates a DSC resource and sends the object down the pipeline to be installed.
After the installation, use `Get-InstalledModule` to view the results.

Multiple resources from the same module can be sent down the pipeline to the `Install-Module`.
`Install-Module` attempts to only install the module once.

```powershell
Find-DscResource -Name xWebsite | Install-Module
```

`Find-DscResource` uses the **Name** parameter to find the resource named **xWebsite**. The object
is sent down the pipeline to the `Install-Module` cmdlet. `Install-Module` installs the
**xWebAdministration** module for the resource.

## PARAMETERS

### -AllowPrerelease

Includes resources marked as a prerelease in the results.

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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

Specifies the name of a resource. The default is all resources. Use commas to separate an array of
resource names.

The proxy cmdlet maps this parameter to the **DscResourceName** parameter of `Find-PSResource`.

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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

Specifies a repository to search for resources. Use commas to separate an array of repository names.

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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

The proxy cmdlet ignores this parameter since it's not supported by the **DscResourceNameParameterSet**
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

### PSGetDscResourceInfo

`Find-DscResource` returns a **PSGetDscResourceInfo** object.

## NOTES

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`

## RELATED LINKS

[Get-InstalledModule](Get-InstalledModule.md)

[Install-Module](Install-Module.md)

[Register-PSRepository](Register-PSRepository.md)

[Select-Object](xref:Microsoft.PowerShell.Utility.Select-Object)

[Uninstall-Module](Uninstall-Module.md)
