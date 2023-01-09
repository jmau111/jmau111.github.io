---
title: "Personal thoughts on OS security"
description: "macOS, Windows, and Linux have very different approaches of security."
slug: "operating-systems-security"
date: "2023-01-09"
type: "post"
authors: ["jmau111"]
---

I wrote some guides about operating systems I've used for many years as a casual user but also as a developer.

## What you will read there

Various analysis and personal thoughts about each OS. The Linux guide is more specific, though, and highlights a special mode that remains quite misknown by most users.

You also find the best resources for beginners to me (e.g., useful links for various tools and other guides), for example, to harden your OS or learn about attacking techniques.

The latest versions of Windows are significantly more secure than the previous ones (e.g., XP, Vista), especially when you enable mitigations.

It does not solve all issues, and none of these OSes can prevent its users from installing malicious third-party apps. However, you get more granularity, better kernel isolation, and enhanced memory protection.

## 7 key points

* **never rely on default configs** for your security, as OSes usually do not enable all mitigations for you automatically (e.g., for the usability)
* aggressive patch management (~ keeping all the things up-to-date) is not optional
* built-in firewalls are fine but not sufficient, as they don't watch outgoing traffic by default
* as a developer/security pro, you'd better learn your system internals
* don't focus on the command lines, because the concepts are more important, but, still, learn a few of them (or make aliases)
* use password managers, regardless of the system
* compartmentalize

## 7 security paradoxes

What I mean by "paradoxes":

* having better security layers but disabled by default
* shifting the responsibility to the end-users, like "everything is there, now go"
* building security "fortresses" (e.g., Mac's approach), but leaving entire segments and processes undocumented
* digitizing everything, including lifestyles, but exposing critical data publicly to algorithms and OSINT
* betting everything on tools that will be deprecated very soon
* sacrificing safety for very little more convenience or speed (e.g., going full wireless but using cheap unencrypted devices)
* taking measures that increase the risk of losing data significantly (e.g., encryption is cool but what happen if you lose the key? External devices can cease operating)

From what I've seen so far, these paradoxes happen quite often.

## Read the READMEs

* [macOS](https://github.com/jmau111-org/macos_security)
* [Windows](https://github.com/jmau111-org/windows_security)
* [Linux recovery](https://github.com/jmau111-org/linux_recovery_abuse)
