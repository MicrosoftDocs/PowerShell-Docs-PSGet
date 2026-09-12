---
description: >-
  Learn how to declare package repositories and PowerShell packages in a Microsoft DSC configuration
  document and apply, test, preview, and export it with the dsc config commands.
ms.date: 09/12/2026
ms.topic: how-to
title: Manage packages with a DSC configuration document
---
# Manage packages with a DSC configuration document

A DSC configuration document describes the desired state of a machine as a list of resource
instances. When you apply the document, the DSC engine invokes each resource in order, resolves
dependencies between instances, and reports the result for the document as a whole. This article
shows how to describe the repositories and packages a machine needs in a configuration document
and how to work with that document using the `dsc config` commands.

## Prerequisites

- Complete the steps in [Make the resources discoverable][01] so that `dsc resource list`
  returns both resources.
- A text editor for YAML files. Visual Studio Code with the YAML extension validates the document
  against the DSC schema while you type.

## Write the configuration document

The following document registers a private repository, installs packages from it, and installs
tooling from the PowerShell Gallery. Save it as `packages.dsc.yaml`.

```yaml
# packages.dsc.yaml
$schema: https://aka.ms/dsc/schemas/v3/bundled/config/document.json
resources:
  - name: Contoso feed
    type: Microsoft.PowerShell.PSResourceGet/Repository
    properties:
      name: ContosoModules
      uri: https://pkgs.contoso.com/nuget/v3/index.json
      trusted: true
      priority: 40
      repositoryType: V3

  - name: Contoso modules
    type: Microsoft.PowerShell.PSResourceGet/PSResourceList
    dependsOn:
      - "[resourceId('Microsoft.PowerShell.PSResourceGet/Repository', 'Contoso feed')]"
    properties:
      repositoryName: ContosoModules
      resources:
        - name: Contoso.Deployment
          version: '[3.2.0, 4.0.0)'
        - name: Contoso.Legacy
          _exist: false

  - name: Tooling from the PowerShell Gallery
    type: Microsoft.PowerShell.PSResourceGet/PSResourceList
    properties:
      repositoryName: PSGallery
      trustedRepository: true
      resources:
        - name: PSScriptAnalyzer
          version: '[1.24.0]'
        - name: Pester
          version: '[5.0.0, )'
          scope: AllUsers
        - name: Microsoft.WinGet.Client
          preRelease: true
```

The document shows several patterns:

- **Register before you install.** The `dependsOn` entry tells DSC to process the **Repository**
  instance before the **PSResourceList** instance that installs from it. Without the dependency,
  DSC processes instances in document order, which works here but isn't guaranteed if you
  reorder the document. For more information, see
  [Configuration document resource dependencies][02].
- **One list per repository.** Each **PSResourceList** instance manages the packages from a single
  repository. Use one instance for each repository you install from.
- **Trust at the repository or at the list.** The Contoso repository is registered as trusted, so
  its list doesn't need `trustedRepository`. The PowerShell Gallery keeps its default untrusted
  setting, so the list that installs from it sets `trustedRepository` to `true`.
- **Pin, range, or latest.** `PSScriptAnalyzer` is pinned to an exact version, `Pester` accepts
  any version from 5.0.0 onward, and `Microsoft.WinGet.Client` takes the latest
  version including prereleases. For more information, see the [version][03] property.
- **Remove what shouldn't be there.** `Contoso.Legacy` has `_exist: false`, so DSC uninstalls it
  if it's found.

> [!NOTE]
> The `Pester` entry installs to the `AllUsers` scope, so `dsc` must run in an elevated process to
> apply this document. Remove the `scope` line to keep the whole document per user.

## Test the document

Use `dsc config test` to compare the document with the machine without changing anything. The
output reports the state of every instance and whether the document as a whole is in the desired
state.

```powershell
dsc config test --file ./packages.dsc.yaml
```

