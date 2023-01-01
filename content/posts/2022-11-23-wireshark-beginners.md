---
title: "Getting started with Wireshark 🦈"
description: "Just follow the packets."
slug: "wireshark-beginners"
date: "2022-11-23"
type: "post"
authors: ["jmau111"]
---

[Wireshark](https://www.wireshark.org/) is a fantastic tool to analyze network traffic.

You can read specific captured data (e.g., .pcap files) but live traffic from wired/wireless connections as well, and this free software supports a large range of protocols.

Wireshark is prepackaged in popular pen-testing distributions, like Kali or Parrot OS, but you can also install it on most operating systems (Windows, Linux, macOS, etc).

Blue teams analyze traffic regularly (e.g., malware infection). It's an essential step for Threat Hunting and similar investigations.

Learn that before
--------

* The OSI model (the different stages and layers during the data processing)
* network packets (how data are transmitted)
* LAN (private networks)
* HTTP requests
* TCP and UDP protocols
* HTTPS deeply (SSL encryption keys, TLS handshakes)
* C&C (also known as c2 or command and control servers)

It's not an exhaustive list at all, but it will be very complicated to go further if you don't have a clue about these basic concepts.

IoCs
--------

Wireshark lets you export various data (e.g., streams) in specific formats such as .dll, which you can then upload to popular databases like VirusTotal.

This way, you can determine quickly whether you've been infected with a known malware or not.

You can also share precious IoCs (Indicators Of Compromise) with the community to help others detect the threat and possibly catch further attacks.

What does Wireshark exactly?
--------

The purpose of the software is to intercept the traffic and make it readable enough for human analysis.

The interface is beginner-friendly and you can filter data quite precisely. Indeed, **filters** and **visualizations** are probably the best features provided by Wireshark.

The main difficulty is actually to be able to narrow down the search results and identify the true cause of your problems. That's why you have to be proficient (at least) in networks and other related concepts.

Setup
--------

I won't give you a full guide here, cause there are existing tutorials and videos about it (e.g., YouTube, blog posts), and some of them are great.

I recommend focusing on **profiles** and **preferences**, though, if you do your own researches. You can rearrange columns and panels, add your own controls, and even tweak the data formats to get something more meaningful.

You can also use your own troubleshooting' styles in Wireshark (e.g., custom colors).

How to get free samples
--------

Wireshark can read and export .pcap files, which are captured data from the intercepted traffic.

Wireshark uses popular libraries to capture the packets in various connections like Ethernet, Wifi, Bluetooth, etc. It allows you to manage all those "channels" and only select the ones that you need.

You can get good free samples [here](https://wiki.wireshark.org/SampleCaptures) to start practicing.

A typical scenario
--------

A typical case would be an alert on a web server, so the administrators would provide you the captured data.

Your job would be to open it with Wireshark and start filtering the HTTP requests to spot malicious activities.

By using Wireshark's display filters, you would narrow down the dataset to HTTP requests and responses on the targeted server and catch common attacks like basic enumeration or attempts to upload a malicious backdoor to pass arbitrary commands.

Cute, but how about SSL?
--------

They say Wireshark can monitor traffic, but if communications are SSL encrypted, as you would expect in real conditions, maybe it's another story, isn't it?

SSL is a very broad term, but let's keep it simple here. Yes, HTTPS encrypts data (transport layer in the OSI model), so using Wireshark would be pointless theoretically, as the packets would only reveal sequences of seemingly random strings.

Encryption relies on keys. In Windows, Mac, and Linux, it's possible to set an environment variable called `SSLKEYLOGFILE`.

It forces the system to store session keys in a specific location of our choice.

You can then configure Wireshark accordingly:

```
Wireshark > preferences > Protocols > SSL > (Pre)-Master-Secret log filename
```

In such setup, you can decrypt SSL traffic more conveniently. Visiting any SSL-enabled website with the browser would generate data.

There was a time where TLS relied on a master key for all sessions, so getting that master key would unlock pretty much everything.

Fortunately, it's no longer possible in the latest versions of the protocol and session keys are now renewed with every new TCP connections.

If you find the "key log file approach" a bit unrealistic, there are known tactics to intercept communications in real conditions, like Man-In-The-Middle attacks.

Cybercriminals could install malicious certificates on a targeted server to divert the traffic, analyze it, and then send it back. HTTPS makes these attacks way harder to achieve, but not impossible.

Best links
--------

Here are some of the best resources I've read to write this short introduction:

* [What is the OSI model?](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/)
* [Decrypt SSL with Wireshark](https://www.comparitech.com/net-admin/decrypt-ssl-with-wireshark/)
* [Wireshark tutorial: Decrypting HTTPS Traffic](https://unit42.paloaltonetworks.com/wireshark-tutorial-decrypting-https-traffic/)
