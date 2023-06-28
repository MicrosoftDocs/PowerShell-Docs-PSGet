---
description: This article lists the repositories that have been tested with PowerShellGet v3 and how to configure them.
ms.date: 06/28/2023
title: Supported repository configurations
---

# Supported repository configurations

The **Microsoft.PowerShell.PSResourceGet** module, also known as **PowerShellGet v3**, works with
NuGet package repositories and local file stores. In general, the cmdlets should work with any
artifact repository that supports the NuGet protocol. However, not all NuGet repositories support
all the features of PSResourceGet.

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
API key, see [Quickstart: Create and publish a NuGet package][05].

### NuGet.org limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
NuGet.org repositories:

- Wildcard search by name, for example:
  - `Find-PSResource -Name '*'`
  - `Find-PSResource -Name 'Az*'`
  - `Find-PSResource -Name 'Az*' -Tag compute`
- Search by command or DSC resource name, for example:
  - `Find-PSResource -Command commandname`
  - `Find-PSResource -DSCResource dscresourcename`

## Azure Artifacts

Azure Artifacts is a feature of Azure DevOps. Azure Artifacts enables developers to share their code
efficiently and manage all their packages from one place. With Azure Artifacts, developers can
publish packages to their feeds and share it within the same team, across organizations, and even
publicly.

Azure Artifacts supports the NuGet v3 protocol. Azure Artifacts feed URIs use the following format:

`https://dev.azure.com/<organization-name>/<project-name>/_artifacts/feed/<feed-name>`

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
about getting an API key, see [Connect to feed as a PowerShell repository][03].

### Azure Artifacts limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
Azure Artifacts repositories:

- Wildcard search by name, for example:
  - `Find-PSResource -Name '*'`
  - `Find-PSResource -Name 'Az*'`
  - `Find-PSResource -Name 'Az*' -Tag compute`
- Search by tag, for example:
  - `Find-PSResource -Tag compute`
- Search by command or DSC resource name, for example:
  - `Find-PSResource -Command commandname`
  - `Find-PSResource -DSCResource dscresourcename`
- Publish packages to a feed
  - Returns a `400 (Bad Request)` error

## GitHub Packages

GitHub Packages is a software package hosting service that allows you to host your software packages
privately or publicly and use packages as dependencies in your projects. For more information, see
[Introduction to GitHub Packages][06].

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
must be associated with a user account or organization. Packages published to the feed can be
associated with a repository.

You must use credentials for all operations with a GitHub Packages feed. For more information, see
the _Authenticating to GitHub Packages_ section of [Working with the NuGet registry][07].

### GitHub Packages limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
GitHub Packages repositories:

- Wildcard search by name, for example:
  - `Find-PSResource -Name '*'`
- Search by tag For example:
  - `Find-PSResource -Tag compute`
- Search by command or DSC resource name, for example:
  - `Find-PSResource -Command commandname`
  - `Find-PSResource -DSCResource dscresourcename`
- Publish packages to a feed
  - The package is published as a private package. After publishing, you can use the GitHub
    interface to link it to the desired repository and change the visibility.

## JFrog Artifactory

[JFrog Artifactory][09] is a is a hosting service for NuGet repositories. Artifactory feeds use the
NuGet v3 protocol. The feed URI has the following format:

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
[Creating Access Tokens in Artifactory][10].

### JFrog Artifactory limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for JFrog
Artifactory repositories:

- Wildcard search by name, for example:
  - `Find-PSResource -Name '*'`
- Search by command or DSC resource name, for example:
  - `Find-PSResource -Command commandname`
  - `Find-PSResource -DSCResource dscresourcename`

## MyGet.org

[MyGet.org][11] is a hosting service for NuGet repositories.

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
have an account and an API key. For more information about, see [MyGet Security][08].

### MyGet limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for MyGet
repositories:

- Wildcard search by name, for example:
  - `Find-PSResource -Name '*'`
  - `Find-PSResource -Name 'Az*'`
  - `Find-PSResource -Name 'Az*' -Tag compute`
- Search by tag, for example:
  - `Find-PSResource -Tag compute`
- Search by command or DSC resource name, for example:
  - `Find-PSResource -Command commandname`
  - `Find-PSResource -DSCResource dscresourcename`
- Publish packages to a feed
  - Returns a `403 (Forbidden)` error

## File-share-based repositories

You can create your own package repository as a local file share. For more information, see
[Working with local PSRepositories][02]. You access file-share-based repositories using local
filesystem APIs or remote filesystem protocols, such as SMB or NFS.

The permissions set by the filesystem and the file sharing protocol control access to the files. You
can't use the **Credential** parameter of the **Microsoft.PowerShell.PSResourceGet** cmdlets to
access to the files using alternate user credentials.

The **Microsoft.PowerShell.PSResourceGet** module supports all search scenarios for file share
repositories.

## Self-hosted NuGet repositories

You can create your own package repository by hosting your own NuGet server. **NuGet.Server** is a
package provided by the .NET Foundation. You can use this package to create an ASP.NET application
that hosts a package feed on any Windows server running IIS. This server uses the NuGet v2 protocol.
For more information, see [Working with local PSRepositories][02].

The **Microsoft.PowerShell.PSResourceGet** cmdlets don't currently support this type of repository.

<!-- The feed URI has the following format:

`http://<server-host-name>/nuget`

Use the following command to register your self-hosted server as a PSResource repository:

```powershell
$params = @{
    Name = 'SelfHostedNuGet'
    Uri  = 'https://host.contoso.net/nuget'
}
Register-PSResourceRepository @params
```

You don't need credentials to search, download, or install packages from your self-hosted server.
You can require an API key to publish packages to your server. For more information about getting a
NuGet API key, see the _Adding packages to the feed externally_ section of
[Using NuGet.Server to Host NuGet Feeds][04].

### Self-hosted NuGet server limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
self-hosted NuGet servers:

- Wildcard search by name, for example:
  - `Find-PSResource -Name '*'`
  - `Find-PSResource -Name 'Az*'`
  - `Find-PSResource -Name 'Az*' -Tag compute`
- Search by command or DSC resource name, for example:
  - `Find-PSResource -Command commandname`
  - `Find-PSResource -DSCResource dscresourcename`
-->

<!-- link references -->
[01]: ../how-to/publishing-packages/publishing-a-package.md
[02]: ../how-to/working-with-local-psrepositories.md
[03]: /azure/devops/artifacts/tutorials/private-powershell-library#connect-to-feed-as-a-powershell-repository
[04]: /nuget/hosting-packages/nuget-server#adding-packages-to-the-feed-externally
[05]: /nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli#get-your-api-key
[06]: https://docs.github.com/packages/learn-github-packages/introduction-to-github-packages
[07]: https://docs.github.com/packages/working-with-a-github-packages-registry/working-with-the-nuget-registry#authenticating-to-github-packages
[08]: https://docs.myget.org/docs/reference/security
[09]: https://jfrog.com/artifactory/
[10]: https://jfrog.com/help/r/how-to-generate-an-access-token-video/artifactory-creating-access-tokens-in-artifactory
[11]: https://www.myget.org/