```yaml
metadata:
  Microsoft.DSC:
    version: 3.1.0
    operation: test
    executionType: actual
    startDatetime: 2026-09-12T10:15:03.123456700+02:00
    endDatetime: 2026-09-12T10:15:09.987654300+02:00
    duration: PT6.864197600S
    securityContext: elevated
results:
- metadata:
    Microsoft.DSC:
      duration: PT1.2S
  name: Contoso feed
  type: Microsoft.PowerShell.PSResourceGet/Repository
  result:
    desiredState:
      name: ContosoModules
      uri: https://pkgs.contoso.com/nuget/v3/index.json
      trusted: true
      priority: 40
      repositoryType: V3
    actualState:
      name: ContosoModules
      uri: null
      trusted: false
      priority: 0
      repositoryType: Unknown
      _exist: false
    inDesiredState: false
    differingProperties:
    - uri
    - trusted
    - priority
    - repositoryType
- metadata:
    Microsoft.DSC:
      duration: PT2.4S
  name: Contoso modules
  type: Microsoft.PowerShell.PSResourceGet/PSResourceList
  result:
    desiredState:
      repositoryName: ContosoModules
      resources:
      - name: Contoso.Deployment
        version: '[3.2.0, 4.0.0)'
      - name: Contoso.Legacy
        _exist: false
    actualState:
      repositoryName: ContosoModules
      resources:
      - name: Contoso.Deployment
        version: '[3.2.0, 4.0.0)'
        scope: CurrentUser
        repositoryName: ContosoModules
        preRelease: false
        _exist: true
        _inDesiredState: false
      - name: Contoso.Legacy
        version: null
        scope: CurrentUser
        repositoryName: ContosoModules
        preRelease: false
        _exist: false
        _inDesiredState: false
      trustedRepository: false
      _inDesiredState: false
    inDesiredState: false
    differingProperties:
    - resources
- metadata:
    Microsoft.DSC:
      duration: PT3.1S
  name: Tooling from the PowerShell Gallery
  type: Microsoft.PowerShell.PSResourceGet/PSResourceList
  result:
    desiredState:
      repositoryName: PSGallery
      trustedRepository: true
      resources:
      - name: PSScriptAnalyzer
        version: '[1.24.0]'
      - name: Pester
        version: '[5.0.0, )'
        scope: AllUsers
      - name: Microsoft.WinGet.Client
        preRelease: true
    actualState:
      repositoryName: PSGallery
      resources:
      - name: PSScriptAnalyzer
        version: '[1.24.0]'
        scope: CurrentUser
        repositoryName: PSGallery
        preRelease: false
        _exist: true
        _inDesiredState: false
      - name: Pester
        version: '[5.0.0, )'
        scope: AllUsers
        repositoryName: PSGallery
        preRelease: false
        _exist: true
        _inDesiredState: false
      - name: Microsoft.WinGet.Client
        version: null
        scope: CurrentUser
        repositoryName: PSGallery
        preRelease: true
        _exist: true
        _inDesiredState: false
      trustedRepository: false
      _inDesiredState: false
    inDesiredState: false
    differingProperties:
    - resources
messages: []
hadErrors: false
```

> [!NOTE]
> When a **PSResourceList** instance isn't in the desired state, the `actualState` the resource
> returns for the **Test** operation echoes the entries you defined, with default values filled in,
> rather than the installed packages. The per-entry `_inDesiredState` value is always `false`. Use
> `dsc config get` to see which versions are installed. For more information, see the
> [_inDesiredState][11] property.

The output is long. Convert the JSON output to objects to list only the instances that aren't in
the desired state:

```powershell
$result = dsc config test --file ./packages.dsc.yaml -o json | ConvertFrom-Json

$result.results |
    Where-Object { -not $_.result.inDesiredState } |
    Select-Object -Property name, type
```

```Output
name                                type
----                                ----
Contoso feed         Microsoft.PowerShell.PSResourceGet/Repository
Contoso modules                     Microsoft.PowerShell.PSResourceGet/PSResourceList
Tooling from the PowerShell Gallery Microsoft.PowerShell.PSResourceGet/PSResourceList
```

## Preview the changes

Before you apply a document, use the `--what-if` option to see what would change. The
**PSResourceList** resource implements what-if support, so the projected state includes a
`_metadata.whatIf` message for every package it would install or uninstall. Packages that are
already in the desired state are returned without metadata. Nothing is installed or removed.

