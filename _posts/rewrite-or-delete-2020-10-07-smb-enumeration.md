---
layout: post
title:  "SMB enumeration"
date:   2020-10-07 00:00:00 +0200
categories: jekyll update
---
There are several tools to enumerate *SMB*, a protocol that has had a really bad history when it comes to vulnerabilities.

These are some of the tools I like using when I need to enumerate *SMB*, ordered by their respective usefulness (in my opinion):
* `smbmap -u '' -p '' -H <IP>`

* `smbclient -N -L //<IP>/`

* `rpcclient -N //<IP>/`

* `nmap --script=smb-vuln-* <IP>`

* `nbtscan <IP>`

* `nmblookup <IP>`

* `enum4linux <IP>`