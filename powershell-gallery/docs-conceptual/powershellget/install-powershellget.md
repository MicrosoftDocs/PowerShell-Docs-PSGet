---
description: This article explains how install PowerShellGet.
ms.date: 04/07/2023
title: How to Install PowerShellGet
---
# How to Install PowerShellGet

## Prerequisites

Ensure that you have a version of **PowerShellGet** and **PackageManagement** newer than 1.0.0.1
installed. The latest stable versions are 2.2.5 for **PowerShellGet** and 1.4.8.1 for
**PackageManagement**.

If you're running Windows PowerShell 5.1 with **PowerShellGet** 1.0.0.1, see
[Update PowerShellGet for Windows PowerShell 5.1](update-powershell-51.md).

If you're running PowerShell 6.0 or later, you already have a newer version of **PowerShellGet** and
**PackageManagement** installed. You can upgrade to a newer version if necessary or install the
preview release. You should always install the latest stable release.

Use the following command to see what version is installed.

```powershell
Get-Module PowerShellGet, PackageManagement
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

## Install the Preview release

To install this preview release side-by-side with your existing PowerShellGet version, open any
PowerShell console and run:

```powershell
Install-Module PowerShellGet -AllowClobber -AllowPrerelease
```
