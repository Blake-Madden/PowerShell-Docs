---
description: PowerShell has several features designed to improve the security of your scripting environment.
ms.date: 07/08/2024
title: PowerShell security features
---
# PowerShell security features

PowerShell has several features designed to improve the security of your scripting environment.

## Execution policy

PowerShell's execution policy is a safety feature that controls the conditions under which
PowerShell loads configuration files and runs scripts. This feature helps prevent the execution of
malicious scripts. You can use a Group Policy setting to set execution policies for computers and
users. Execution policies only apply to the Windows platform.

For more information see [about_Execution_Policies][02].

## Use of the SecureString class

PowerShell has several cmdlets that support the use of the `System.Security.SecureString` class.
And, as with any .NET class, you can use **SecureString** in your own scripts. However, Microsoft
doesn't recommend using **SecureString** for new development. Microsoft recommends that you avoid
using passwords and rely on other means to authenticate, such as certificates or Windows
authentication.

PowerShell continues to support the **SecureString** class for backward compatibility. Using a
**SecureString** is still more secure than using a plain text string. By default, PowerShell doesn't
show the unprotected value of a **SecureString** object. However, **SecureString** can be easily
converted to a plain text string. For a full discussion about using **SecureString**, see the
[System.Security.SecureString class][01] documentation.

## Module and script block logging

Module Logging allows you to enable logging for selected PowerShell modules. This setting is
effective in all sessions on the computer. Pipeline execution events for the specified modules are
recorded in the Windows PowerShell log in Event Viewer.

Script Block Logging enables logging for the processing of commands, script blocks, functions, and
scripts - whether invoked interactively, or through automation. This information is logged to the
**Microsoft-Windows-PowerShell/Operational** event log.

For more information, see the following articles:

- [about_Group_Policy_Settings][03]
- [about_Logging_Windows][06]
- [about_Logging_Non-Windows][05]

## AMSI Support

The Windows Antimalware Scan Interface (AMSI) is an API that allows application actions to be passed
to an antimalware scanner, such as Windows Defender, to be scanned for malicious payloads. Beginning
with PowerShell 5.1, PowerShell running on Windows 10 (and higher) passes all script blocks to AMSI.

PowerShell 7.3 extends the data that's sent to AMSI for inspection. It now includes all invocations
of .NET method members.

For more information about AMSI, see [How AMSI helps][09].

## Constrained language mode

**ConstrainedLanguage** mode protects your system by limiting the cmdlets and .NET types that can be
used in a PowerShell session. For a full description, see [about_Language_Modes][04].

## Application Control

Windows 10 includes two technologies, [Windows Defender Application Control (WDAC)][08] and
[AppLocker][07] that you can use to control applications. They allow you to create a lockdown
experience to help secure your PowerShell environment.

For more information about how PowerShell supports AppLocker and WDAC, see
[Using Windows Defender Application Control][10].

### Changes in PowerShell 7.4

On Windows, when PowerShell runs under a Windows Defender Application Control (WDAC) policy, it
changes its behavior based on the defined security policy. Under a WDAC policy, PowerShell runs
trusted scripts and modules allowed by the policy in Full Language mode. All other scripts and
script blocks are untrusted and run in Constrained Language mode. PowerShell throws errors when the
untrusted scripts attempt to perform disallowed actions. It's difficult to know why a script fails
to run correctly in Constrained Language mode.

PowerShell 7.4 now supports WDAC policies in **Audit** mode. In audit mode, PowerShell runs the
untrusted scripts in Constrained Language mode but logs messages to the event log instead of
throwing errors. The log messages describe what restrictions would apply if the policy was in
**Enforce** mode.

### Changes in PowerShell 7.3

- PowerShell 7.3 now supports the ability to block or allow PowerShell script files via the WDAC API.

### Changes in PowerShell 7.2

- There was a corner-case scenario in AppLocker where you only have **Deny** rules and constrained
  mode isn't used to enforce the policy that allows you to bypass the execution policy. Beginning in
  PowerShell 7.2, a change was made to ensure AppLocker rules take precedence over a
  `Set-ExecutionPolicy -ExecutionPolicy Bypass` command.

- PowerShell 7.2 now disallows the use of the `Add-Type` cmdlet in a **NoLanguage** mode PowerShell
  session on a locked down machine.

- PowerShell 7.2 now disallows scripts from using COM objects in AppLocker system lock down
  conditions. Cmdlets that use COM or DCOM internally aren't affected.

### Security Servicing Criteria

PowerShell follows the [Microsoft Security Servicing Criteria for Windows][12]. The table below
outlines the features that meet the servicing criteria and those that do not.

|                  Feature                   |       Type       |
| ------------------------------------------ | ---------------- |
| System Lockdown - with WDAC                | Security Feature |
| Constrained language mode - with WDAC      | Security Feature |
| System Lockdown - with AppLocker           | Defense in Depth |
| Constrained language mode - with AppLocker | Defense in Depth |
| Execution Policy                           | Defense in Depth |

## Software Bill of Materials (SBOM)

Beginning with PowerShell 7.2, all install packages contain a Software Bill of Materials (SBOM). The
SBOM is found at `$PSHOME/_manifest/spdx_2.2/manifest.spdx.json`. The creation and publishing of the
SBOM is the first step to modernize Federal Government cybersecurity and enhance software supply
chain security.

The PowerShell team is also producing SBOMs for modules that they own but ship separately from
PowerShell. SBOMs will be added in the next release of the module. For modules, the SBOM is
installed in the module's folder under `_manifest/spdx_2.2/manifest.spdx.json`.

For more information about this initiative, see the blog post
[Generating Software Bills of Materials (SBOMs) with SPDX at Microsoft][11].

<!-- link references -->
[01]: /dotnet/fundamentals/runtime-libraries/system-security-securestring
[02]: /powershell/module/microsoft.powershell.core/about/about_execution_policies
[03]: /powershell/module/microsoft.powershell.core/about/about_group_policy_settings#turn-on-module-logging
[04]: /powershell/module/microsoft.powershell.core/about/about_language_modes
[05]: /powershell/module/microsoft.powershell.core/about/about_logging_non-windows
[06]: /powershell/module/microsoft.powershell.core/about/about_logging_windows
[07]: /windows/security/threat-protection/windows-defender-application-control/applocker/what-is-applocker
[08]: /windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control
[09]: /windows/win32/amsi/how-amsi-helps
[10]: application-control.md
[11]: https://devblogs.microsoft.com/engineering-at-microsoft/generating-software-bills-of-materials-sboms-with-spdx-at-microsoft/
[12]: https://www.microsoft.com/msrc/windows-security-servicing-criteria
