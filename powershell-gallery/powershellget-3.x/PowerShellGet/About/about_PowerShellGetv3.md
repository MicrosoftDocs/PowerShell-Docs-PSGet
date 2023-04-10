---
description: Describes how to use version 3.x of the PowerShellGet module.
Locale: en-US
ms.date: 04/04/2023
online version:
schema: 2.0.0
title: about PowerShellGet v3
---
# about_PowerShellGetv3

## Short description
Describes how to use version 3.x of the PowerShellGet module.

## Long description

**PowerShellGet** v3 is an updated version of the **PowerShellGet** module
completely written in C#.

This version of PowerShellGet focuses on a few key areas:

- Simplify the code base making it easier to enhance and fix bugs
- Remove the dependency on the **PackageManagement** module and use the
  **NuGet** library directly
- Address long-standing usability issues that would be breaking changes from v2
- Maintain compatibility for existing scripts written expecting v2 through a
  separate compatibility module
- Improve search and installation performance

## Design changes

Previous versions of **PowerShellGet** had separate commands to work with
modules and scripts. In **PowerShellGet** v3, all packages in the PowerShell
Gallery are defined as **PSResource** objects. This reduces the number of
cmdlets from 26 in version 2.x to 16 in version 3.x.

The following table shows the cmdlets that are available in **PowerShellGet**
v3 and their v2 equivalents.

|         PowerShellGet v3          |     PowerShellGet v2      |
| --------------------------------- | ------------------------- |
| `Find-PSResource`                 | `Find-Command`            |
| `Find-PSResource`                 | `Find-DscResource`        |
| `Find-PSResource`                 | `Find-Module`             |
| `Find-PSResource`                 | `Find-Script`             |
| n/a                               | `Find-RoleCapability`     |
| `Get-PSResource`                  | `Get-InstalledModule`     |
| `Get-PSResource`                  | `Get-InstalledScript`     |
| `Get-PSResourceRepository`        | `Get-PSRepository`        |
| `Get-PSScriptFileInfo`            | n/a                       |
| `Install-PSResource`              | `Install-Module`          |
| `Install-PSResource`              | `Install-Script`          |
| `New-PSScriptFileInfo`            | `New-ScriptFileInfo`      |
| `Publish-PSResource`              | `Publish-Module`          |
| `Publish-PSResource`              | `Publish-Script`          |
| `Register-PSResourceRepository`   | `Register-PSRepository`   |
| `Save-PSResource`                 | `Save-Module`             |
| `Save-PSResource`                 | `Save-Script`             |
| `Set-PSResourceRepository`        | `Set-PSRepository`        |
| `Test-PSScriptFileInfo`           | `Test-ScriptFileInfo`     |
| `Uninstall-PSResource`            | `Uninstall-Module`        |
| `Uninstall-PSResource`            | `Uninstall-Script`        |
| `Unregister-PSResourceRepository` | `Unregister-PSRepository` |
| `Update-ModuleManifest`           | `Update-ModuleManifest`   |
| `Update-PSResource`               | `Update-Module`           |
| `Update-PSResource`               | `Update-Script`           |
| `Update-PSScriptFileInfo`         | `Update-ScriptFileInfo`   |

## Searching by NuGet version ranges

Several **PowerShellGet** v3 cmdlets provide a **Version** parameter that
allows you to specify a range of versions to search for. The **Version**
parameter uses the NuGet versioning syntax. For more information about NuGet
version ranges, see [Package versioning][01].

PowerShellGet supports all but the _minimum inclusive version_ listed in the
NuGet version range documentation. Using `1.0.0.0` as the version doesn't yield
versions 1.0.0.0 and higher (minimum inclusive range). Instead, the value is
considered to be the required version. To search for a minimum inclusive range,
use `[1.0.0.0, ]` as the version range.

## Searching by required resources

The [`Install-PSResource`][04] cmdlet has **RequiredResource** and
**RequiredResourceFile** parameters that are used to find **PSResource**
objects matching specific criteria. You can specify the search criteria using a
hashtable or a JSON object. For the **RequiredResourceFile** parameter, the
hashtable is stored in a `.psd1` file and the JSON object is stored in a
`.json` file.

The hashtable can contain attributes for multiple modules. The following
example show the structure of the module specification:

```Syntax
@{
    <modulename> = @{
        version = '<version-spcification>'
        repository = '<reponame>'
        prerelease = '<boolean>'
    }
}
```

This example contains specifications for three modules. As you can, the module
attributes are optional.

```powershell
 @{
    TestModule = @{
        version = '[0.0.1,1.3.0]'
        repository = 'PSGallery'
    }

    TestModulePrerelease = @{
        version = '[0.0.0,0.0.5]'
        repository = 'PSGallery'
        prerelease = $true
    }

    TestModule99 = @{}
}
```

The next example shows the same specification in JSON format.

```json
{
  "TestModule": {
    "version": "[0.0.1,1.3.0)",
    "repository": "PSGallery"
  },
  "TestModulePrerelease": {
    "version": "[0.0.0,0.0.5]",
    "repository": "PSGallery",
    "prerelease": "true"
  },
  "TestModule99": {}
}
```

## See also

- [Install-PSResource][04]
- [Find-PSResource][02]
- [Get-PSResource][03]
- [Install-PSResource][04]
- [Save-PSResource][05]
- [Uninstall-PSResource][06]
- [Update-PSResource][07]

<!-- link references -->
[01]: /nuget/concepts/package-versioning#version-ranges
[02]: xref:PowerShellGet.Find-PSResource
[03]: xref:PowerShellGet.Get-PSResource
[04]: xref:PowerShellGet.Install-PSResource
[05]: xref:PowerShellGet.Save-PSResource
[06]: xref:PowerShellGet.Uninstall-PSResource
[07]: xref:PowerShellGet.Update-PSResource
