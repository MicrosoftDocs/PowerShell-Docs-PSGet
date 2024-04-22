---
description: This article demonstrates how to use SecretStore credentials with an Artifactory repository.
ms.date: 06/28/2023
title: How to add credentials to repositories in Microsoft.PowerShell.PSResourceGet
---
# How to add credentials to repositories with Microsoft.PowerShell.PSResourceGet

Microsoft.PowerShell.PSResourceGet allows you to register private repositories that contain
installable PSResource packages. Typically, private repositories require you to have credentials to
access them. [JFrog Artifactory][02] is a service that allows you to create private NuGet
repositories. This article demonstrates how to use **SecretStore** credentials with an Artifactory
repository.

## Prerequisites

- Set up a **SecretManagement** vault
- Create a credential to be used with Artifactory

> [!NOTE]
> This example uses **SecretStore**, but you can use any **SecretManagement** extension vault and
> with any repository.

Use `Get-SecretInfo` to verify that the vault contains the credential you need.

```powershell
Get-SecretInfo
```

```Output
Name          Type         VaultName
----          ----         ---------
JFrogCred     PSCredential SecretStore
```

For more information about **SecretManagement**, see
[Overview of the SecretManagement and SecretStore modules][01].

## Add a credential to a PSResourceRepository

Once you have a store credential, you need to add it to the registered repository. Create a
**PSCredentialInfo** object that references the store credential. You can register a new
**PSResourceRepository** or add the credential to an existing one using the
`Set-PSResourceRepository` cmdlet. This example shows how to register a new repository.

```powershell
$registerPSResourceRepositorySplat = @{
  Name = 'artifactory'
  Uri = 'https://myaccount.jfrog.io/artifactory/api/nuget/v3/myrepository'
  Trusted = $true
  CredentialInfo = [Microsoft.PowerShell.PSResourceGet.UtilClasses.PSCredentialInfo]::new(
    'SecretStore', 'JFrogCred')
}
Register-PSResourceRepository @registerPSResourceRepositorySplat
```

Run `Get-PSResourceRepository` to see the registered repositories.

```powershell
PS C:\Users > Get-PSResourceRepository
```

```Output
Name        Uri                                                                 Trusted Priority
----        ---                                                                 ------- --------
artifactory https://myaccount.jfrog.io/artifactory/api/nuget/v3/myrepository    True    50
PSGallery   https://www.powershellgallery.com/api/v2                            False   50
```

## Publishing resources to the repository

To publish resources to a secured repository, you must provide the credential you configured. This
example show how to publish a resource to the `artifactory` repository using your stored credential.

```powershell
Publish-PSResource -Path .\Get-Hello\ -Repository artifactory -ApiKey (Get-Secret JFrogPublish)
```

Once you have provided the credential, **PowerShellGet** reuses the credential for subsequent
commands that target the same repository. The following examples show how to find and install a
resource. Notice that the credential isn't required.

```powershell
Find-PSResource -Name Get-Hello -Repository artifactory
```

```Output
Name      Version Prerelease Repository  Description
----      ------- ---------- ----------  -----------
Get-Hello 0.0.2.0            artifactory test module
```

```powershell
PS C:\Users> Install-PSResource Get-Hello
```

<!-- link references -->
[01]: /powershell/utility-modules/secretmanagement/overview
[02]: https://jfrog.com/artifactory/
