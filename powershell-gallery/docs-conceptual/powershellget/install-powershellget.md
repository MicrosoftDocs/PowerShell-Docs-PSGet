---
description: This article explains how install PowerShellGet.
ms.date: 11/10/2023
title: How to Install PowerShellGet
---
# How to Install PowerShellGet

## Prerequisites

Ensure that you have a version of **PowerShellGet** and **PackageManagement** newer than 1.0.0.1
installed. The latest stable versions are 2.2.5 for **PowerShellGet** and 1.4.8.1 for
**PackageManagement**.

If you're running Windows PowerShell 5.1 with **PowerShellGet** 1.0.0.1, see
[Update PowerShellGet for Windows PowerShell 5.1](update-powershell-51.md).

To access the PowerShell Gallery, you must use Transport Layer Security (TLS) 1.2 or higher. Use the
following command to enable TLS 1.2 in your PowerShell session.

```powershell
[Net.ServicePointManager]::SecurityProtocol =
    [Net.ServicePointManager]::SecurityProtocol -bor
    [Net.SecurityProtocolType]::Tls12
```

Add this command to your PowerShell profile script to ensure TLS 1.2 is configured for every
PowerShell session. For more information about profiles, see [about_Profiles][01].

If you're running PowerShell 6.0 or later, you already have a newer version of **PowerShellGet** and
**PackageManagement** installed. You can upgrade to a newer version if necessary or install the
preview release. You should always install the latest stable release.

Use the following command to see what version is installed.

```powershell
Get-Module PowerShellGet, PackageManagement -ListAvailable
```

The following output shows that the latest stable version needs to be installed.

```Output
    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version  Name               ExportedCommands
---------- -------  ----               ----------------
Binary     1.0.0.1  PackageManagement  {Find-Package, Get-Package, ...
Script     1.0.0.1  PowerShellGet      {Install-Module, Find-Module, ...
```

## Install the latest stable release

To install the latest versions of these modules run the following:

```powershell
Install-Module PowerShellGet -Force -AllowClobber
```

## Install Microsoft.PowerShell.PSResourceGet

**Microsoft.PowerShell.PSResourceGet** is the new package management solution for PowerShell. With
this module, you no longer need to use **PowerShellGet** and **PackageManagement**. However, it can
be installed side-by-side with the existing **PowerShellGet** module. To install
**Microsoft.PowerShell.PSResourceGet** side-by-side with your existing **PowerShellGet** version,
open any PowerShell console and run:

```powershell
Install-Module Microsoft.PowerShell.PSResourceGet -Repository PSGallery
```

**Microsoft.PowerShell.PSResourceGet** is preinstalled with PowerShell 7.4 and later.

<!-- link references -->
[01]: /powershell/module/microsoft.powershell.core/about/about_profiles
