---
description: This article explains how to install the required NuGet components for Windows PowerShell 5.1.
ms.date: 01/27/2025
title: Bootstrapping NuGet
---
# Bootstrap the NuGet components for Windows PowerShell 5.1

On a new deployment of Windows, Windows PowerShell 5.1 doesn't include to the necessary NuGet
components to interact with the PowerShell Gallery. PowerShellGet includes logic to update these
components as long as you can connect to the PowerShell Gallery. If the machine isn't connected to
the internet, you must copy required files from a trusted source to the disconnected machine.

The required NuGet components are included in PowerShellGet v2+ and PackageManagement v1.1+. Newer
versions of these modules are available from the PowerShell Gallery and included in PowerShell 6 and
higher. These instructions are for Windows PowerShell 5.1.

> [!IMPORTANT]
> After bootstrapping the NuGet components, you must install latest versions of the PowerShellGet
> and PackageManagement modules to be supported.

## Bootstrap on an internet-connected machine

The following processes assume the machine is connected to the internet and can download files from
a public location.

### ERROR: NuGet provider is required to continue

You receive this error when the NuGet provider isn't available on the machine.

```powershell
Find-Module -Repository PSGallery -Verbose -Name Contoso
```

Answer the prompt with `Y` to install the NuGet provider.

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

Version    Name                                Type       Repository           Description
-------    ----                                ----       ----------           -----------
2.5        Contoso                             Module     PSGallery        Contoso module
```

### ERROR: NuGet.exe is required to continue

You receive this error when the NuGet provider is available, but the `nuget.exe` binary isn't.

```powershell
Publish-Module -Name Contoso -Repository PSGallery -Verbose
```

Answer the prompt with `Y` to install `nuget.exe`.

```Output
NuGet.exe is required to continue
PowerShellGet requires NuGet.exe to publish an item to the NuGet-based repositories. NuGe
t.exe must be available under one of the paths specified in PATH environment variable val
ue. Do you want PowerShellGet to install NuGet.exe now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://
www.powershellgallery.com/api/v2/'.
Please allow few minutes for 'Contoso' to show up in the search results.
```

### ERROR: NuGet.exe and NuGet provider are required to continue

You receive this error when both the NuGet provider and `nuget.exe` aren't installed.

```powershell
Publish-Module -Name Contoso -Repository PSGallery -Verbose
```

Answer the prompt with `Y` to install both the NuGet provider and `nuget.exe`.

```Output
NuGet.exe and NuGet provider are required to continue
PowerShellGet requires NuGet.exe and NuGet provider version '2.8.5.201' or newer to inter
act with the NuGet-based repositories. Do you want PowerShellGet to install both NuGet.ex
e and NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
VERBOSE: Installing NuGet provider.
VERBOSE: Installing NuGet.exe.
VERBOSE: Successfully published module 'Contoso' to the module publish location 'https://
www.powershellgallery.com/api/v2/'.
 Please allow few minutes for 'Contoso' to show up in the search results.
```

## Bootstrap on a machine not connected to the internet

The following processes assume the machine isn't connected to the internet. To install the necessary
components, follow the bootstrap process on an internet-connected machine then manually copy the
provider to the isolated node through an offline trusted process.

1. Copy the NuGet provider files to the offline machine.

   Copy the `C:\Program Files\PackageManagement\ProviderAssemblies\NuGet` folder from the connected
   machine to the same location on the offline machine.

1. Copy the PowerShellGet and PackageManagement modules to the offline machine.

   Copy the following module folders from the connected machine to same location on the offline
   machine.

   - `C:\Program Files\WindowsPowerShell\Modules\PowerShellGet`
   - `C:\Program Files\WindowsPowerShell\Modules\PackageManagement`
