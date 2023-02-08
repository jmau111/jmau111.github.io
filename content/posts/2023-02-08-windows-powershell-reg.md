---
title: "Windows PowerShell and reg commands"
description: "Review useful tricks to attack Windows and its registry, but also learn how to mitigate these threats"
slug: "windows-powershell-reg"
date: "2023-02-08"
type: "post"
authors: ["jmau111"]
---

I've been "PowerShelling" over the past weeks, so here are commands and tricks I find helpful for Windows security.

## System commands are prone to attacks

While it should be noted that most attacks require elevated privileges, like "admin" capabilities, misconfigured machines usually allow advanced enumeration, and sometimes even privilege escalation. Besides, many organizations attribute admin roles to some employees only for convenience, which breaks the least privilege principle and grant way too many privileges to these accounts.

The PowerShell syntax is pretty convenient to use, and even if security products and policies can detect and even block `PowerShell.exe`, there are ways to run the same commands without using the system executable. You can also obfuscate malicious commands to evade detection, but the two guides I'll link here won't cover that specific part to keep things as simple as possible.

These commands are powerful and allow extracting sensitive data, but you can also disable security settings and functionalities, as the Registry hives control pretty much everything on the system.

## Misknown mitigations

I've found lots of information about attacking techniques and even free tools to automate enumeration and privesc. In contrast, there are very few resources to mitigate the threats and harden the system.

As usual, nothing is bulletproof, but you might leverage advanced features like JEA (Just Enough Administration) that can improve PowerShell security significantly. With a robust configuration and an appropriate logging system, you can mitigate entire classes of vulnerabilities and attacks, and, above all, understand what's happening.

## Log & monitor

Indeed, you might enable cmd prompt for specific users only, for example, administrators. After some tests, you may deploy the appropriate GPOs (Group Policies) in your environment.

However, there are many contexts where it's just not possible without jamming legitimate processes. In this case, it can be very hard and costly to maintain such configuration.

Instead, you may enable advanced PowerShell logging and connect your logs to a SIEM (Security information and event management) or similar solutions that can monitor suspicious usages efficiently and raise custom alerts.

## Read the READMEs

The complete cheat sheets are available here:

* [Cheat Sheet PowerShell: attack & defense](https://github.com/jmau111-org/powershell_commands)
* [Cheat Sheet: Windows reg commands](https://github.com/jmau111-org/windows_reg)
