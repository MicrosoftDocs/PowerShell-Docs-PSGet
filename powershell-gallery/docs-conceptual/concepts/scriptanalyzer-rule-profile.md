---
description: Explains how the PowerShell ScriptAnalyzer is integrated with the PowerShell Gallery.
ms.date: 12/12/2024
title: ScriptAnalyzer rule profile for Gallery
---
# ScriptAnalyzer rule profile for Gallery

To ensure the quality of packages published to PowerShell Gallery, we run [PSScriptAnalyzer][01]
rules to determine if there are any violations in the scripts submitted.

You can find the list of rules we are running on ScriptAnalyzer [GitHub page][02]. If you have any
concerns regarding the rules we are running, please contact PowerShell Gallery Administrators, or
open an issue for ScriptAnalyzer.

ScriptAnalyzer results will be displayed on each individual package page in Gallery in the coming
release. We encourage package owners to check their packages to make sure there are no severe errors
in published packages.

<!-- link references -->
[01]: https://github.com/PowerShell/PSScriptAnalyzer
[02]: https://github.com/PowerShell/PSScriptAnalyzer/blob/main/Engine/Settings/PSGallery.psd1
