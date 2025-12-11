---
description: This article describes how PSResourceGet integrates with the Azure Artifacts Credential Provider with ADO feeds.
ms.date: 12/10/2025
ms.topic: reference
title: Use the Azure Artifacts Credential Provider with Azure Artifacts feeds
---

# Use the Azure Artifacts Credential Provider with Azure Artifacts feeds

PSResourceGet v1.2.0-preview5 adds support for using the Azure Artifacts Credential Provider with
Azure Artifacts feeds.

The `Register-PSResourceRepository` and `Set-PSResourceRepository` cmdlets have a new dynamic
parameter `-CredentialProvider` that allows you to configure the **CredentialProvider** property of
a **PSResourceRepository**. This dynamic parameter only works with repositories that aren't
**ContainerRegistry** repositories or the **PSGallery** repository.

PSResourceGet automatically sets the value **CredentialProvider** to `AzArtifacts` when the URI
contains `pkgs.dev.azure.com` or `pkgs.visualstudio.com`. This check is done when registering a new
repository or when PSResourceGet reads the registered repositories.

The Credential Provider can be bypassed by setting the `-CredentialProvider` parameter to `None`.

## How it works

PSResourceGet looks for the credential provider any time network credentials are set in the
following priority order:

1. Credentials passed in via **Credential** parameter.
1. Credentials retrieved via the Azure Artifacts Credential Provider.
1. Credentials retrieved from SecretManagement.

If the **CredentialProvider** property of a repository is set to `AzArtifacts`, PSResourceGet
attempts to find the Credential Provider file on the machine in the following order:

1. Checking the environment variable `NUGET_PLUGIN_PATHS`
1. Checking the default locations: `$env:USERPROFILE\.nuget\plugins`
1. Checking the location where Visual Studio is installed:
   `common7\IDE\CommonExtensions\Microsoft\NuGet`

For more information about the discovery process, see _Convention-based discovery_ section of
[NuGet cross platform plugins][01].

After locating the Credential Provider, PSResourceGet uses one of the following commands to call the
provider:

- `CredentialProvider.Microsoft.exe -Uri <uri> -NonInteractive -IsRetry -F Json`
- `dotnet CredentialProvider.Microsoft.dll -Uri <uri> -NonInteractive -IsRetry -F Json`

`CredentialProvider.Microsoft.exe` is used on Windows machines, while `dotnet` works on all supported
platforms.

## Dependencies

For this feature to work, the Azure Artifacts Credential Provider must be installed on the machine.
The provider might already be available if you have Visual Studio or the .NET SDK installed. For
more information about installing the Azure Artifacts Credential Provider, see the [README][02] file
in the GitHub repository for the Azure Artifacts Credential Provider.

<!-- link references -->
[01]: /nuget/reference/extensibility/nuget-cross-platform-plugins#convention-based-discovery
[02]: https://github.com/microsoft/artifacts-credprovider/blob/master/README.md
