---
title: "How to sign your commits"
description: "Which method should you choose? SSH? GPG?"
slug: "sign-commits"
date: "2022-11-17"
type: "post"
authors: ["jmau111"]
---

GitHub has a [full guide](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits) to sign commits.

With a few command lines, you can configure Git to use your custom GPG or SSH key automatically, every time you add a new commit.

I prefer signing commits with an SSH key. Let's see why.
	
Why do we need to sign commits?
--------

The answer is relatively simple:

```
git config user.email "developer@yourcompany.org"
git commit -m "This modification looks legitimate"
```

Unless you sign your commits, there's no way to ensure the code was added by a legitimate member of the team.

That's why most Git platforms provide features to verify identities with cryptographic keys.
 
GPG keys in Git
--------

GPG encryption consists of generating a pair of keys. The process allows signing various kinds of messages, including your Git commits.

If you want to dig further, [read gnupg.org](https://www.gnupg.org/gph/en/manual.html).

Git has a long history with GPG, that's why most tutorials and commands on the topic will include it. However, SSH is more straightforward, to me.

How to use SSH keys
--------

SSH is usually available by default on your system. If you don't have it, it's easy to install.

Then:

```
touch ~/.ssh/allowed_signers
echo "{EMAIL} ssh-rsa {KEY}" > ~/.ssh/allowed_signers
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
git config --global commit.gpgsign true
git config --global gpg.format ssh
git config --global user.signingkey {/path/to/.ssh/key.pub}
```

Don't forget to replace data wrapped in `{}` with your own credentials.

How to check the signature
--------

```
git show --show-signature
```

If everything is ok, you'll get something like the following:

> Good "git" signature for {EMAIL} with ED25519 key {KEY}

Here, Git says "ED25519" because that's the algorithm I use to generate my keys.

_N.B.: Don't forget to upload your key in the desired platform. For example, GitHub lets you define an SSH key as a signing key (and not the classic authentication key)._

Is it really better?
--------

The main caveat of using SSH to sign commits is that old versions of OpenSSH are probably not compatible, but that should not be a major issue.

Also, it's essential to bear in mind that cryptographic keys should be used with caution. In other words, don't use the same pairs for multiple machines and purposes.


