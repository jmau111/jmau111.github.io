---
title: "SELinux: Beginner's guide"
description: "As a developer, a sysadmin, or a hacker, you might want to know a bit more about SELinux."
slug: "selinux-guide-beginners"
date: "2023-01-18"
type: "post"
authors: ["jmau111"]
---

As a developer, a sysadmin, or a hacker, you might want to know a bit more about SELinux.

What is it ? What's the point?
--------

SELinux is a MAC system (Mandatory Access Control) created by the NSA. The purpose is to isolate privileged processes and ease security policy setup.

SELinux will prevent an application from doing something that is not explicitly allowed by a policy. It's not meant to avoid memory leaks or kernel exploits, but it's a serious mitigation to consider.

In other words, it "allows resources (e.g., apps, user logins) to be given only the privileges they need," which can confine damages and leakages.

SELinux is also a labeling system. That's how you can gain more control access granted to each processes by the kernel.

[Source: SELinux Notebook](https://github.com/SELinuxProject/selinux-notebook/blob/main/src/title.md)

SELinux is a labeling system
--------

One of the best resources on the topic is [the SELinux policy guide](https://opensource.com/business/13/11/selinux-policy-guide). You'll get an introduction to the labeling system with visuals and analogies.

Basically, SELinux associates labels to resources like processes and files. This allows controlling how a process label can access to a file label, for example. However, there is no ownership or UID involved, so it can mitigate privilege escalations.

You can modify these labels to configure SELinux correctly for your application (e.g., to control the directory that contain your website).

In a nutshell, labels are defined by **types** (e.g., `httpd_can_` for Apache). If one instance/process gets hacked, it's confined.

Very basic install
--------

Replace AppArmor by SELinux if it's installed (as it's the same purpose: isolation), and enable logging with the `auditd` package:

```
sudo systemctl stop apparmor
sudo apt purge -y apparmor
sudo apt install -y selinux-basics selinux-policy-default auditd
sudo selinux-activate
sudo systemctl enable auditd
reboot # required
```

You have two main modes for SELinux:

* `Permissive` mode: don't block anything but log events
* `Enforcing` mode: block denied operations

If you are on a server, ensure ssh and other services you might need are allowed before quitting:

```
semanage port -l
```

You can inspect logs in `/var/log/audit/audit.log`.

About hard configs
--------

Be extra careful with the `/etc/selinux/config` file (global config). SELinux is not an ordinary package.

For example, in the config file, `SELINUXTYPE` is set to `default` by default, which makes sense, but if you decide to use `mls` (Multi-Level Security) or `src` (for custom policies), ensure you have set the corresponding rules on your system (SELinux expect specific files in specific directories).

Otherwise, you might get locked out.

Even the `sudo setenforce 1` command (set mode "enforcing") can cause problems. It's usually not set right away. You'd better put your server on permissive mode temporarily to check your logs and fine tune policies before.

Policies in very short
--------

SELinux Policies are sets of rules to secure the system. To list them, type the following:

```
semanage boolean -l
```

To configure your app with SELinux, you will have to modify the corresponding booleans.

The **context** determines access control. SELinux maps pretty much everything with attributes such as **roles**, **domains**, **types**, and **levels**.

You can use the `-Z` switch (added by SELinux to system utilities) to see the context:

```
id -Z
```

It works with `ls` (files) and `ps` (processes) too.

These attributes allows confining entities, so they only access to the necessary resources and actions.

To see how SELinux maps specific entities, use the `semanage` command:

```
semanage user -l
semanage port -l
```

Modifying the context
--------

Once you get more familiar with SELinux, you want to modify values and include your custom app. This part can be tricky, especially at the beginning, and will likely require some troubleshooting, as errors happen frequently.

In any case, a basic example could be:

```
semanage fcontext -a httpd_sys_rw_content_t "/var/www/html/wordpress/wp-content/uploads(/.*)?"
```

The above assigns the `httpd_sys_rw_content_t` context to the upload directory of a WordPress installation to allow Apache (and ultimately WP users) to upload files.

We do that because, by default, `/var/www/html` is set to `httpd_sys_content_t`, without write access.

Pros & cons
--------

The SELinux approach is a bit radical. The default config applies the old strict and targeted policies, which means custom apps and processes will likely be denied.

You will have some troubleshooting!

While it requires time and efforts, it's a mature solution that can mitigate various attacks significantly even with vulnerable apps. Although, I'm not saying that to give you a false impression of security. Don't consider SELinux as the ultimate shield for any flaw at a higher level.

Useful links
--------

* [the SELinux policy guide](https://opensource.com/business/13/11/selinux-policy-guide)
* [the SELinux Notebook](https://github.com/SELinuxProject/selinux-notebook/blob/main/src/title.md)
* [RedHat - What is SELinux?](https://www.redhat.com/en/topics/linux/what-is-selinux)
* [DigitalOcean - What is SELinux?](https://www.digitalocean.com/community/tutorials/what-is-selinux)
* [SELinux cheat sheet](https://aerostitch.github.io/linux_and_unix/RedHat/selinux.html)
* [ArchWiki - SELinux](https://wiki.archlinux.org/title/SELinux)
* [SELinux project](https://github.com/selinuxproject/)

Wrap up
--------

You won't turn your machine into a fortress with SELinux and its default configuration, but it's a significant mitigation to various attacks, including privilege escalations.

It's also a great support to learn important concepts in cybersecurity.