```powershell
dsc config set --file ./packages.dsc.yaml --what-if
```

```yaml
metadata:
  Microsoft.DSC:
    version: 3.1.0
    operation: set
    executionType: whatIf
    startDatetime: 2026-09-12T10:16:40.123456700+02:00
    endDatetime: 2026-09-12T10:16:47.987654300+02:00
    duration: PT7.864197600S
    securityContext: elevated
results:
- metadata:
    Microsoft.DSC:
      duration: PT1.2S
  name: Contoso feed
  type: Microsoft.PowerShell.PSResourceGet/Repository
  result:
    beforeState:
      name: ContosoModules
      uri: null
      trusted: false
      priority: 0
      repositoryType: Unknown
      _exist: false
    afterState:
      name: ContosoModules
      uri: https://pkgs.contoso.com/nuget/v3/index.json
      trusted: true
      priority: 40
      repositoryType: V3
    changedProperties:
    - uri
    - trusted
    - priority
    - repositoryType
- metadata:
    Microsoft.DSC:
      duration: PT2.4S
  name: Contoso modules
  type: Microsoft.PowerShell.PSResourceGet/PSResourceList
  result:
    beforeState:
      repositoryName: ContosoModules
      resources:
      - name: Contoso.Deployment
        version: null
        scope: CurrentUser
        repositoryName: null
        preRelease: false
        _exist: false
      - name: Contoso.Legacy
        version: null
        scope: CurrentUser
        repositoryName: null
        preRelease: false
        _exist: false
    afterState:
      repositoryName: ContosoModules
      resources:
      - name: Contoso.Deployment
        version: '[3.2.0, 4.0.0)'
        scope: CurrentUser
        repositoryName: ContosoModules
        preRelease: false
        _exist: true
        _metadata:
          whatIf:
          - Would install resource 'Contoso.Deployment' version '[3.2.0, 4.0.0)'
      - name: Contoso.Legacy
        version: null
        scope: CurrentUser
        repositoryName: null
        preRelease: false
        _exist: false
    changedProperties:
    - resources
- metadata:
    Microsoft.DSC:
      duration: PT3.1S
  name: Tooling from the PowerShell Gallery
  type: Microsoft.PowerShell.PSResourceGet/PSResourceList
  result:
    beforeState:
      repositoryName: PSGallery
      resources:
      - name: PSScriptAnalyzer
        version: 1.24.0
        scope: CurrentUser
        repositoryName: PSGallery
        preRelease: false
        _exist: true
      - name: Pester
        version: 5.7.1
        scope: AllUsers
        repositoryName: PSGallery
        preRelease: false
        _exist: true
      - name: Microsoft.WinGet.Client
        version: null
        scope: CurrentUser
        repositoryName: null
        preRelease: false
        _exist: false
    afterState:
      repositoryName: PSGallery
      resources:
      - name: PSScriptAnalyzer
        version: 1.24.0
        scope: CurrentUser
        repositoryName: PSGallery
        preRelease: false
        _exist: true
      - name: Pester
        version: 5.7.1
        scope: AllUsers
        repositoryName: PSGallery
        preRelease: false
        _exist: true
      - name: Microsoft.WinGet.Client
        version: latest
        scope: CurrentUser
        repositoryName: PSGallery
        preRelease: true
        _exist: true
        _metadata:
          whatIf:
          - Would install resource 'Microsoft.WinGet.Client' version 'latest'
    changedProperties:
    - resources
messages: []
hadErrors: false
```

The `Contoso.Legacy` entry isn't installed, so the resource returns it unchanged and without
metadata. If it were installed, the projected entry would have `_exist: false` and a
`Would uninstall resource 'Contoso.Legacy'` message.

The **Repository** resource doesn't implement what-if support. For it, DSC synthesizes the result
from the **Get** operation and the desired state, which is why the entry has no `_metadata`.

> [!NOTE]
> What-if support for the **PSResourceList** resource requires a version of DSC that recognizes the
> what-if definition in the resource manifest. On older versions of DSC, the engine synthesizes the
> result from the **Test** operation instead. The synthesized result still shows which instances
> would change, but doesn't include the `_metadata.whatIf` messages.

