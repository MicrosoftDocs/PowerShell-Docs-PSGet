---
description: This article explains how install PowerShellGet.
ms.date: 01/26/2026
ms.topic: install-set-up-deploy
title: Install a package manager for PowerShell
---
# Install a package manager for PowerShell

If you are running PowerShell 6.0 or later, you already have a newer version of **PowerShellGet**
and **PackageManagement** installed. You should ensure you are running the latest versions of those
modules.

If you are running PowerShell 7.4 or later, you also have **Microsoft.PowerShell.PSResourceGet**
installed. **Microsoft.PowerShell.PSResourceGet** is the new package management solution for
PowerShell. With this module, you no longer need to use **PowerShellGet** and **PackageManagement**.
It's installed side-by-side with the existing **PowerShellGet** and **PackageManagement** modules.

Windows PowerShell ships with version 1.0.0.1 of **PowerShellGet** and **PackageManagement**. If
you're running Windows PowerShell 5.1, you must upgrade to the latest version of PowerShellGet and
PackageManagement. All versions of PowerShellGet v1.x are no longer supported.

Use the following instructions to install or update to the latest versions of these modules.

## Step 1: Enable TLS 1.2

To access the PowerShell Gallery, you must use Transport Layer Security (TLS) 1.2 or higher. Use the
following command to enable TLS 1.2 in your PowerShell session.

```powershell
[Net.ServicePointManager]::SecurityProtocol =
    [Net.ServicePointManager]::SecurityProtocol -bor
    [Net.SecurityProtocolType]::Tls12
```

Add this command to your PowerShell profile script to ensure TLS 1.2 is configured for every
PowerShell session. For more information about profiles, see [about_Profiles][01].

## Step 2: Check the installed versions

To check the currently installed versions of the modules, run the following command:

```powershell
$Names = @('PowerShellGet', 'PackageManagement', 'Microsoft.PowerShell.PSResourceGet')
Get-Module -Name $Names -ListAvailable
```

In Windows PowerShell 5.1 on a newly installed Windows system, you should get the following output:

```Output
    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version  Name               ExportedCommands
---------- -------  ----               ----------------
Binary     1.0.0.1  PackageManagement  {Find-Package, Get-Package, ...
Script     1.0.0.1  PowerShellGet      {Install-Module, Find-Module, ...
```

If the version of **PowerShellGet** is newer than `1.0.0.1` then you can
[check for updates][02] and [install the latest release][03].

If you are still running version `1.0.0.1`, you must follow the steps to let **PowerShellGet** install
an updated NuGet provider and the `nuget.exe` command-line tool. Continue to the next step.

## Step 3: Check for updates

To check for the latest versions of the modules available from the PowerShell Gallery, run the
following command:

```powershell
$Names = @('PowerShellGet', 'PackageManagement', 'Microsoft.PowerShell.PSResourceGet')
Find-Module -Name $Names -Repository PSGallery
```

You should get a result similar to the following output:

```Output
Version   Name                                Repository   Description
-------   ----                                ----------   -----------
1.4.8.1   PackageManagement                   PSGallery    PackageManagement (a.k.a. OneGet) is a n…
2.2.5     PowerShellGet                       PSGallery    PowerShell module with commands for disc…
1.1.1     Microsoft.PowerShell.PSResourceGet  PSGallery    PowerShell module with commands for disc…
```

## Step 4: Update NuGet components (if required)

An updated NuGet provider is required by **PowerShellGet** commands to work with the PowerShell
Gallery. The `Publish-*` commands use `nuget.exe` or `dotnet.exe` to publish resources. If neither
tool is available, PowerShellGet installs `nuget.exe`. If you are still running version `1.0.0.1` of
**PowerShellGet**, `Find-Module` prompts you to install the NuGet provider. Enter <kbd>Y</kbd> to
install the provider.

```Output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet
-based repositories. The NuGet provider must be available in 'C:\Program Files\PackageMan
agement\ProviderAssemblies' or 'C:\Users\user1\AppData\Local\PackageManagement\ProviderAs
semblies'. You can also install the NuGet provider by running 'Install-PackageProvider -N
ame NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and imp
ort the NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.
```

When you answer <kbd>Y</kbd>, PowerShellGet installs the NuGet provider and the `nuget.exe`
command-line tool (if necessary).

## Step 5: Install the latest release

To install the latest versions of these modules run the following:

```powershell
Install-Module PowerShellGet -Repository PSGallery -Force -AllowClobber
Install-Module Microsoft.PowerShell.PSResourceGet -Repository PSGallery
```

> [!NOTE]
> When you install **PowerShellGet**, it automatically installs the latest version of
> **PackageManagement**.

<!-- link references -->
[01]: /powershell/module/microsoft.powershell.core/about/about_profiles
[02]: #step-3-check-for-updates
[03]: #step-5-install-the-latest-release
