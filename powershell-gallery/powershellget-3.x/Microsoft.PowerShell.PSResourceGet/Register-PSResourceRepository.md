---
external help file: Microsoft.PowerShell.PSResourceGet.dll-Help.xml
Module Name: Microsoft.PowerShell.PSResourceGet
ms.custom: 1.2.0-p5
ms.date: 12/10/2025
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.psresourceget/register-psresourcerepository?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
---
# Register-PSResourceRepository

## SYNOPSIS

Registers a repository for PowerShell resources.

## SYNTAX

### NameParameterSet (Default)

```
Register-PSResourceRepository [-Name] <String> [-Uri] <String> [-Trusted] [-Priority <Int32>]
 [-ApiVersion <APIVersion>] [-CredentialInfo <PSCredentialInfo>] [-PassThru] [-Force]
 [-CredentialProvider <CredentialProvider>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### PSGalleryParameterSet

```
Register-PSResourceRepository [-PSGallery] [-Trusted] [-Priority <Int32>] [-PassThru] [-Force]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### RepositoriesParameterSet

```
Register-PSResourceRepository -Repository <Hashtable[]> [-PassThru] [-Force] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## DESCRIPTION

The cmdlet registers a NuGet repository containing PowerShell resources.

## EXAMPLES

### Example 1

This example registers the repository with the **Name** of `PoshTestGallery`.

```powershell
Register-PSResourceRepository -Name PoshTestGallery -Uri 'https://www.poshtestgallery.com/api/v2'
Get-PSResourceRepository -Name PoshTestGallery
```

```Output
Name             Uri                                          Trusted   Priority
----             ---                                          -------   --------
PoshTestGallery  https://www.poshtestgallery.com/api/v2         False         50
```

### Example 2

This example registers the default `PSGallery` repository. Unlike the previous example, we can't use
the **Name** and **Uri** parameters to register the `PSGallery` repository. The `PSGallery`
repository is registered by default but can be removed. Use this command to restore the default
registration.

```powershell
Register-PSResourceRepository -PSGallery
Get-PSResourceRepository -Name 'PSGallery'
```

```Output
Name             Uri                                          Trusted   Priority
----             ---                                          -------   --------
PSGallery        https://www.powershellgallery.com/api/v2       False         50
```

### Example 3

This example registers multiple repositories at once. To do so, we use the **Repository** parameter
and provide an array of hashtables. Each hashtable can only have keys associated with parameters for
the **NameParameterSet** or the **PSGalleryParameterSet**.

```powershell
$arrayOfHashtables = @{
        Name = 'Local'
        Uri = 'D:/PSRepoLocal/'
        Trusted = $true
        Priority = 20
    },
    @{
        Name = 'PSGv3'
        Uri = 'https://www.powershellgallery.com/api/v3'
        Trusted = $true
        Priority = 50
    },
    @{
        PSGallery = $true
        Trusted = $true
        Priority = 10
    }
Register-PSResourceRepository -Repository $arrayOfHashtables
Get-PSResourceRepository
```

```Output
Name      Uri                                      Trusted Priority
----      ---                                      ------- --------
PSGallery https://www.powershellgallery.com/api/v2 True    10
Local     file:///D:/PSRepoLocal/                  True    20
PSGv3     https://www.powershellgallery.com/api/v3 True    50
```

### Example 4

This example registers a repository with credential information to be retrieved from a registered
**SecretManagement** vault, where **SecretStore** is the name of the vault and **TestSecret** is the
name of the stored secret.

You must have the **Microsoft.PowerShell.SecretManagement** module installed, have a registered
vault, and stored a secret within it. If setup correctly, the command
`Get-SecretInfo -Name 'TestSecret'` would return the secret.

The format of the secret must match the requirements of the repository. In some instances,
`TestSecret` might need storing as a **PSCredential** object with a username and password or token.
In others it may need storing as a **SecureString** representing just the token.

```powershell
$parameters = @{
  Name = 'PSGv3'
  Uri = 'https://www.powershellgallery.com/api/v3'
  Trusted = $true
  Priority = 50
  CredentialInfo = [Microsoft.PowerShell.PSResourceGet.UtilClasses.PSCredentialInfo]::new(
    'SecretStore', 'TestSecret')
}
Register-PSResourceRepository @parameters
Get-PSResourceRepository | Select-Object * -ExpandProperty CredentialInfo
```

```Output
Name           : PSGv3
Uri            : https://www.powershellgallery.com/api/v3
Trusted        : True
Priority       : 50
CredentialInfo : Microsoft.PowerShell.PSResourceGet.UtilClasses.PSCredentialInfo
VaultName      : SecretStore
SecretName     : TestSecret
Credential     :
```

## PARAMETERS

### -ApiVersion

Specifies the API version used by the repository. Valid values are:

- `V2` - uses the NuGet V2 API
- `V3` - uses the NuGet V3 API
- `ContainerRegistry` - used for Azure Container Registry
- `Local` - use this for file system based repositories
- `NugetServer` - use this for NuGet.Server based repositories

The `Register-PSResourceRepository` cmdlet should automatically detect the API version. This
parameter allows you to change the API version after you have registered a repository.

```yaml
Type: Microsoft.PowerShell.PSResourceGet.UtilClasses.PSRepositoryInfo+APIVersion
Parameter Sets: NameParameterSet
Aliases:
Accepted values: V2, V3, Local, NugetServer, ContainerRegistry

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CredentialInfo

