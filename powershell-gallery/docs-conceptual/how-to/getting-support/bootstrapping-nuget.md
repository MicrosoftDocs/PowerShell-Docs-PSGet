---
description: >-
  This article explains how to manually install the required NuGet components for Windows PowerShell 5.1.
ms.date: 01/26/2026
title: Bootstrapping NuGet
---
# Bootstrap the NuGet components for Windows PowerShell 5.1

On a new deployment of Windows, Windows PowerShell 5.1 doesn't include to the necessary NuGet
components to interact with the PowerShell Gallery. PowerShellGet includes logic to update these
components as long as you can connect to the PowerShell Gallery. If the machine isn't connected to
the internet, you must copy required files from a trusted source to the disconnected machine.

The updated NuGet provider is required by the commands to work with the PowerShell Gallery. The
`Publish-*` commands use `nuget.exe` to publish resources.

## Install the latest version of PowerShellGet on an internet-connected machine

To install the latest version of PowerShellGet, run the following command:

```powershell
Install-Module -Name PowerShellGet -Repository PSGallery
```

Answer the prompt with <kbd>Y</kbd> to install the NuGet provider.

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

> [!NOTE]
> When you install PowerShellGet, it automatically installs the latest version of
> PackageManagement.

## Copy the required files to the isolated computer

After installing the updates on the internet-connected machine, manually copy the components to the
isolated node through a trusted offline process.

1. Copy the PowerShellGet and PackageManagement modules to the offline machine. Use the following
   command to locate the modules on the source machine:

   ```powershell
   Get-Module PowerShellGet, PackageManagement -ListAvailable |
       Sort-Object Version -Descending |
       Select-Object Path -First 2
   ```

   The result should look similar to the following output:

   ```Output
   Path
   ----
   C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\2.2.5\PowerShellGet.psd1
   C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.4.8.1\PackageManagement.psd1
   ```

   Copy the entire module folders to the same location on the isolated machine. For example, if the
   modules are located in `PowerShellGet\2.2.5` and `PackageManagement\1.4.8.1` folders to the
   same folder names under `$env:PROGRAMFILES\WindowsPowerShell\Modules` on the target machine.

   > [!NOTE]
   > You need administrative privileges to copy files to `$env:PROGRAMFILES`.

1. Copy `nuget.exe` to the isolated machine. PowerShellGet installs `nuget.exe` in the following
   location: `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\PowerShellGet\nuget.exe`

   If the file isn't present at that location it's either installed somewhere else or PowerShellGet
   found the .NET CLI (`dotnet.exe`). You can download the latest version of `nuget.exe` from
   [https://aka.ms/psget-nugetexe][01].

    Copy `nuget.exe` to `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\PowerShellGet\nuget.exe` on
    the target computer.

## Unblock the copied files

When you copy files from another computer, Windows may block the files. To unblock the copied files,
run the following commands on the target machine:

```powershell
$getChildItemSplat = @{
    Path = @(
      "$env:PROGRAMFILES\WindowsPowerShell\Modules\PowerShellGet"
      "$env:PROGRAMFILES\WindowsPowerShell\Modules\PackageManagement"
      "$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\PowerShellGet\nuget.exe"
    )
    Recurse = $true
}
Get-ChildItem @getChildItemSplat | Unblock-File
```

<!-- link references -->
[01]: https://aka.ms/psget-nugetexe
