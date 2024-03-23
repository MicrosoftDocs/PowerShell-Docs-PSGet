---
description: This article lists the repositories that have been tested with PowerShellGet v3 and how to configure them.
ms.date: 03/26/2024
ms.topic: reference
title: Supported repository configurations
---

# PSResourceGet supported repositories

The **Microsoft.PowerShell.PSResourceGet** module works with NuGet package repositories and local
file stores. In general, the cmdlets should work with any artifact repository that supports the
NuGet protocol. However, not all NuGet repositories support all the features of PSResourceGet.

The following repositories have been tested with PSResourceGet.

## The PowerShell Gallery

The PowerShell Gallery is the default repository for PSResourceGet. The Gallery is a NuGet
repository that uses the NuGet v2 protocol.

You don't need credentials to search, download, or install packages from the PowerShell Gallery. You
must create an account and API key to publish packages to the PowerShell Gallery. For more
information, see [Creating and publishing an item][01].

The PowerShell Gallery supports all the features of PSResourceGet.

## NuGet.org

NuGet.org is a public host of NuGet packages used by millions of .NET and .NET Core developers every
day. NuGet.org provides a NuGet repository that uses the NuGet v3 protocol.

Use the following command to register NuGet.org as a PSResource repository:

```powershell
$params = @{
    Name = 'NuGetGallery'
    Uri  = 'https://api.nuget.org/v3/index.json'
}
Register-PSResourceRepository @params
```

You don't need credentials to search, download, or install packages from NuGet.org. You must create
an account and API key to publish packages to NuGet.org. For more information about getting a NuGet
API key, see [Quickstart: Create and publish a NuGet package][08].

### NuGet.org limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
NuGet.org repositories:

- Wildcard search by name
  - `Find-PSResource -Name *`
  - `Find-PSResource -Name PartialName*`
- Search by command name
  - `Find-PSResource -Command CommandName`
- Search by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName`

### Publish to NuGet.org

You don't need to use the **Credential** parameter of the `Publish-PSResource` cmdlet, but you must
use the **ApiKey** parameter. You can provide credentials, but NuGet.org doesn't use them. To create
an API key, see [How to publish NuGet packages][07]. For example:

```powershell
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'AzArtifactFeed'
    ApiKey = '<nuget-api-key>'
}
Publish-PSResource @publishPSResourceSplat
```

> [!CAUTION]
> You should never use plaintext credentials in a script. Use the **SecretManagement** module or
> other methods to store and retrieve your credentials securely. For more information, see
> [Overview of the SecretManagement and SecretStore modules][09].

## Azure Artifacts

Azure Artifacts is a feature of Azure DevOps. Azure Artifacts enables developers to share their code
efficiently and manage all their packages from one place. With Azure Artifacts, developers can
publish packages to their feeds and share it within the same team, across organizations, and even
publicly.

Azure Artifacts supports the NuGet v3 protocol. Azure Artifacts feed URIs use the following format:

`https://dev.azure.com/<organization-name>/<project-name>/_packaging/<feed-name>/v3/index.json`

Substitute the placeholder values with your organization, project, and feed names. For example, use
the following command to register an Azure Artifacts feed as a PSResource repository:

```powershell
$params = @{
    Name = 'MyAdoFeed'
    Uri  = 'https://pkgs.dev.azure.com/MyOrg/MyProject/_packaging/MyFeed/nuget/v3/index.json'
}
Register-PSResourceRepository @params
```

Azure DevOps allows you to create public or private feeds for your Azure Artifacts. You don't need
credentials to search, download, or install packages from an Azure Artifacts public feed. To publish
artifacts or access private feeds, you must have an account and an API key. For more information
about getting an API key, see [Connect to feed as a PowerShell repository][05].

### Azure Artifacts limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
Azure Artifacts repositories:

- Wildcard search by name
  - `Find-PSResource -Name '*'`
  - `Find-PSResource -Name 'PartialName*'`
- Search by tag
  - `Find-PSResource -Tag compute`
- Search by command name
  - `Find-PSResource -Command CommandName`
