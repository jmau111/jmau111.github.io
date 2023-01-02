---
title: "Filter outgoing traffic"
description: "Most people understand why incoming connections must be filtered, but what about outgoing traffic?"
slug: "outgoing-traffic"
date: "2023-01-02"
type: "post"
authors: ["jmau111"]
---

Whether it's for an individual or a corporate network, the vast majority of users understand why incoming connections must be filtered, but what about outgoing traffic?

Manual checking
--------

If you like the terminal, you can start with the `netstat` and the `lsof` commands, for example:

```
netstat -na | egrep 'LISTEN|ESTABLISH'
netstat -ano
lsof -i TCP # show TCP activity
lsof -i4 # ipv4 activity
lsof -i6 # ipv6 activity
lsof -i -n # open connections
```

You can enumerate open ports and conduct further investigations, but it will neither block further requests nor protect your system.

Attackers like to initiate requests from the targeted machine with various "connect back tools," as malicious TCP connections won't be detected, unless you take specific measures to block and monitor unwanted traffic.

The Built-in Firewall
--------

Regardless of the system, most built-in Firewall solutions will only deny incoming connections by default.

You may track Firewall activity in the logs or with a dedicated interface, but it does not protect you against all threats.

A malicious program could still initiate multiple outgoing connections, for example, to an attacker's machine, and there's nothing you can do about it, unless you set specific rules to restrict and monitor such traffic.

However, you might get some issues, as other programs and processes can be affected and could even stop working correctly if the rules are too strict.

More generally, many products, especially operating systems, do not enable all mitigations and security mechanisms by default to keep it usable by most users, so you should never rely on default configurations.

Inbound vs. outbound
--------

Inbound requests originate from outside whereas outbound requests originate from inside.

In my experience, outbound traffic is more tricky to filter, especially in a corporate environment. There's a huge market for that because of the complexity.

It's not uncommon to disrupt legitimate processes, especially during the setup, so it requires tests and efforts from both admins and users.

However, it's helpful to mitigate the exfiltration of sensitive data and catch unauthorized communications.

Basic Linux config
--------

Corporate environments will likely use proprietary software and other cloud solutions to tackle the 
problem. 

Otherwise, it's a significant risk for the business, as implanting a reverse shell can be achieved with a spear phishing campaign or by exploiting another vulnerability (e.g., Remote Code Execution, insider jobs).

In any case, at the individual scale, Linux users may _slightly_ improve their security by enabling the Uncomplicated Firewall (ufw):

```
sudo systemctl enable ufw
sudo ufw default deny forward
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw allow out to any port 80
sudo ufw allow out to any port 443
sudo ufw allow out to any port 53
sudo ufw logging high
sudo ufw reload
```

While it's **not the ultimate setup**, it's very easy to test. The above lines enable ufw, deny all traffic (forward, incoming, outgoing), but allow ports 53, 80, and 443 for outgoing traffic.

Indeed, you still need some ports to be allowed. Otherwise, everything is blocked, which looks like the "ultimate" protection (nothing comes in, nothing comes out) but is not usable at all.

More specifically, you can whitelist network interfaces (e.g., tun0) on very specific ports:

```
sudo ufw reset
sudo ufw enable
sudo ufw allow out on {YOUR_NETWORK_INTERFACE} to any port 53
sudo ufw allow out on {YOUR_NETWORK_INTERFACE} to any port 80
sudo ufw allow out on {YOUR_NETWORK_INTERFACE} to any port 443
sudo ufw reload
```

To find the name of your interfaces, just use the `ifconfig` command. You can even go further by specifying a protocol, such as UDP:

```
sudo ufw allow out on {YOUR_NETWORK_INTERFACE} to any port 53 proto udp
sudo ufw allow out on {YOUR_NETWORK_INTERFACE} to any port 443 proto tcp
sudo ufw reload
```

If you encounter some issues with the above rules and you can't fix it, you can hard reset ufw:

```
sudo ufw reset
sudo ufw enable
````

The cons
--------

Our basic ufw config isn't perfect at all, as legitimate programs often need to open outbound connections, so you'll likely have to configure some exceptions manually.

Besides, **an attacker can still use the allowed ports (e.g., 80, 443) to initiate malicious requests**. You might want to limit the outgoing traffic to only specific IPs or domains, but that's pretty hard to setup correctly and very frustrating for the users.

Moreover, rules and policies can be changed, for example, after a privilege escalation. If your precious whitelist is modified or removed, it's the same result. One layer is rarely enough!

That's why blocking everything almost blindly is not the best strategy. You need monitoring tools and, if possible, custom alerts on the side.

Best tools
--------

### Disclaimer

Most packages might be best suited for an individual usage and are only suggested for your tests.

Besides, their setup might be tricky, so read each documentation carefully before. Windows and macOS apps should be easier to configure, though.

### Linux tools

* [htop](https://htop.dev/)
* [nethogs](https://github.com/raboof/nethogs)
* [nagios](https://www.nagios.org/)
* [tcpflow](https://github.com/simsong/tcpflow)
* [tcpreplay](https://github.com/appneta/tcpreplay)
* iftop
* [netfilter & iptables](https://www.netfilter.org/)

### macOS apps

* [LuLu](https://objective-see.com/products/lulu.html)
* [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html)

### Windows software

* [simplewall](https://github.com/henrypp/simplewall)
* [tinywall](https://tinywall.pados.hu/download.php)

Wrap up
--------

Filtering incoming requests is necessary, but you cannot skip the outgoing traffic.

Security without monitoring seems hazardous, so blocking requests is not enough. You should also watch your traffic and collect data to understand what's happening.
