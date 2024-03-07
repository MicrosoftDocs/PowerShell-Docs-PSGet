---
description: This article explains how to update PowerShellGet for Windows PowerShell.
ms.date: 06/09/2023
ms.topic: install-set-up-deploy
title: Update PowerShellGet for Windows PowerShell 5.1
---
# Update PowerShellGet for Windows PowerShell 5.1

Windows PowerShell 5.1 comes with version 1.0.0.1 of the **PowerShellGet** and **PackageManagement**
preinstalled. This version of PowerShellGet has a limited features and must be updated to work
with the PowerShell Gallery. To be supported, you must update to the latest version.

### Prerequisites

- **PowerShellGet** requires .NET Framework 4.5 or above. For more information, see
  [Install the .NET Framework for developers][01].

- To access the PowerShell Gallery, you must use Transport Layer Security (TLS) 1.2 or higher. Use
  the following command to enable TLS 1.2 in your PowerShell session.

  ```powershell
  [Net.ServicePointManager]::SecurityProtocol =
      [Net.ServicePointManager]::SecurityProtocol -bor
      [Net.SecurityProtocolType]::Tls12
  ```

  Add this command to your PowerShell profile script to ensure TLS 1.2 is configured for every
  PowerShell session. For more information about profiles, see [about_Profiles][02].

### Installing the latest version of PowerShellGet

The PowerShellGet module includes cmdlets to install and update modules:

- `Install-Module` installs the latest (non-prerelease) version of a module.
- `Update-Module` installs the latest (non-prerelease) version of a module if it's newer than the
  currently installed module. However, this cmdlet only works if the previous version was installed
  using `Install-Module`.

To update the preinstalled module you must use `Install-Module`. After you have installed the new
version from the PowerShell Gallery, you can use `Update-Module` to install newer releases.

Windows PowerShell 5.1 comes with **PowerShellGet** version 1.0.0.1, which doesn't include the NuGet
provider. The provider is required by **PowerShellGet** when working with the PowerShell Gallery.

> [!NOTE]
> The following commands must be run from an elevated PowerShell session. Right-click the PowerShell
> icon and choose **Run as administrator** to start an elevated session.

There are two ways to install the NuGet provider:

- Use `Install-PackageProvider` to install NuGet before installing other modules

  Run the following command to install the NuGet provider.

  ```powershell
  Install-PackageProvider -Name NuGet -Force
  ```

  After you have installed the provider you should be able to use any of the **PowerShellGet**
  cmdlets with the PowerShell Gallery.

- Let `Install-Module` prompt you to install the NuGet provider

  The following command attempts to install the updated PowerShellGet module without the NuGet
  provider.

  ```powershell
  Install-Module PowerShellGet -AllowClobber -Force
  ```

  `Install-Module` prompts you to install the NuGet provider. Type <kbd>Y</kbd> to install the
  provider.

  ```Output
  NuGet provider is required to continue
  PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based
  repositories. The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies'
  or 'C:\Users\user1\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the
  NuGet provider by running 'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'.
  Do you want PowerShellGet to install and import the NuGet provider now?
  [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
  VERBOSE: Installing NuGet provider.
  ```

### After installing PowerShellGet

After you have installed the new version of **PowerShellGet**, you should open a new PowerShell
session. PowerShell automatically loads the newest version of the module when you use a
**PowerShellGet** cmdlet.

We also recommend that you register the PowerShell Gallery as a trusted repository. Use the
following command:

```powershell
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
```

For more information, see [Set-PSRepository][03].

<!-- link references -->
[01]: /dotnet/framework/install/guide-for-developers
[02]: /powershell/module/microsoft.powershell.core/about/about_profiles
[03]: xref:PowerShellGet.Set-PSRepository