- Search by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName`

### Publish to Azure Artifacts

You must use the **Credential** and **ApiKey** parameters of the `Publish-PSResource` cmdlet to
publish packages to an Azure Artifacts feed. The credential must be a personal access token (PAT)
that has the **Packaging (read, write, and manage)** scope. For more information, see
[Use Azure Artifacts feeds as a private PowerShell repository][04]. The value of the **ApiKey**
parameter is not important. It can be any arbitrary string, but it must be included. For example:

```powershell
$patToken = ConvertTo-SecureString -String '<personal-access-token>' -AsPlainText -Force
$Credential = [System.Management.Automation.PSCredential]::new('<username>', $patToken)
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'AzArtifactFeed'
    ApiKey = 'AzureDevOps'
    Credential = $Credential
}
Publish-PSResource @publishPSResourceSplat
```

For this example `<personal-access-token>` is the token you created for publishing. `<username>` is
your Azure DevOps user name.

> [!CAUTION]
> You should never use plaintext credentials in a script. Use the **SecretManagement** module or
> other methods to store and retrieve your credentials securely. For more information, see
> [Overview of the SecretManagement and SecretStore modules][09].

## Azure Container Registry

Azure Container Registry (ACR) allows you to build, store, and manage container images and artifacts
in a private registry for all types of container deployments. Version 1.1.0-preview.1 of the
**Microsoft.PowerShell.PSResourceGet** module adds support for Azure Container registries as a
PSResource repository.

ACR-based repositories are private repositories and require credentials for access. Users of the
repository must have the necessary permissions to access the repository. The user needs to have
`ACRPull` permissions to get resources and `ACRPush` permissions to publish resources. For more
information, see [Registry roles and permissions][03].

To register an ACR repository, you must know the **LoginServer** name of the ACR. Use the following
commands to register an ACR repository as a PSResource repository.

```powershell
$myAcr = Get-AzContainerRegistry -Name myAcr
$acrUrl = "https://$($myAcr.LoginServer)"
Register-PSResourceRepository -Name ACRDemoRepo -Uri $acrUrl
```

After you register the repository, the first time you perform an operation on the registered
repository in a new session you are prompted to login.

### Use the ACR repository

Using the ACR-based repository is similar to using other repositories. You can search for resources,
install resources, and publish resources. The first time you perform an operation on the registered
repository in a new session, you are prompted to login. For more detailed examples using ACR, see
[Working with Azure Container Registry repositories][11].

### ACR limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
ACR repositories:

- Wildcard search by name
  - `Find-PSResource -Name * -Repository ACRDemoRepo`
  - `Find-PSResource -Name PartialName* -Repository ACRDemoRepo`
- Find by tag value
  - `Find-PSResource -Tag TagValue -Repository ACRDemoRepo`
- Find by command
  - `Find-PSResource -Command CommandName -Repository ACRDemoRepo`
- Find by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName -Repository ACRDemoRepo`

## GitHub Packages

GitHub Packages is a software package hosting service that allows you to host your software packages
privately or publicly and use packages as dependencies in your projects. For more information, see
[Introduction to GitHub Packages][13].

The GitHub Packages feed is a NuGet repository that uses the NuGet v3 protocol. The feed URI has the
following format: `https://nuget.pkg.github.com/<namespace>/index.json`. Replace `<namespace>` with
the name of the personal account or organization to which your packages are scoped. For example, use
the following command to register an GitHub Packages feed as a PSResource repository:

```powershell
$params = @{
    Name = 'MyGitHubFeed'
    Uri  = 'https://nuget.pkg.github.com/MyGitHubOrg/index.json'
}
Register-PSResourceRepository @params
```

The Github Packages service doesn't support NuGet feeds that are scoped to a repository. The feed
must be associated with a user account or organization.

You must use credentials for all operations with a GitHub Packages feed. For more information, see
the _Authenticating to GitHub Packages_ section of [Working with the NuGet registry][14].

### GitHub Packages limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
GitHub Packages repositories:

- Wildcard search by name
  - `Find-PSResource -Name *`
  - `Find-PSResource -Name PartialName*`
- Search by tag For example:
  - `Find-PSResource -Tag compute`
- Search by command name
  - `Find-PSResource -Command CommandName`
