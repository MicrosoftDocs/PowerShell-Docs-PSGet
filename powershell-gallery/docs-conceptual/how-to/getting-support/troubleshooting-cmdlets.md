---
description: This article provide information and steps for troubleshooting errors using the PowerShell Gallery
ms.date: 09/09/2025
title: Troubleshooting cmdlets
---
# Troubleshooting cmdlets

## How to resolve "WARNING: Package 'your package name' failed to download" issue

It is reported that `Install-Module` or `Update-Module` sometimes fails on some machines. Based on
our investigation, it is something to do with the networking connection. Recently we updated NuGet
provider so that it can reliably download packages. You can follow the instructions below to install
the latest build of NuGet provider and then install or update your module. Let's use 'Azure' module
as an example below.

```powershell
Install-PackageProvider NuGet -MinimumVersion 2.8.5.206 -Force
Launch new PowerShell Console
Update-Module Azure -Verbose
```

## Required network endpoints

The `Install-*` and `Update-*` cmdlets require internet access to connect to the network endpoints
used by the PowerShell Gallery.

[!INCLUDE [TLS 1.2 Requirement](../includes/tls-gallery.md)]

Ensure that your network access policies allow you to connect to TCP port 443 of the following
endpoints.

Hosts required for package discovery and download:

- `cdn.oneget.org`
- `cdn.powershellgallery.com`

Hosts required when using the PowerShell Gallery website:

- `*.powershellgallery.com` - website
- `go.microsoft.com` and `aka.ms` - redirection services

> [!NOTE]
> These endpoints have changed. The old endpoints that ended with `azureedge.net` are no longer
> supported.

Cmdlets that interact with the PowerShell Gallery can fail with unexpected errors when there is an
outage of the PowerShell Gallery services. To see the current status of the PowerShell Gallery, see
the [PowerShell Gallery Status][01] page on GitHub.

<!-- link references -->
[01]: https://github.com/PowerShell/PowerShellGallery/blob/master/psgallery_status.md
