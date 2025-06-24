---
description: This article provide examples for using PSResourceGet with Azure Container Registry (ACR) repositories.
ms.date: 03/27/2024
ms.topic: reference
title: Use ACR repositories with PSResourceGet
---

# Use ACR repositories with PSResourceGet

Azure Container Registry (ACR) allows you to build, store, and manage container images and artifacts
in a private registry for all types of container deployments. Version 1.1.1 of the
**Microsoft.PowerShell.PSResourceGet** module adds support for Azure Container registries as a
PSResource repository. For more information about Azure Container registries, see
[Managed container registries][04].

## Prerequisites

To create an ACR repository, you must have:

- Az PowerShell or Az CLI
- Sufficient rights in your Azure subscription to create an ACR repository.

To use an ACR repository with PSResourceGet, you must have:

- Version 1.1.1 of the **Microsoft.PowerShell.PSResourceGet** module
- An Azure account with the necessary permissions to access the ACR repository. ACR-based
  repositories are private repositories and require credentials for access. Users of the repository
  must have the necessary permissions to access the repository. The user needs to have `ACRPull`
  permissions to get resources and `ACRPush` permissions to publish resources. For more information,
  see [Registry roles and permissions][05].

## Install the PSResourceGet module

To use an ACR repository you must install the latest release of the
**Microsoft.PowerShell.PSResourceGet** module from the PowerShell Gallery.

If you have a previous version of **Microsoft.PowerShell.PSResourceGet** installed, use the
following command to install the latest release.

```powershell
$installPSResourceSplat = @{
    Repository = 'PSGallery'
    Name = 'Microsoft.PowerShell.PSResourceGet'
}
Install-PSResource @installPSResourceSplat
```

Using the following command, you can also install the module using **PowerShellGet**.

```powershell
$installModuleSplat = @{
    Repository = 'PSGallery'
    Name = 'Microsoft.PowerShell.PSResourceGet'
}
Install-Module @installModuleSplat
```

## Create an Azure Container Registry

In this example, you create a _Basic_ registry, which is a cost-optimized option for developers. You
might want to choose a different tier depending on your storage and image throughput requirements.
For details on available service tiers (SKUs), see [Container registry service tiers][06].

```powershell
# Sign into your Azure account (if necessary)
Connect-AzAccount
# Create a resource group (if necessary)
New-AzResourceGroup -Name myResourceGroup -Location EastUS
# Create a container registry
$newAzContainerRegistrySplat = @{
    ResourceGroupName = 'myResourceGroup'
    Name = 'psresourcedemo'
    EnableAdminUser = $true
    Sku = 'Basic'
}
$registry = New-AzContainerRegistry @newAzContainerRegistrySplat
```

You can run the Azure commands in [Azure Cloud Shell][01] or from any computer that has Azure
PowerShell installed.

## Register the repository

To register an ACR repository, you must know the **LoginServer** name of the ACR. Use the following
commands to register an ACR repository as a PSResource repository.

```powershell
$myAcr = Get-AzContainerRegistry -Name myAcr
$acrUrl = "https://$($myAcr.LoginServer)"
Register-PSResourceRepository -Name ACRDemoRepo -Uri $acrUrl
```

## Authentication

Registering the a PSResource repository does not require any credentials. However, the first time
you perform an operation on the registered repository, you are prompted to login. If you are not
authenticated yet, will be prompted to authenticate the first time you perform an operation on the
repository.

To authenticate before you access the repository, use the `Connect-AzAccount` command. For more
information, see [Connect-AzAccount (Az.Accounts)][08].

Once you are authenticated, you can use the repository interactively or in a pipeline. You are not
prompted to authenticate again until your access token expires.

## Use the ACR repository interactively

Using the ACR repository interactively is similar to using any other PSResource repository. You can
search for, install, and publish packages.

### Publish a package

Use the `Publish-PSResource` command to publish a package to an ACR repository. You only need to
provide a path to the resource and the name of the repository.

```powershell
$publishPSResourceSplat = @{
    Path = 'D:\MyModule'
    Repository = 'ACRDemoRepo'
}
Publish-PSResource @publishPSResourceSplat
```

For more information, see [Publish-PSResource][12].

### Search for packages

Use the `Find-PSResource` command to search for packages in the ACR repository. For more
information, see [Find-PSResource][10].

- Find by name

  ```powershell
  Find-PSResource -Name MyModule -Repository ACRDemoRepo
  ```

- Find by version

  ```powershell
  Find-PSResource -Name MyModule -Version '3.0.0' -Repository ACRDemoRepo
  ```

- Find a prerelease version

  ```powershell
  Find-PSResource -Name MyModule -Version '4.0.0-preview.1' -Repository ACRDemoRepo
  ```

- Find by version wildcard

  ```powershell
  Find-PSResource -Name MyModule -Version '*' -Repository ACRDemoRepo
  ```

- Find by version range

  This example searches for versions between 2.0.0 and 3.0.0 inclusive.

  ```powershell
  Find-PSResource -Name MyModule -Version '[2.0.0,3.0.0]' -Repository ACRDemoRepo
  ```

  For more information about version ranges, see [NuGet Package Version Reference][07].

### Install a package

Use the `Install-PSResource` command to install packages from the ACR repository. For more
information, see [Install-PSResource][11].

- Install by name

  ```powershell
  Install-PSResource -Name MyModule -Repository ACRDemoRepo
  ```

- Install by version

  ```powershell
  Install-PSResource -Name MyModule -Version 3.0.0 -Repository ACRDemoRepo
  ```

## Limitations

The **Microsoft.PowerShell.PSResourceGet** module doesn't support the following scenarios for
ACR repositories:

- Find by name using wildcards
  - `Find-PSResource -Name * -Repository ACRDemoRepo`
  - `Find-PSResource -Name PartialName* -Repository ACRDemoRepo`
- Find by tag value
  - `Find-PSResource -Tag TagValue -Repository ACRDemoRepo`
- Find by command
  - `Find-PSResource -Command CommandName -Repository ACRDemoRepo`
- Find by DSC resource name
  - `Find-PSResource -DscResourceName ResourceName -Repository ACRDemoRepo`

<!-- link references -->
[01]: /azure/cloud-shell/overview
[04]: /azure/container-registry/container-registry-intro
[05]: /azure/container-registry/container-registry-roles?tabs=azure-powershell
[06]: /azure/container-registry/container-registry-skus
[07]: /nuget/concepts/package-versioning?tabs=semver20sort#version-ranges
[08]: /powershell/module/az.accounts/connect-azaccount
[10]: xref:Microsoft.PowerShell.PSResourceGet.Find-PSResource
[11]: xref:Microsoft.PowerShell.PSResourceGet.Install-PSResource
[12]: xref:Microsoft.PowerShell.PSResourceGet.Publish-PSResource