- Search by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName`
- Publish packages to a feed
  - The package is published as a private package. After publishing, you can use the GitHub
    interface to link it to the desired repository and change the visibility.

### Publish to GitHub Packages

You can use either the **Credential** or **ApiKey** parameter of the `Publish-PSResource` cmdlet to
publish packages to a GitHub Package feed. You must create a personal access token (PAT) with the
necessary scopes enabled. For more information on scopes and permissions, see
[About permissions for GitHub Packages][12].

If you use the **Credential** parameter, the value must be a **PSCredential** object that contains
your username and the PAT. For this example `<personal-access-token>` is the token you created for
publishing. `<username>` is your GitHub username.

```powershell
$patToken = ConvertTo-SecureString -String '<personal-access-token>' -AsPlainText -Force
$Credential = [System.Management.Automation.PSCredential]::new('<username>', $patToken)
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'MyGitHubFeed'
    Credential = $Credential
}
Publish-PSResource @publishPSResourceSplat
```

If you use the **ApiKey** parameter, the value must be the plaintext PAT. For example:

```powershell
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'MyGitHubFeed'
    ApiKey = '<personal-access-token>'
}
Publish-PSResource @publishPSResourceSplat
```

> [!CAUTION]
> You should never use plaintext credentials in a script. Use the **SecretManagement** module or
> other methods to store and retrieve your credentials securely. For more information, see
> [Overview of the SecretManagement and SecretStore modules][09].

## JFrog Artifactory

[JFrog Artifactory][16] is a hosting service for NuGet repositories. Artifactory feeds use the NuGet
v3 protocol. The feed URI has the following format:

`https://<jfrog-account>.jfrog.io/artifactory/api/nuget/v3/nuget/index.json`.

Replace `<jfrog-account>` with the name of your JFrog account. For example, use the following
command to register an Artifactory feed as a PSResource repository:

```powershell
$params = @{
    Name = 'MyJFrogFeed'
    Uri  = 'https://myjfrogaccount.jfrog.io/artifactory/api/nuget/v3/nuget/index.json'
}
Register-PSResourceRepository @params
```

You must use credentials for all operations with a JFrog Artifactory feed. For more information, see
[Creating Access Tokens in Artifactory][17].

### JFrog Artifactory limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for JFrog
Artifactory repositories:

- Wildcard search by name
  - `Find-PSResource -Name *`
  - `Find-PSResource -Name PartialName*`
- Search by command name
  - `Find-PSResource -Command CommandName`
- Search by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName`

### Publish to JFrog Artifactory

You must use the **Credential** parameter of the `Publish-PSResource` cmdlet to publish packages to
a JFrog Artifactory feed. The value of the **Credential** parameter must be a **PSCredential**
object constructed from your email address and the access token that was created when you configured
the NuGet repository in your JFrog account. For example:

```powershell
$patToken = ConvertTo-SecureString -String '<jfrog-access-token>' -AsPlainText -Force
$Credential = [System.Management.Automation.PSCredential]::new('<email-address>', $patToken)
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'MyJFrogFeed'
    Credential = $Credential
}
Publish-PSResource @publishPSResourceSplat
```

For this example `<jfrog-access-token>` is the token you created for publishing. `<email-address>`
is your email address associated with your JFrog account.

> [!CAUTION]
> You should never use plaintext credentials in a script. Use the **SecretManagement** module or
> other methods to store and retrieve your credentials securely. For more information, see
> [Overview of the SecretManagement and SecretStore modules][09].

## MyGet.org

[MyGet.org][18] is a hosting service for NuGet repositories.

The **Microsoft.PowerShell.PSResourceGet** module supports MyGet feeds that use the NuGet v3
protocol. The feed URI has the following format:

`https://www.myget.org/F/<feedname>/api/v3/index.json`

Replace `<feedname>` with the name of your MyGet feed. For example, use the following command to
register a MyGet feed as a PSResource repository:

```powershell
$params = @{
    Name = 'PublicMyGetFeed'
    Uri  = 'https://www.myget.org/F/mypackagefeed/api/v3/index.json'
}
Register-PSResourceRepository @params
```

MyGet allows you to create public or private feeds. You don't need credentials to search, download,
or install packages from a public MyGet feed. To publish artifacts or access private feeds, you must
have an account and an API key. For more information about, see [MyGet Security][15].

### MyGet limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for MyGet
repositories:

- Wildcard search by name
  - `Find-PSResource -Name *`
  - `Find-PSResource -Name PartialName*`
- Search by tag
  - `Find-PSResource -Tag compute`
- Search by command name
  - `Find-PSResource -Command CommandName`