## Apply the document

Use `dsc config set` to enforce the document. DSC invokes the **Set** operation for every instance
that isn't in the desired state and reports the state before and after each change.

```powershell
dsc config set --file ./packages.dsc.yaml
```

The command is idempotent. Run it again and the resources report no changes, because every
instance is already in the desired state. Run `dsc config test` at any time to detect drift, for
example after someone installs or removes a package interactively.

If a **PSResourceList** instance fails to install a package, DSC stops processing the document and
reports the error. The `hadErrors` property in the output is `true`, and the `messages` property
contains the error the resource wrote. Fix the cause and apply the document again. Packages that
were installed before the failure stay installed and aren't reinstalled. For more information, see
the [PSResourceList exit codes][04].

## Capture the state of a machine

Use `dsc config export` to build a configuration document from the machine you're on. The document
lists every registered repository and every installed package, grouped by repository. Use it as a
starting point for a document you can apply to other machines.

```yaml
# export.dsc.yaml
$schema: https://aka.ms/dsc/schemas/v3/bundled/config/document.json
resources:
  - name: Repositories
    type: Microsoft.PowerShell.PSResourceGet/Repository
  - name: Packages
    type: Microsoft.PowerShell.PSResourceGet/PSResourceList
```

```powershell
dsc config export --file ./export.dsc.yaml | Out-File -FilePath ./machine.dsc.yaml
```

Before you apply the exported document elsewhere, review it:

- Remove packages that shouldn't be managed, such as modules that ship with PowerShell.
- Decide how strict each `version` should be. The export records bare versions, which the
  resource treats as minimum versions when it tests the machine and as exact versions when it
  installs. Pin with `[<version>]` or widen to a range as needed.
- Add `trustedRepository: true` to lists that install from untrusted repositories, or set
  `trusted: true` on the corresponding **Repository** instance.

## Use parameters for values that change per machine

Configuration documents support parameters. Use them to keep repository URIs or package versions
out of the document body so that the same document works across environments.

```yaml
# packages-parameterized.dsc.yaml
$schema: https://aka.ms/dsc/schemas/v3/bundled/config/document.json
parameters:
  feedUri:
    type: string
  analyzerVersion:
    type: string
    defaultValue: '[1.24.0]'
resources:
  - name: Contoso feed
    type: Microsoft.PowerShell.PSResourceGet/Repository
    properties:
      name: ContosoModules
      uri: "[parameters('feedUri')]"
      trusted: true
  - name: Tooling from the PowerShell Gallery
    type: Microsoft.PowerShell.PSResourceGet/PSResourceList
    properties:
      repositoryName: PSGallery
      trustedRepository: true
      resources:
        - name: PSScriptAnalyzer
          version: "[parameters('analyzerVersion')]"
```

```powershell
$parameters = @{
    parameters = @{
        feedUri = 'https://pkgs.contoso.com/nuget/v3/index.json'
    }
} | ConvertTo-Json -Compress

dsc config --parameters $parameters set --file ./packages-parameterized.dsc.yaml
```

For more information, see [DSC configuration document parameters][05].

## See also

- [Invoke the PSResourceGet DSC resources directly][06]
- [Manage PowerShell packages with Microsoft DSC][07]
- [Microsoft.PowerShell.PSResourceGet/Repository][08]
- [Microsoft.PowerShell.PSResourceGet/PSResourceList][09]
- [dsc config command reference][10]

<!-- link references -->
[01]: ../overview.md#make-the-resources-discoverable
[02]: /powershell/dsc/reference/schemas/config/resource?view=dsc-3.0&preserve-view=true#dependson
[03]: ../reference/psresourcelist.md#version
[04]: ../reference/psresourcelist.md#exit-codes
[05]: /powershell/dsc/reference/schemas/config/parameter?view=dsc-3.0&preserve-view=true
[06]: invoke-resources.md
[07]: ../overview.md
[08]: ../reference/repository.md
[09]: ../reference/psresourcelist.md
[10]: /powershell/dsc/reference/cli/config/index?view=dsc-3.0&preserve-view=true
[11]: ../reference/psresourcelist.md#_indesiredstate
