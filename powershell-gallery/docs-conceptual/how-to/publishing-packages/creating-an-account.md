---
description: This article describe the user account requirements for the PowerShell Gallery
ms.date: 08/04/2023
title: Creating a PowerShell Gallery account
---

# Creating a PowerShell Gallery account

You must create a PowerShell Gallery account before publishing anything to the PowerShell Gallery.
PowerShell Gallery accounts must be linked to an email-enabled login account. This account can be an
Microsoft Entra account or a Microsoft ID, like an email account from `outlook.com` or
`hotmail.com`.

To create a PowerShell Gallery account, go to [https://PowerShellGallery.com][01] and click on
**Sign in** as shown in the following image.

![Register new account][03]

To use an Azure Active Directory account, select **Work or School Account**, and sign in with your
account. To use a Microsoft ID, choose **Personal Account** and sign in.

Next, you are prompted to create a username for the PowerShell Gallery. Review the Terms of Use and
Privacy Policy, enter a username, and then click **Register**.

> [!NOTE]
> The account name can't be changed once it's created. For more information, see
> [Managing Package Owners][02].

## Recommended practices for PowerShell Gallery accounts

It's important to actively monitor the email account used with your PowerShell Gallery account. All
communication with owners of PowerShell Gallery packages is through this email address. If the
PowerShell Gallery Operations team is unable to contact a package owner, we may be required to
delete a package.

Organizations that publish to the PowerShell Gallery often create a unique external account for that
purpose. We recommend you use email forwarding to forward notifications to an address within your
organization.

When multiple owners are associated with a package, all PowerShell Gallery notifications are sent to
all owners. For more information, see [Managing Package Owners][02].

<!-- link references -->
[01]: https://PowerShellGallery.com
[02]: managing-package-owners.md
[03]: media/creating-an-account/CreateAccount-Register.png