- Search by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName`

### Publish to a MyGet feed

You must use the **ApiKey** parameter of the `Publish-PSResource` cmdlet with personal access token
(PAT) to publish packages to a MyGet feed. The value of the **ApiKey** parameter must be a plaintext
PAT from your MyGet account that has read and write permissions. For more information about creating
access tokens, see [MyGet Security][15].

For example:

```powershell
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'MyGitHubFeed'
    ApiKey = '<personal-access-token>'
}
Publish-PSResource @publishPSResourceSplat
```

> [!CAUTION]
> You should never use plaintext credentials in a script. Use the **SecretManagement** module or
> other methods to store and retrieve your credentials securely. For more information, see
> [Overview of the SecretManagement and SecretStore modules][09].

## File-share-based repositories

You can create your own package repository as a local file share. For more information, see
[Working with local PSRepositories][02]. You access file-share-based repositories using local
filesystem APIs or remote filesystem protocols, such as SMB or NFS.

The permissions set by the filesystem and the file sharing protocol control access to the files. You
can't use the **Credential** parameter of the **Microsoft.PowerShell.PSResourceGet** cmdlets to
access to the files using alternate user credentials. When using a file sharing protocol, like NFS
or SMB, be sure to follow the recommended guidance for securing the protocol. For more information
about securing SMB on Windows, see [SMB security enhancements][10].

The **Microsoft.PowerShell.PSResourceGet** module supports all search scenarios for file share
repositories.

## Self-hosted NuGet repositories

You can create your own package repository by hosting your own NuGet server. **NuGet.Server** is a
package provided by the .NET Foundation. You can use this package to create an ASP.NET application
that hosts a package feed on any Windows server running IIS. This server uses the NuGet v2 protocol.
For more information, see [Working with local PSRepositories][02].

The **Microsoft.PowerShell.PSResourceGet** module supports NuGet.Server feeds. The feed URI has the
following format:

`http://<server-host-name>/nuget`

> [!NOTE]
> The **Microsoft.PowerShell.PSResourceGet** module expects the URI to end with `/nuget`. Any URI
> that ends with `/nuget` is treated as a NuGet.Server feed.

Use the following command to register your self-hosted server as a PSResource repository:

```powershell
$params = @{
    Name = 'SelfHostedNuGet'
    Uri  = 'https://host.contoso.net/nuget'
}
Register-PSResourceRepository @params
```

You don't need credentials to search, download, or install packages from your self-hosted server.

### Self-hosted NuGet server limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
self-hosted NuGet servers:

- General search that uses the **Type** parameter
  - `Find-PSResource` finds resources that match by name, but the value of the **Type** parameter is
    ignored. The cmdlet returns all modules and scripts that match the name.
- Search by command name
  - `Find-PSResource -Command CommandName`
- Search by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName`

### Publish to a NuGet.Server instance

When you first setup a NuGet.Server instance, there is no API key defined and pushing packages to
the feed is disabled. To configure an API key, see the _Adding packages to the feed_ section of
[Using NuGet.Server to Host NuGet Feeds][06]. The following example shows how to publish a package
using the configured API key.

```powershell
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'MyGitHubFeed'
    ApiKey = '<apikey>'
}
Publish-PSResource @publishPSResourceSplat
```

> [!CAUTION]
> You should never use plaintext credentials in a script. Use the **SecretManagement** module or
> other methods to store and retrieve your credentials securely. For more information, see
> [Overview of the SecretManagement and SecretStore modules][09].

<!-- link references -->
[01]: ../how-to/publishing-packages/publishing-a-package.md
[02]: ../how-to/working-with-local-psrepositories.md
[03]: /azure/container-registry/container-registry-roles?tabs=azure-powershell
[04]: /azure/devops/artifacts/tutorials/private-powershell-library?view=azure-devops&preserve-view=true#create-a-personal-access-token
[05]: /azure/devops/artifacts/tutorials/private-powershell-library#connect-to-feed-as-a-powershell-repository
[06]: /nuget/hosting-packages/nuget-server#adding-packages-to-the-feed-externally
[07]: /nuget/nuget-org/publish-a-package#create-an-api-key
[08]: /nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli#get-your-api-key
[09]: /powershell/utility-modules/secretmanagement/overview
[10]: /windows-server/storage/file-server/smb-security
[11]: how-to/use-acr-repository.md
[12]: https://docs.github.com/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries
[13]: https://docs.github.com/packages/learn-github-packages/introduction-to-github-packages
[14]: https://docs.github.com/packages/working-with-a-github-packages-registry/working-with-the-nuget-registry#authenticating-to-github-packages
[15]: https://docs.myget.org/docs/reference/security
[16]: https://jfrog.com/artifactory/
[17]: https://jfrog.com/help/r/how-to-generate-an-access-token-video/artifactory-creating-access-tokens-in-artifactory
[18]: https://www.myget.org/
