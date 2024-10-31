---
external help file: Microsoft.PowerShell.PSResourceGet.dll-help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.1.0-rc2
ms.date: 10/31/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/import-psgetrepository?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---

# Import-PSGetRepository

## SYNOPSIS
Finds the repositories registered with PowerShellGet and registers them for PSResourceGet.

## SYNTAX

```
Import-PSGetRepository [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

This cmdlet finds the NuGet repositories registered with PowerShellGet v2 and registers them for
PSResourceGet. PowerShellGet v2 has a provider model that allows you to register repositories that
use different provider protocols. PSResourceGet only supports NuGet repositories, so this cmdlet
only imports NuGet repositories.

The **PSGallery** repository is registered by default. This cmdlet doesn't import the **PSGallery**
repository from PowerShellGet v2. If you need to reregister the **PSGallery** repository, use the
`Register-PSResourceRepository` cmdlet with the **PSGallery** parameter.

## EXAMPLES

### Example 1 - Show the NuGet repositories registered with PowerShellGet v2

This example uses the **Verbose** and **WhatIf** parameters to show the NuGet repositories
registered with PowerShell v2.

```powershell
Import-PSGetRepository -Verbose -WhatIf
```

```Output
VERBOSE: Found 3 registered PowerShellGet repositories.
VERBOSE: Selected 2 NuGet repositories.
What if: Registering LocalGallery at E:\LocalGallery\ -Trusted:$True -Force:$False.
What if: Registering PrivateRepo at https://PrivateRepo:44370/nuget -Trusted:$True -Force:$False.
```

The cmdlet found three repositories registered with PowerShellGet v2, but will only import two of
them. In this case, the third repository is the default **PSGallery** repository.

### Example 2 - Register the NuGet repositories registered with PowerShellGet v2

```powershell
Import-PSGetRepository
```

```Output
Name         Uri                             Trusted Priority
----         ---                             ------- --------
LocalGallery file:///E:/LocalGallery/        True    50
PrivateRepo  https://PrivateRepo:44370/nuget True    50
```

### Example 3 - Overwrite existing repositories

By default, the cmdlet doesn't import PowerShellGet v2 repositories that have the same name as a
registered PSResourceGet repository. Use the **Force** parameter to overwrite existing repositories.

```powershell
Import-PSGetRepository
```

```Output
WARNING: Adding to repository store failed: The PSResource Repository 'LocalGallery' already exists.
WARNING: Use the -Force switch to overwrite existing repositories.
WARNING: Adding to repository store failed: The PSResource Repository 'PrivateRepo' already exists.
WARNING: Use the -Force switch to overwrite existing repositories.
```

```powershell
Import-PSGetRepository -Force
```

```Output
Name         Uri                             Trusted Priority
----         ---                             ------- --------
LocalGallery file:///E:/LocalGallery/        True    50
PrivateRepo  https://PrivateRepo:44370/nuget True    50
```

## PARAMETERS

### -Force

Use the **Force** parameter to overwrite existing repositories.

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

### -Confirm

Prompts you for confirmation before running the cmdlet.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
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
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see
[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

## OUTPUTS

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSRepositoryInfo

The cmdlet returns a **PSRepositoryInfo** object for each NuGet repository registered with
PowerShellGet v2.

## NOTES

## RELATED LINKS

[Register-PSResourceRepository](Register-PSResourceRepository.md)
