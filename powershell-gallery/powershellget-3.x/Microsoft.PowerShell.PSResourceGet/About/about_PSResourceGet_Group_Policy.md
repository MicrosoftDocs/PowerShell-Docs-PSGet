---
description: Describes the Group Policy settings for PowerShell
Locale: en-US
ms.custom: 1.1.0-rc1
ms.date: 10/22/2024
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_psresourceget_group_policy?view=powershellget-3.x&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PSResourceGet_Group_Policy
---
# about_PSResourceGet_Group_Policy

## Short description

Describes the Group Policy settings for the PSResourceGet module.

## Long description

**Microsoft.PowerShell.PSResourceGet** includes a Group Policy to specify a
list of repositories that can be accessed by the module. All other repositories
are blocked.

Support for Group Policy was added in **Microsoft.PowerShell.PSResourceGet**
v1.1.0-rc1.

The PowerShell Group Policy settings are in the following Group Policy path:

```
User Configuration\
  Administrative Templates\
    PSResourceGet Repository Policies
```

There is no computer configuration path for these settings.

**Microsoft.PowerShell.PSResourceGet** includes Group Policy templates and an
installation script in module install location.

Group Policy tools use administrative template files (`.admx`, `.adml`) to
populate policy settings in the user interface. This allows administrators to
manage registry-based policy settings. To install the administrative template,
run the `InstallPSResourceGetPolicyDefinitions.ps1` script from an elevated
PowerShell session. The following example shows the location of the script and
template files.

```powershell
$modulePath = Split-Path (Get-Module -Name Microsoft.PowerShell.PSResourceGet).Path
Get-ChildItem -Path $modulePath\*.adm*, $modulePath\*.ps1
```

```Output
    Directory: C:\Program Files\PowerShell\Modules\Microsoft.PowerShell.PSResourceGet\1.1.0

Mode            LastWriteTime     Length Name
----            -------------     ------ ----
-a---     10/22/2024  3:48 PM       1364 PSResourceRepository.adml
-a---     10/22/2024  3:48 PM       1839 PSResourceRepository.admx
-a---     10/22/2024  3:48 PM       2869 InstallPSResourceGetPolicyDefinitions.ps1
```

After installing the templates, you can edit these settings in the Group Policy
Editor (`gpedit.msc`).

## Configure a list of allowed repositories

There is only one setting for the **PSResourceGet Repository Policies** Group
Policy.

1. Open the Group Policy Editor (`gpedit.msc`)
1. Locate the policy under **User configuration** -> **Administrative
   Templates** -> **Windows Components** -> **PSResourceGet Repository
   Policies**
1. Open the **PSResourceGet Repository Policy** setting
1. Select **Enabled**
1. Select **Show**
1. Add repository in the format:
   `Name=PSGallery;Uri=https://www.powershellgallery.com/api/v2`

   > [!NOTE] Don't use quote characters around the values. Separate the
   > key-value pairs with a semicolon (`;`) only.

You may add as may repositories as you like, each on their own line.

> [!CAUTION]
> If you enable the policy without defining any repositories then no
> repositories are allowed, effectively disabling
> **Microsoft.PowerShell.PSResourceGet** module.

## Determine the affect of Group Policy

Once you have applied the Group Policy setting, the PSResourceGet commands that
communicate with repositories check the policy before attempting to access the
repository. If the a specified repository isn't in the allow list, you receive
and error message similar to:

> Repository 'RepoName' is not allowed by Group Policy.

Adding repositories to the allow list doesn't register them for the user. You
must use add the repositories using the `Register-PSResourceRepository` cmdlet.
The `Register-PSResourceRepository` cmdlet doesn't access the repository at
registration time. You won't receive an error if the repository isn't in the
allow list until you try to access the repository.

You can use the following command to determine the repositories that are
allowed by Group Policy:

```powershell
Get-PSResourceRepository | Where-Object { $_.IsAllowedByPolicy }
```
