---
layout: post
title:  "Learn port forwarding and tunneling"
date:   2020-09-24 00:00:00 +0200
categories: jekyll update
---

*Port redirection*, *tunneling* and *traffic encapsulation* are a **must-know** if you are a *sysadmin* or *pentester*, as <u>they provide the ability to change the direction of the network traffic, and encapsulate a protocol inside a different one</u>, allowing the operator to, for example, **access restricted networkt, bypass firewall protections and much more**.

Let's start talking about *port forwarding*.

## 1 - Port forwarding/redirection
Let's explain *port forwarding* using a simple example:

Supose that you are at work on your computer, and you want to navigate to *Github*, but you can't because **there is a firewall that only allows communications on port 123 (NTP)***. You try to go to *Github* but the requests get dropped by the firewall and the page doesn't load.

So what do you do?

1. On your local linux machine, you install the tool **rinetd** with the command `apt install rinetd`.
1. You open */etc/rinetd.conf* with your favorite editor.
1. Suposing that your work IP is *1.2.3.4*, the IP of the *Github* webpage is *5.6.7.8* and the IP of your home machine is *a.b.c.d*, you add the following line:
```
#bindadress    bindport    connectaddress  connectport
 1.2.3.4       123         5.6.7.8         80
```
That line tells your home system to redirect (<i>forward</i>) the packets that come from 1.2.3.4 on port 123 (the port that the firewall allows) to 5.6.7.8 on port 80, essentially **redirecting the requests you make from your computer at work (on port 123) to your machine at home (on port 123), to the Github webpage (on port 80)**.

Now, while being at work, you can navigate to *GitHub* by going to `https://a.b.c.d:123`.

## 2 - SSH Tunneling

Like the name says, *SSH tunneling* is creating encrypted (<u>secure</u>) "tunnels" using the SSH protocol. This, for instance, can be used to **encrypt traffic that goes over an insecure network**, therefore making it secure.

### 2.1 - Local port forwarding
Let's use the same example as with port forwarding.

Instead of installing *rinetd*, you do the following things:
1. You set up an ***SSH server*** on your local linux machine, to <u>listen on port 123</u> (the one that is allowed by the firewall between you and your computer at work). This can be done editting the configuration file `/etc/ssh/sshd_config`.
1. On your <u>work computer</u>, you run this command: `ssh a.b.c.d -p 123 -L 8080:5.6.7.8:80`

What that command does is:
* Connect to your home machine (*a.b.c.d*) via *SSH* (with or without ssh-key, it doesn't matter).
* <u>Open the port 8080 on your work computer</u>.
* **Redirect the requests to port 8080 on your work computer through SSH to your home machine, and then though port 80 to the GitHub (*5.6.7.8*) webpage**, and the same thing but backwards with the webpage responses.

This, <u>appart from being more practical than the previous example</u> (because it is more likely to have an SSH server than having *rinetd* installed on your system), **it encrypts all the traffic between your work and your home computer**, thus making it safe, even if the network is insecure. 

Now, while being at work, you can navigate to *GitHub* by going to `https://127.0.0.1:8080`.

### 2.2 - Remote port forwarding
We'll change up a little bit the previous example to explain remote port forwarding.

Let's say that there is an *FTP* server (running on port 21) in the corporate network, and a <u>coworker asks me if I can help him with a bug in the *FTP* server that he's having trouble solving</u>.
Right now I can't help him, because **the firewall blocks all connections that don't go through port 123**, and the FTP server is listening on port 21.

What I can do is this:
1. As the previous example, set up an ***SSH server*** on your home machine that <u>will listen on port 123</u>.
1. Tell my friend to run the following commmand: `ssh a.b.c.d -p 123 -R 21:127.0.0.1:21`

What this is going to do is create an ***SSH* tunnel** that will communicate a remote port in my home machine (`21:`) with the local port (`:21`) on the corporate server (`127.0.0.1`).

Now I can access the corporate *FTP* server running on port 21 from my machine with this command: `ftp 127.0.0.1`