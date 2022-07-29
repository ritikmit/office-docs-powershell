---
title: User-reported email settings for spam, phish, as malicious mail
f1.keywords:
  - NOCSH
ms.author: chrisda
author: chrisda
manager: dansimp
ms.date: 07/19/2022
audience: ITPro
ms.topic: how-to
ms.localizationpriority: medium
search.appverid:
  - MET150
ms.collection:
  - M365-security-compliance
  - m365initiative-defender-office365
ms.custom:
description: Admins can learn how identify a custom mailbox (also known as a user submissions mailbox) to collect spam and phishing messages that are reported by users. Other settings complete the reporting experience for users when they report messages.
ms.technology: mdo
ms.prod: m365-security
---

# User reported message settings

[!INCLUDE [MDO Trial banner](../includes/mdo-trial-banner.md)]

**Applies to**
- [Exchange Online Protection](exchange-online-protection-overview.md)
- [Microsoft Defender for Office 365 plan 1 and plan 2](defender-for-office-365.md)
- [Microsoft 365 Defender](../defender/microsoft-365-defender.md)

In Microsoft 365 organizations with Exchange Online mailboxes, you can direct mail to a custom mailbox (also known as the _user submissions mailbox_) when users report malicious or not malicious messages. When users report email messages using the supported reporting options, admins can configure the user reported message settings in their organization to send reported messages to the user submissions mailbox. You can configure the user submissions mailbox to intercept user-reported messages (send to the user submissions mailbox only) or receive copies of user-reported messages as users report messages to Microsoft. These settings were formerly known as the _User submissions policy_.

User reported message settings and the user submissions mailbox work with the following message reporting options:

- [The Report Message add-in](enable-the-report-message-add-in.md)
- [The Report Phishing add-in](enable-the-report-phish-add-in.md)
- [Third-party reporting tools](#third-party-reporting-tools-options)

Delivering user reported messages to a user submissions mailbox instead of directly to Microsoft allows admins to selectively and manually report messages to Microsoft on the **Submissions** page at <https://security.microsoft.com/reportsubmission>. For more information, see [Admin submission](admin-submission.md).

  > [!NOTE]
  > If reporting has been [disabled in Outlook on the web](report-junk-email-and-phishing-scams-in-outlook-on-the-web-eop.md#disable-or-enable-junk-email-reporting-in-outlook-on-the-web), enabling user-reported messages here will override that setting and allow users to report messages in Outlook on the web again.

## Configuration requirements for the user submissions mailbox

Before you get started, yu need to configure Exchange Online Protection and Defender for Office 365 so user reported messages are delivered to the user submissions mailbox without being filtered as described in the following steps:

- Identify the user submissions mailbox as a SecOps mailbox. For instructions, see [Use the Microsoft 365 Defender portal to configure SecOps mailboxes in the advanced delivery policy](configure-advanced-delivery.md#use-the-microsoft-365-defender-portal-to-configure-secops-mailboxes-in-the-advanced-delivery-policy).

- Create a custom anti-malware policy for the user submissions mailbox with the following settings:

  - Turn off Zero-hour auto purge (ZAP) for malware (**Protection settings** section \> **Enable zero-hour auto purge for malware** is not selected or `-ZapEnabled $false` in PowerShell).

  - Turn off common attachments filtering (**Protection settings** section \> **Enable the common attachments filter** is not selected or `EnableFileFilter $false` in PowerShell).
  
  For instructions, see [Create an anti-malware policy](configure-anti-malware-policies.md#use-the-microsoft-365-defender-portal-to-create-anti-malware-policies).

- Verify the user submissions mailbox is not included in the **Standard** or **Strict** preset security policies. For instructions, see [Preset security policies](preset-security-policies.md).

- **Defender for Office 365**: Configure the following additional settings:

  - Exclude the user submissions mailbox from the **Built-in protection** preset security policy. For instructions, see [Preset security policies](preset-security-policies.md).

  - Create a Safe Attachments policy for the user submissions mailbox where Safe Attachments scanning, including Dynamic Delivery, is turned off  (**Settings** \> **Safe Attachments unknown malware response** section \> **Off** or `-Enable $false` in PowerShell). For instructions, see [Set up Safe Attachments policies in Microsoft Defender for Office 365](set-up-safe-attachments-policies.md).

  - Create a Safe Links policy for the user submissions mailbox where Safe Links scanning in email is turned off (**URL & click protection settings** \> **On: Safe Links checks a list of known, malicious links when users click links in email** is not selected or `EnableSafeLinksForEmail $false` in PowerShell). For instructions, see [Set up Safe Links policies in Microsoft Defender for Office 365](set-up-safe-links-policies.md).

After you've verified that the mailbox meets these requirements, use the rest of the instructions in this article to identify the user submissions mailbox and other user reported message settings.

## What do you need to know before you begin?

- You open the Microsoft 365 Defender portal at <https://security.microsoft.com>. To go directly to the **User submissions** page, use <https://security.microsoft.com/userSubmissionsReportMessage>.

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

- To modify the configuration for User submissions, you need to be a member of one of the following role groups:

  - **Organization Management** or **Security Administrator** in the [Permissions in the Microsoft 365 Defender portal](permissions-microsoft-365-security-center.md).

- You need access to Exchange Online PowerShell. If the account that you're trying to use doesn't have access to Exchange Online PowerShell, you'll receive an error that looks like this when specifying the submissions mailbox:

  > Specify an email address in your domain

  For more information about enabling or disabling access to Exchange Online PowerShell, see the following topics:

  - [Enable or disable access to Exchange Online PowerShell](/powershell/exchange/disable-access-to-exchange-online-powershell)
  - [Client Access Rules in Exchange Online](/exchange/clients-and-mobile-in-exchange-online/client-access-rules/client-access-rules)

## Use the Microsoft 365 Defender portal to configure the user submissions mailbox for email

1. In the Microsoft 365 Defender portal at <https://security.microsoft.com>, go to **Policies & rules** \> **Threat policies** \> **User reported message settings** in the **Others** section. To go directly to the **User submissions** page, use <https://security.microsoft.com/userSubmissionsReportMessage>.

2. On the **User submissions** page, what you see is largely determined by the **Microsoft Outlook Report Message button** toggle:

   - **On** ![Toggle on.](../../media/scc-toggle-on.png): You use the Microsoft integrated reporting experience, which includes the Report Message add-in, the Report Phishing add-in or the built-in reporting in Outlook on the web.

     This setting also allows users to report false positive messages from the quarantine portal.

   - **Off** ![Toggle off.](../../media/scc-toggle-off.png): You use third-party message reporting tools instead of the Microsoft integrated reporting experience.

The related configuration options are described in the following sections.

### Microsoft integrated reporting experience options

When **Microsoft Outlook Report Message button** is **On** ![Toggle on.](../../media/scc-toggle-on.png), the following settings are available on the **User submissions** page:

- **Send the reported messages to** section: Select one of the following options:

  - **Microsoft**: The user submissions mailbox isn't used (all reported messages go to Microsoft for analysis).

  - **Microsoft and my organization's mailbox**: In the box that appears, enter the email address of an existing Exchange Online mailbox to use as the user submissions mailbox. Distribution groups are not allowed. User submissions go to Microsoft for analysis and to the user submissions mailbox for an admin or security operations team to analyze.

  - **My organization's mailbox**: In the box that appears, enter the email address of an existing Exchange Online mailbox. Distribution groups are not allowed. User submissions go only to the user submissions mailbox for an admin or the security operations team to analyze. Messages don't go to Microsoft for analysis unless an admin manually submits the messages.

  > [!IMPORTANT]
  > In U.S. Government organizations (GCC, GCC High, and DoD) organizations, the only available selection in the **Send the reported messages to** section is **My organization's mailbox**. The other two options are grayed out.
  >
  > If you used [Outlook on the web mailbox policies](/exchange/clients-and-mobile-in-exchange-online/outlook-on-the-web/configure-outlook-web-app-mailbox-policy-properties) to disable junk email reporting in Outlook on the web, but you select **Microsoft** or **Microsoft and my organization's mailbox**, users will be able to report messages to Microsoft in Outlook on the web using the Report Message add-in or the Report Phishing add-in.
  >
  > If you select **My organization's mailbox**, reported messages appear on the **User reported messages** tab on the **Submissions** page at <https://security.microsoft.com/reportsubmission>. But the **Result** value of these messages will always be empty, because the messages were not rescanned.
  >
  > If you use [Attack simulation training](attack-simulation-training-get-started.md) or a third-party product to do phishing simulations, you must configure the user submissions mailbox as a SecOps mailbox as previously described in the [Configuration requirements for the user submissions mailbox](#configuration-requirements-for-the-user-submissions-mailbox) section earlier in this article. If you don't, a user reporting a message might trigger a training assignment in the phishing simulation product.

  Regardless of your selection, the following settings are also available in the **Send the reported messages to** section:

  - **Let users choose if they want to report**: This setting controls the options that are available in the **Select reporting options that are available to users** section:

    - **Let users choose if they want to report** selected: You can select some, all or none of the settings in the **Select reporting options that are available to users** section.
    - **Let users choose if they want to report** not selected: You can select only one setting in the **Select reporting options that are available to users** section.

    - **Select reporting options that are available to users** section:
      - **Ask me before sending the message**
      - **Always report the message**
      - **Never report the message**

- **User reporting experience** section: The following settings are available:

  As shown on the page, if you select an option that sends the reported messages to Microsoft, the following text is also added to the notification:

  > Your email will be submitted as-is to Microsoft for analysis. Some emails might contain personal or sensitive information.

  - **Before reporting** tab: In the **Title** and **Message body** boxes, enter the descriptive text that users see before they report a message using the Report Message add-in or the Report Phishing add-in. You can use the variable `%type%` to include the submission type (junk, not junk, phishing, etc.).
  - **After reporting** tab: In the **Title** and **Confirmation message** boxes, enter the descriptive text that users see after they report a message using the Report Message add-in or the Report Phishing add-in. You can use the variable `%type%` to include the submission type.

  - **Only display when user reports phishing**: Select this option to display the **Before reporting** and **After reporting** notifications only when users report messages as phishing. Otherwise, the notifications are shown for all reported messages.

- **Email notifications for admin review results** section: The following settings are available:

  - **Specify Office 365 email address to use as sender**: Select this setting and enter the email address in the box that appears.
  
  - **Customize notifications**: Click this link to customize the email notification that's sent after an admin reviews and marks a reported messages.

    On the **Customize confirmation message** flyout that appears, configure the following settings:

    - **Phishing**, **Junk** and **No threats found** tabs: In the **Review result text** on some, none, or all of the tabs, enter the custom text to use.
    - **Footer** tab: The following options are available:
      - **Footer text**: Enter the custom message footer text to use.
      - **Display company logo**: Before select this option, you need to follow the instructions in [Customize the Microsoft 365 theme for your organization](../../admin/setup/customize-your-organization-theme.md) to upload your custom logo.

  When you're finished on the **Customize confirmation message** flyout, click **Confirm**.

- **Customize your organization's experience when reporting potential threats in quarantine** section:

  **Quarantine report message button**: Verify this setting is **On** ![Toggle on.](../../media/scc-toggle-on.png) to let users report messages from quarantine. Otherwise, turn this setting **Off** ![Toggle off.](../../media/scc-toggle-off.png).

When you're finished on the **User submissions** page, click **Save**. To restore the settings to their immediately previous values, click **Restore**.

### Third-party reporting tools options

You can turn off the Microsoft integrated reporting experience to use third-party message reporting tools to send reported messages to the user submissions mailbox.

The only requirement is that the original messages are included as uncompressed .EML or .MSG attachments in messages that are sent to user submissions mailbox. In other words, don't just forward the original messages to the user submissions mailbox.

> [!NOTE]
> If multiple email attachments exist in the message, then the submission will be discarded. We only support message with one email attachment.

The message formatting requirements are described in the next section. The formatting is optional, but reported messages don't follow the prescribed format, the reported messages are always identified as phishing.

When **Microsoft Outlook Report Message button** is **Off** ![Toggle off.](../../media/scc-toggle-off.png) the following settings are available on the **User submissions** page:

- **Microsoft and my organization's mailbox**: In the box that appears, enter the email address of an existing Exchange Online mailbox to use as the user submissions mailbox. Distribution groups are not allowed.

- **Customize your organization's experience when reporting potential threats in quarantine** section:

  **Quarantine report message button**: Verify this setting is **On** ![Toggle on.](../../media/scc-toggle-on.png) to let users report messages from quarantine. Otherwise, turn this setting **Off** ![Toggle off.](../../media/scc-toggle-off.png).

When you're finished on the **User submissions** page, click **Save**. To restore the settings to their immediately previous values, click **Restore**.

#### Message submission format

To correctly identify the original attached messages, messages sent to the custom mailbox require specific formatting. If the messages don't use this format, the original attached messages are always identified as phishing.

To specify the reason why the original attached messages were reported, messages sent to the user submissions mailbox must meet the following criteria:

- The original message attachment is unmodified.
- The Subject line (Envelope Title) of messages sent to the user submissions mailbox must start with one of the following prefix values:
  - `1|` or `Junk:`.
  - `2|` or `Not junk:`.
  - `3|` or `Phishing:`.

  For example:

  - `3|This text in the Subject line is ignored by the system`
  - `Not Junk:This text in the Subject line is also ignored by the system`

  Messages that don't follow this format will not display properly on the **Submissions** page at <https://security.microsoft.com/reportsubmission>.

## Use Exchange Online PowerShell to configure the user submissions mailbox for email

After you [connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell), you use the **\*-ReportSubmissionPolicy** and **\*-ReportSubmissionRule** cmdlets to manage and configure the user submissions mailbox and the related settings.

In Exchange Online PowerShell, the basic elements of the user submissions mailbox settings are:

- **The report submission policy**: Turns the Microsoft integrated reporting experience on or off, turns sending reported messages to Microsoft on or off, turns sending reported messages to the user submissions mailbox on or off, and most other settings.
- **The report submission rule**: Specifies the email address of the user submissions mailbox or a blank value when the user submissions mailbox isn't used (report messages to Microsoft only).

The difference between these two elements isn't obvious when you manage user submissions mailbox settings in the Microsoft 365 Defender portal:

- There's only report submission policy named DefaultReportSubmissionPolicy and one report submission rule that's named DefaultReportSubmissionRule by default.

  If you've never gone to <https://security.microsoft.com/userSubmissionsReportMessage>, there's no report submission policy or report submission rule (the Get-ReportSubmissionPolicy and Get-ReportSubmissionRule cmdlets return nothing).

  As soon as you visit <https://security.microsoft.com/userSubmissionsReportMessage> and even before you configure any settings, the report submission policy is created with the default values and is visible in PowerShell.

  As after you configure and save the settings at <https://security.microsoft.com/userSubmissionsReportMessage> to specify a user submissions mailbox (Microsoft integrated reporting experience or third-party tools), the report submission rule named DefaultReportSubmissionRule is automatically created. Note that it takes several seconds before the rule is visible in PowerShell.

- You can delete the report submission rule and recreate it with a different name, but the rule is always associated with the report submission policy whose name you can't change. So, we recommend that you name the rule DefaultReportSubmissionRule whenever you create or recreate the rule.

- When you specify the email address of the user submissions mailbox in the Microsoft 365 Defender portal, that value is primarily set in the report submission rule, but the value is also copied into the related properties in the report submission policy. In PowerShell, when you set the email address in the rule, the value isn't copied into the related properties in the policy. For consistency with the Microsoft 365 Defender portal and for clarity, we recommend that you add or update the email address in the policy and the rule.

### Use PowerShell to view the report submission policy and the report submission rule

To view the report submission policy, run the following command in Exchange Online PowerShell:

```powershell
Get-ReportSubmissionPolicy
```

To view the report submission rule, run the following command:

```powershell
Get-ReportSubmissionRule
```

To view both the policy and the rule at the same time, run the following commands:

```powershell
Write-Output -InputObject `r`n,"Report Submission Policy",("-"*79); Get-ReportSubmissionPolicy; Write-Output -InputObject `r`n,"Report Submission Rule","("-"*79)"; Get-ReportSubmissionRule
```

Remember, if you've never gone to <https://security.microsoft.com/userSubmissionsReportMessage> or manually created the report submission policy or the report submission rule in PowerShell, there is no report submission policy or report submission rule, so the **Get-ReportSubmissionPolicy** and **Get-ReportSubmissionRule** cmdlets return nothing.

For detailed syntax and parameter information, see [Get-ReportSubmissionPolicy](/powershell/module/exchange/get-reportsubmissionpolicy) and [Get-ReportSubmissionRule](/powershell/module/exchange/get-reportsubmissionrule).

### Use PowerShell to create the report submission policy and the report submission rule

If the **Get-ReportSubmissionPolicy** and **Get-ReportSubmissionRule** cmdlets return no output, you can create the report submission policy and the report submission rule. If you try to create them when they already exist, you'll get an error.

Always create the report submission policy first, because you specify the report submission policy in the report submission rule.

For detailed syntax and parameter information, see [New-ReportSubmissionPolicy](/powershell/module/exchange/new-reportsubmissionpolicy) and [New-ReportSubmissionRule](/powershell/module/exchange/new-reportsubmissionrule).

#### Use PowerShell to enable the Microsoft integrated reporting experience with report to Microsoft only

This example creates the report submission policy with the default settings (the same settings as when you first visit <https://security.microsoft.com/userSubmissionsReportMessage>, but before you configure or save any settings):

- The Microsoft integrated reporting experience is turned on: **Microsoft Outlook Report Message button** toggle: **On** ![Toggle on.](../../media/scc-toggle-on.png).

- Send reported messages to Microsoft only: **Send the reported messages to** \> **Microsoft**.

- No user submissions mailbox is needed or specified, so the report submission rule is not created.

```powershell
New-ReportSubmissionPolicy
```

#### Use PowerShell for the Microsoft integrated reporting experience with report to Microsoft and the user submissions mailbox

This example creates the report submission policy and the report submission rule with the following settings:

- The Microsoft integrated reporting experience is turned on: **Microsoft Outlook Report Message button** toggle: **On** ![Toggle on.](../../media/scc-toggle-on.png). On the **New-ReportSubmissionPolicy** cmdlet, the default value of the _EnableReportToMicrosoft_ parameter is `$true` and the default value of the _EnableThirdPartyAddress_ parameter is `$false`, so you don't need to use them.

- Send reported messages to Microsoft and the user submissions mailbox: **Send the reported messages to** \> **Microsoft and my organization's mailbox**:

  - **New-ReportSubmissionPolicy**: `-ReportJunkToCustomizedAddress $true -ReportJunkAddresses <emailaddress> -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses <emailaddress> -ReportPhishToCustomizedAddress $true -ReportPhishAddresses <emailaddress>`.
  - **Set-ReportSubmissionRule**: `SentTo <emailaddress>`.

  In this example, the email address of the user submissions mailbox is reportedmessages@contoso.com in Exchange Online (you can't specify an external email address).

  > [!NOTE]
  > You must use the same email address value in all parameters that identify the user submissions mailbox.

The remaining parameters are required to create the report submission policy. In this example, the default values are used. If you don't specify the default values as described in this example, additional parameters and settings might be required:

- **Let users choose if they want to report** selected (`-DisableUserSubmissionOptions $false`) and no **Select reporting options that are available to users** options are selected (the default _UserSubmissionOptions_ parameter value `0` is used).
- **User reporting experience** section:
  - Nothing is entered in the **Title** and **Message body** boxes on the **Before reporting** or **After reporting** tabs (`-EnableCustomizedMsg $false`).
  - **Only display when user reports phishing** selected (`-OnlyShowPhishingDisclaimer $true`).
- **Email notifications for admin review results** section:
  - **Specify Office 365 email address to use as sender** not selected (`-EnableCustomNotificationSender $false`).
  - **Customize notifications** link \> **Customize confirmation** flyout \> **Footer** tab: **Display company logo** not selected (`-EnableOrganizationBranding $false`).
- **Customize your organization's experience when reporting potential threats in quarantine** section:

  **Quarantine report message button** toggle: **On** ![Toggle on.](../../media/scc-toggle-on.png) (`-DisableQuarantineReportingOption $false`).

```powershell
$usersub = "reportedmessages@contoso.com"

New-ReportSubmissionPolicy -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub -DisableUserSubmissionOptions $false -EnableCustomizedMsg $false -OnlyShowPhishingDisclaimer $true -EnableCustomNotificationSender $false -EnableOrganizationBranding $false -DisableQuarantineReportingOption $false

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

#### Use PowerShell for the Microsoft integrated reporting experience with report to the user submissions mailbox only

This example creates the report submission policy and the report submission rule with the following settings:

- The Microsoft integrated reporting experience is turned on: **Microsoft Outlook Report Message button** toggle: **On** ![Toggle on.](../../media/scc-toggle-on.png). On the **New-ReportSubmissionPolicy** cmdlet: `-EnableReportToMicrosoft $false`. The default value of the _EnableThirdPartyAddress_ parameter is `$false`, so you don't need to use it.

- Send reported messages to the user submissions mailbox only: **Send the reported messages to** \> **My organizations mailbox**:

  - **New-ReportSubmissionPolicy**: `-ReportJunkToCustomizedAddress $true -ReportJunkAddresses <emailaddress> -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses <emailaddress> -ReportPhishToCustomizedAddress $true -ReportPhishAddresses <emailaddress>`.
  - **Set-ReportSubmissionRule**: `SentTo <emailaddress>`.

  In this example, the email address of the user submissions mailbox is userreportedmessages@fabrikam.com in Exchange Online (you can't specify an external email address).

  > [!NOTE]
  > You must use the same email address value in all parameters that identify the user submissions mailbox.

As in the previous example, the same remaining parameters are required to create the report submission policy. As in the previous example, the default values of those parameters are also used:

```powershell
$usersub = "userreportedmessages@fabrikam.com"

New-ReportSubmissionPolicy -EnableReportToMicrosoft $false -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub -DisableUserSubmissionOptions $false -EnableCustomizedMsg $false -OnlyShowPhishingDisclaimer $true -EnableCustomNotificationSender $false -EnableOrganizationBranding $false -DisableQuarantineReportingOption $false

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

#### Use PowerShell for third-party reporting tools

This example creates the report submission policy and the report submission rule with the following settings:

- The Microsoft integrated reporting experience is turned off: **Microsoft Outlook Report Message button** toggle: **Off** ![Toggle off.](../../media/scc-toggle-off.png). On the **New-ReportSubmissionPolicy** cmdlet: `-EnableReportToMicrosoft $false -EnableThirdPartyAddress $true`.

- Use the following parameters to specify the email address of the user submissions mailbox:

  - **New-ReportSubmissionPolicy**: `-ThirdPartyReportAddresses <emailaddress>`
  - **Set-ReportSubmissionRule**: `SentTo <emailaddress>`.

  In this example, the email address of the user submissions mailbox is thirdpartyreporting@wingtiptoys.com in Exchange Online (you can't specify an external email address

  > [!NOTE]
  > You must use the same email address value in all parameters that identify the user submissions mailbox.

The remaining parameters are required to create the report submission policy successfully. In this example, the default values are used. No other options are available in the report submission policy when you turn off the Microsoft integrated reporting experience:

- **Customize your organization's experience when reporting potential threats in quarantine** section:

  **Quarantine report message button** toggle: **On** ![Toggle on.](../../media/scc-toggle-on.png) (`-DisableQuarantineReportingOption $false`).

```powershell
$usersub = "thirdpartyreporting@wingtiptoys.com"

New-ReportSubmissionPolicy -EnableReportToMicrosoft $false -EnableThirdPartyAddress $true -ThirdPartyReportAddresses $usersub -DisableQuarantineReportingOption $false

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

### Use PowerShell to modify the report submission policy and the report submission rule

The same settings are available when you modify the report submission policy in PowerShell as when you created the policy as described in [the previous section](#use-powershell-to-create-the-report-submission-policy-and-the-report-submission-rule). The major difference is: the additional parameters that were required to create the policy (for example, _DisableQuarantineReportingOption_) are no long required. But, you might have to undo or nullify some important settings that you previously configured or didn't configure. And, you might need to create or delete the report submission rule to allow or prevent reporting to a user submissions mailbox.

For detailed syntax and parameter information, see [Set-ReportSubmissionPolicy](/powershell/module/exchange/set-reportsubmissionpolicy).

The following examples show how to change the user reporting experience without concern for the existing settings or values:

- Change to **Microsoft integrated reporting experience \> report to Microsoft only**:

  ```powershell
   Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $true -EnableThirdPartyAddress $false -ThirdPartyReportAddresses $null -ReportJunkToCustomizedAddress $false -ReportJunkAddresses $null -ReportNotJunkToCustomizedAddress $false -ReportNotJunkAddresses $null -ReportPhishToCustomizedAddress $false -ReportPhishAddresses $null

  Get-ReportSubmissionRule | Remove-ReportSubmissionRule
  ```

- Change to **Microsoft integrated reporting experience \> report to Microsoft and the user submissions mailbox** (for example, reportedmessages@contoso.com):

  ```powershell
  $usersub = "reportedmessages@contoso.com"

  Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $true -EnableThirdPartyAddress $false -ThirdPartyReportAddresses $null -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub
  ```

  The following command is required only if you don't already have the report submission rule:

  ```powershell
  New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
  ```

- Change to **Microsoft integrated reporting experience \> report to the user submissions mailbox only** (for example, userreportedmessages@fabrikam.com):

  ```powershell
  $usersub = "userreportedmessages@fabrikam.com"

  Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $false -EnableThirdPartyAddress $false -ThirdPartyReportAddresses $null -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub
  ```

  The following command is required only if you don't already have the report submission rule:

  ```powershell
  New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
  ```

- Change to **Third-party reporting tools** (for example, thirdpartyreporting@wingtiptoys.com):

  ```powershell
  $usersub = "thirdpartyreporting@wingtiptoys.com"

  Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $false -EnableThirdPartyAddress $true -ThirdPartyReportAddresses $usersub -ReportJunkToCustomizedAddress $false -ReportJunkAddresses $null -ReportNotJunkToCustomizedAddress $false -ReportNotJunkAddresses $null -ReportPhishToCustomizedAddress $false -ReportPhishAddresses $null
  ```

  The following command is required only if you don't already have the report submission rule:

  ```powershell
  New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
  ```

The only meaningful setting that you can modify in the report submission rule is the email address of the user submissions mailbox (the _SentTo_ parameter value). For example:

```powershell
Get-ReportSubmissionRule | Set-ReportSubmissionRule -SentTo newemailaddress@contoso.com
```

For detailed syntax and parameter information, see [Set-ReportSubmissionRule](/powershell/module/exchange/set-reportsubmissionrule).

To temporarily disable sending email messages to the user submissions mailbox (Microsoft integrated reporting experience or third-party tools) without deleing the report submission rule, use [Disable-ReportSubmissionRule](/powershell/module/exchange/disable-reportsubmissionrule). For example:

```powershell
Get-ReportSubmissionRule | Disable-ReportSubmissionRule -Confirm:$false
```

To enable the report submission rule again, use [Enable-ReportSubmissionRule](/powershell/module/exchange/enable-reportsubmissionrule). For example:

```powershell
Get-ReportSubmissionRule | Disable-ReportSubmissionRule -Confirm:$false
```

### Use PowerShell to remove the report submission policy and the report submission rule

To start over with the default settings of the report submission policy, you can delete it and recreate it. Removing the report submission policy does not remove the report submission rule, and vice-versa.

To remove the report submission policy, run the following command in Exchange Online PowerShell:

```powershell
Remove-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy
```

To remove the report submission rule, run the following command:

```powershell
Get-ReportSubmissionRule | Remove-ReportSubmissionRule
```

To remove both the report submission policy and report submission rule in the same command without prompts, run the following command:

```powershell
Remove-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy; Get-ReportSubmissionRule | Remove-ReportSubmissionRule -Confirm:$false
```

For detailed syntax and parameter information, see [Remove-ReportSubmissionPolicy](/powershell/module/exchange/remove-reportsubmissionpolicy) and [Remove-ReportSubmissionRule](/powershell/module/exchange/remove-reportsubmissionrule).