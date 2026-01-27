---
ms.date: 01/27/2026
---
# Package manager & PSGallery documentation

Welcome to the PowerShell-Docs-PSGet repository, the home of the official documentation for
PSResourceGet, PowerShellGet, and the PowerShell Gallery.

## Microsoft Open Source Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct][coc].

## PowerShell Updatable Help (CabGen) CI Build Status

[![Build Status][cabgen-status]][cabgen-log]

## Repository Structure

The following list describes the main folders in this repository.

- `.devcontainer` - contains devcontainer settings used by GitHub Codespaces and Visual Studio Code
  (VS Code) Remote - Containers extension
- `.github` - contains configuration settings used by GitHub for this repository
- `.vscode` - contains configuration settings and recommended extensions for VS Code
- `powershell-gallery` - contains the documentation published to
  [learn.microsoft.com/powershell/gallery][04]. This includes both reference and conceptual content.
  - `breadcrumb` - contains the TOC used for breadcrumb navigation
  - `docs-conceptual` - contains the conceptual articles that are published to the Docs site. In
    general, the folder structure mirrors the Table of Contents (TOC).
    - `media` - contains image files used in documentation. There are media folders throughout the
      `docs-conceptual` content. See the Contributor Guide for information on using images in
      documentation.
  - `includes` - contains markdown include files
  - `mapping` - contains the version mapping configuration used by the build system
  - `powershellget-2.x` - contains the cmdlet reference and about topics for PowerShellGet 2.x
  - `powershellget-3.x` - contains the cmdlet reference and about topics for PSResourceGet

> [!NOTE]
> The reference content (in the numbered folders) is used to create the webpages on the Docs site as
> well as the updateable help used by PowerShell. The articles in the `docs-conceptual` folder are
> only published to the Docs website.

## Contributing

We welcome public contributions into this repository via pull requests into the _main_ branch.
Please note that before we can accept your pull request you must sign our
[Contribution License Agreement][03]. This is a one-time requirement.

For more information on contributing, read our [contributor's guide][01]. The contributor's guide
contains detailed information about how to contribute documentation, suggested tools, and style and
formatting requirements. Please use the Issue and Pull Request templates to help keep documentation
consistent across versions.

## Licenses

There are two license files for this project. The MIT License applies to the code contained in this
repo. The Creative Commons license applies to the documentation.

<!-- updated link references -->
[01]: https://aka.ms/PSDocsContributor
[03]: https://cla.microsoft.com/
[04]: https://learn.microsoft.com/powershell/gallery/
[coc]: CODE_OF_CONDUCT.md
[cabgen-status]: https://apidrop.visualstudio.com/Content%20CI/_apis/build/status/PROD/CabGen(PowerShell_Updatable_Help)/GitHub_MicrosoftDocs_PowerShell-Docs-PSGet/8f69f151-baa4-9541-9991-266b76474533_cabgen_Publish-Updatable-Help?repoName=MicrosoftDocs%2FPowerShell-Docs-PSGet&branchName=live
[cabgen-log]: https://apidrop.visualstudio.com/Content%20CI/_build/latest?definitionId=5501&repoName=MicrosoftDocs%2FPowerShell-Docs-PSGet&branchName=live
