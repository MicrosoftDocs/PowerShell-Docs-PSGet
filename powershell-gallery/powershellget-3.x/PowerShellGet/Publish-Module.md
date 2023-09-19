---
external help file: PSModule-help.xml
Locale: en-US
Module Name: PowerShellGet
ms.custom: 2.9.0-preview
ms.date: 09/19/2023
online version: https://learn.microsoft.com/powershell/module/powershellget/publish-module?view=powershellget-2.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Publish-Module
---

# Publish-Module

## SYNOPSIS
Publishes a specified module from the local computer to an online gallery.

## SYNTAX

### ModuleNameParameterSet (Default)

```
Publish-Module -Name <String> [-RequiredVersion <String>] [-NuGetApiKey <String>]
 [-Repository <String>] [-Credential <PSCredential>] [-FormatVersion <Version>]
 [-ReleaseNotes <String[]>] [-Tags <String[]>] [-LicenseUri <Uri>] [-IconUri <Uri>]
 [-ProjectUri <Uri>] [-Exclude <String[]>] [-Force] [-AllowPrerelease] [-SkipAutomaticTags]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ModulePathParameterSet

```
Publish-Module -Path <String> [-NuGetApiKey <String>] [-Repository <String>]
 [-Credential <PSCredential>] [-FormatVersion <Version>] [-ReleaseNotes <String[]>]
 [-Tags <String[]>] [-LicenseUri <Uri>] [-IconUri <Uri>] [-ProjectUri <Uri>] [-Force]
 [-SkipAutomaticTags] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

The `Publish-Module` cmdlet publishes a module to an online NuGet-based gallery by using an API key,
stored as part of a user's profile in the gallery. You can specify the module to publish either by
the module's name, or by the path to the folder containing the module.

This is a proxy cmdlet for the `Publish-PSResource` cmdlet in the
**Microsoft.PowerShell.PSResourceGet**. For more information, see
[Publish-PSResource](../Microsoft.PowerShell.PSResourceGet/Publish-PSResource.md).

## EXAMPLES

### Example 1: Publish a module

In this example, **MyDscModule** is published to the online gallery by using the API key to indicate
the module owner's online gallery account. If MyDscModule is not a valid manifest module that
specifies a name, version, description, and author, an error occurs.

```powershell
Publish-Module -Name "MyDscModule" -NuGetApiKey "11e4b435-6cb4-4bf7-8611-5162ed75eb73"
```

### Example 2: Publish a module with gallery metadata

In this example, **MyDscModule** is published to the online gallery by using the API key to indicate
the module owner's gallery account. The additional metadata provided is displayed on the webpage for
the module in the gallery. The owner adds two search tags for the module, relating it to Active
Directory; a brief release note is added. If MyDscModule is not a valid manifest module that
specifies a name, version, description, and author, an error occurs.

```powershell
$parameters = @{
    Name        = "MyDscModule"
    NuGetApiKey = "11e4b435-6cb4-4bf7-8611-5162ed75eb73"
    LicenseUri  = "http://contoso.com/license"
    Tag         = "Active Directory","DSC"
    ReleaseNote = "Updated the ActiveDirectory DSC Resources to support adding users."
}
Publish-Module @parameters
```

## PARAMETERS

### -AllowPrerelease

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

Specifies a user account that has rights to publish a module for a specified package provider or
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

### -Exclude

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

```yaml
Type: System.String[]
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -FormatVersion

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

```yaml
Type: System.Version
Parameter Sets: (All)
Aliases:
Accepted values: 2.0

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IconUri

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -LicenseUri

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -Name

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

```yaml
Type: System.String
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NuGetApiKey

Specifies the API key that you want to use to publish a module to the online gallery. The API key is
part of your profile in the online gallery, and can be found on your user account page in the
gallery. The API key is NuGet-specific functionality.

The proxy cmdlet maps this parameter to the **ApiKey** parameter of `Publish-PSResource`.

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

### -Path

Specifies the path to the module that you want to publish. This parameter accepts the path to the
folder that contains the module. The folder must have the same name as the module.

```yaml
Type: System.String
Parameter Sets: ModulePathParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProjectUri

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -ReleaseNotes

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -Repository

Specifies the friendly name of a repository that has been registered by running
`Register-PSRepository`. The repository must have a **PublishLocation**, which is a valid NuGet URI.
The **PublishLocation** can be set by running `Set-PSRepository`.

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

### -RequiredVersion

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

```yaml
Type: System.String
Parameter Sets: ModuleNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipAutomaticTags

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -Tags

The proxy cmdlet ignores this parameter since it's not supported by `Publish-PSResource`.

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

### -Confirm

Prompts you for confirmation before running the `Publish-Module`.

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

Shows what would happen if the `Publish-Module` runs. The cmdlet is not run.

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
-WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.String

### System.Management.Automation.PSCredential

## OUTPUTS

### System.Object

## NOTES

PowerShell includes the following aliases for `Publish-Module`:

- All platforms:
  - `pumo`

`Publish-Module` runs on PowerShell 3.0 or later releases of PowerShell, on Windows 7 or Windows
2008 R2 and later releases of Windows.

The PowerShell Gallery no longer supports Transport Layer Security (TLS) versions 1.0 and 1.1. You
must use TLS 1.2 or higher. Use the following command to ensure you are using TLS 1.2:

`[Net.ServicePointManager]::SecurityProtocol = [Net.ServicePointManager]::SecurityProtocol -bor [Net.SecurityProtocolType]::Tls12`

Publishing a module requires metadata that is displayed on the gallery page for the module. Required
metadata includes the module name, version, description, and author. The metadata must be defined in
the module manifest. For more information, see
[Package manifest values that impact the PowerShell Gallery UI](/powershell/scripting/gallery/concepts/package-manifest-affecting-ui).

## RELATED LINKS

[Find-Module](Find-Module.md)

[Install-Module](Install-Module.md)

[Register-PSRepository](Register-PSRepository.md)

[Set-PSRepository](Set-PSRepository.md)

[Uninstall-Module](Uninstall-Module.md)

[Update-Module](Update-Module.md)
