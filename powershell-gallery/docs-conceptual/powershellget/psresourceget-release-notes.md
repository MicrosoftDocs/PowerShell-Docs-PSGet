---
description: This article contains release notes for the PSResourceGet module.
ms.date: 03/21/2025
ms.topic: release-notes
title: What's new in PSResourceGet?
---
# What's new in PSResourceGet?

This is a summary of changes to the **Microsoft.PowerShell.PSResourceGet** module. For a more
complete list of changes, see the [CHANGELOG][01] in the GitHub repository.

- Current stable release: **Microsoft.PowerShell.PSResourceGet** v1.1.0

## Release history

- v1.1.0 - Current release - released to the PowerShell Gallery
- v1.1.0 - shipped in PowerShell 7.5.0 and PowerShell 7.6.0-preview.1
- v1.1.0-rc3 - Preview release - released to the PowerShell Gallery only
- v1.1.0-rc2 - Preview release - shipped in PowerShell 7.5.0-rc1
- v1.1.0-rc1 - Preview release - shipped in PowerShell 7.5.0-preview.5
- v1.1.0-preview2 - released to the PowerShell Gallery only
- v1.1.0-preview.1 - Preview release - shipped in PowerShell 7.5.0-preview.4
- v1.0.6 - released to the PowerShell Gallery only
- v1.0.5 - shipped in PowerShell 7.5.0-preview.3
- v1.0.4.1 - shipped in PowerShell 7.4.2
- v1.0.4 - released to the PowerShell Gallery only
- v1.0.3 - released to the PowerShell Gallery only
- v1.0.2 - shipped in PowerShell 7.5.0-preview.2
- v1.0.1 - shipped in PowerShell 7.4.0 GA release and PowerShell 7.5.0-preview.1
- v1.0.0 - shipped in PowerShell 7.4.0-preview.5

## Release notes

### v1.1.1 - 2025-03-06

- Retrieve all metadata properties when finding a **PSResource** from a **ContainerRegistry**
  repository
- Use authentication challenge for unauthenticated **ContainerRegistry** repository
- Fix `Install-PSResource` to allow varying digit version against **ContainerRegistry** repository
- Update **ContainerRegistry** dependency parsing logic to account for **AzPreview** package
- Add wildcard support for MAR repository for `FindAll()` and `FindByName()`
- Fix nuspec dependency version range calculation for **RequiredModules**

### v1.1.0 - 2025-01-09

- Fixed publishing .nupkg file to **ContainerRegistry** repository
- Fixed PMPs like Artifactory needing modified filter query parameter to proxy upstream
- Fixed **ContainerRegistry** repository to parse out dependencies from metadata
- Fixed `Install-PSResource` Null pointer when package is present only in upstream feed in ADO
- Fixed local repository casing issue on Linux
- Fixed case sensitive License.txt when **RequireLicense** is specified
- Fixed `-Quiet` parameter for `Save-PSResource`

### v1.1.0-rc3 - 2024-11-15

- Included missing commits

### v1.1.0-rc2 - 2024-10-30

- Implemented full Microsoft Artifact Registry integration
- Update to use OCI v2 APIs for Container Registry
- Fixes finding and installing from local repositories on Linux
- Fixes finding package names with 4 part version from local repositories

### v1.1.0-rc1 - 2024-10-24

- Added Group Policy support
- Fix package name matching in local repositories
- `Compress-PSResource -PassThru` now return **FileInfo** instead of string
- Add `AcceptLicense` to `Save-PSResource`
- Other minor bug fixes and improvements

### v1.1.0-preview2 - 2024-09-13

- New cmdlet `Compress-PSResource` to create a `.nupkg` package without publishing it to a
  repository system.
- Added `-Nupkg` parameter to `Publish-PSResource` to publish a `.nupkg` file to a repository.
- Added `-ModulePrefix` parameter for `Publish-PSResource`, which adds a prefix to a module name for
  container registry repositories. This is only used for publishing and is not part of metadata.
- Improved error messages for Authenticode failures.
- Construct Prerelease string for repositories that don't return the prerelease information.
- Added retry logic when deleting files.

### v1.1.0-preview1 - 2024-04-01

- Added support for Azure Container Registries as a repository type
- Allowed PSResourceGet to run Constrained Languange Mode
- Fixed request URL when installing resources from ADO

### v1.0.6 - 2024-10-10

- Bump System.Text.Json to 8.0.5

### v1.0.5 - 2024-05-13

- Added 10 minute timeout to HTTPClient
- Refactor `V2ServerAPICalls` and NuGetServerAPICalls to use object-oriented query/filter builder
- Removed unnecessary `and` for version globbing in `V2ServerAPICalls`
- Fixed requiring `tags` in server response
- Fixed save script without `-IncludeXml`
- Fixed request URL when installing from ADO
- Improved exception handling
- Allowed PSResourceGet to run Constrained Languange Mode

### v1.0.4.1 - 2024-04-05

- PSResourceGet packaging update

### v1.0.4 - 2024-04-05

- Dependency package updates

### v1.0.3 - 2024-03-13

- Fixed null package version in `Install-PSResource`

### v1.0.2 - 2024-02-06

- Fixed `Update-PSResource` not updating from correct repository
- Fixed `InstalledScriptInfos` directory is now if it doesn't exist
- Fixed `Update-ModuleManifest` throwing null pointer exception
- Fixed `name` property in `PSResourceInfo` when using `Find-PSResource` with JFrog Artifactory
- Fixed configuration of requests to JFrog Artifactory v2 endpoints
- Fixed determining JFrog Artifactory repositories (#1532 Thanks @sean-r-williams!)
- Fixed for v2 server repositories incorrectly adding script endpoint (1526)
- Fixed typos in message prompts in `Install-PSResource`
- Only add `NormalizedVersion` property to `AdditionalMetadata` only when it exists
- Fix to verify whether `Uri` is a UNC path and set respective `ApiVersion`

### v1.0.1 - 2023-11-07

- Unix local user installation paths now compatible with .NET 7 and .NET 8
- Fixed `Import-PSGetRepository` in Windows PowerShell
- Fixed `Test-PSScriptFileInfo` to be less sensitive to whitespace
- Overwrite rels/rels directory on net472 when extracting nupkg to directory
- Added pipeline by property name support for **Name** and **Repository** parameters for
  `Find-PSResource`

### v1.0.0 - 2023-10-09

- Add `ApiVersion` parameter for `Register-PSResourceRepository`
- Automatically set the ApiVersion to v2 for repositories imported from PowerShellGet
- Fixed ADO v2 feed installation failures
- Fixed Artifactory v2 and v3 endpoint failures
- Fixed `-RequiredResource` silent failures
- Fixed v2 repository returning extra packages for `-Tag` based search with `-Prerelease`

<!-- link references -->
[01]: https://github.com/PowerShell/PSResourceGet/tree/master/CHANGELOG