A **PSCredentialInfo** object that includes the name of a vault and a secret that's stored in a
**Microsoft.PowerShell.SecretManagement** store.

```yaml
Type: Microsoft.PowerShell.PSResourceGet.UtilClasses.PSCredentialInfo
Parameter Sets: NameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CredentialProvider

This is a dynamic parameter that specifies the credential provider to use for the repository. This
parameter is only available when the repository being registered is an Azure Artifacts feed. Valid
values are:

- `None` - No credential provider defined
- `AzArtifacts` - Use the Azure Artifacts Credential Provider

If you don't use this parameter, the default value is `None`. If the repository URL contains
`pkgs.dev.azure.com` or `pkgs.visualstudio.com`, the command automatically registers the repository
with the **CredentialProvider** property set to `AzArtifacts`.

```yaml
Type: Microsoft.PowerShell.PSResourceGet.UtilClasses.CredentialProviderType
Parameter Sets: NameParameterSet
Accepted values: None, AzArtifacts
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

Overwrites a repository if it already exists.

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

Name of the repository to be registered. Can't be `PSGallery`.

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

### -PassThru

When specified, displays the successfully registered repository and its information.

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

### -Priority

Specifies the priority ranking of the repository. Valid priority values range from 0 to 100. Lower
values have a higher priority ranking. The default value is `50`.

Repositories are sorted by priority then by name. When searching for resources across multiple
repositories, the **PSResourceGet** cmdlets search the repositories using this sort order and
return the first match found.

```yaml
Type: System.Int32
Parameter Sets: NameParameterSet, PSGalleryParameterSet
Aliases:

Required: False
Position: Named
Default value: 50
Accept pipeline input: False
Accept wildcard characters: False
```

### -PSGallery

When specified, registers **PSGallery** repository.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PSGalleryParameterSet
Aliases:

Required: True
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Repository

Specifies an array of hashtables that contain repository information. Use this parameter to register
multiple repositories at once. Each hashtable can only have keys associated with parameters for
the **NameParameterSet** or the **PSGalleryParameterSet**.

```yaml
Type: System.Collections.Hashtable[]
Parameter Sets: RepositoriesParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Trusted

Specifies whether the repository should be trusted.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NameParameterSet, PSGalleryParameterSet
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Uri

Specifies the location of the repository to be registered. The value must use one of the following
URI schemas:

- `https://`
- `http://`
- `ftp://`
- `file://`

```yaml
Type: System.String
Parameter Sets: NameParameterSet
Aliases:

Required: True
Position: 1
Default value: None
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

### None

## OUTPUTS

### Microsoft.PowerShell.PSResourceGet.UtilClasses.PSRepositoryInfo

By default, the cmdlet produces no output. When you use the **PassThru** parameter, the cmdlet
returns a **PSRepositoryInfo** object.

## NOTES

Repositories are unique by **Name**. Attempting to register a repository with same name results in
an error.

## RELATED LINKS

[Microsoft.PowerShell.SecretManagement](/powershell/utility-modules/secretmanagement/overview)


