---
layout: post
title:  "Port forwarding and tunneling"
date:   2020-09-24 00:00:00 +0200
categories: jekyll update
---

*Port redirection*, *tunneling* and *traffic encapsulation* are a **must-know** if you are a *sysadmin* or *pentester*, as they provide the ability to change the direction of the network traffic, and encapsulate a protocol inside a different one, allowing the operator to, for example, **access restricted networkt, bypass firewall protections and much more**.

Let's start talking about *port forwarding*.

## 1 - Port forwarding/redirection
Let's explain *port forwarding* using a simple example:

Suppose that you are at work on your computer, and you want to navigate to *Github*, but you can't because **there is a firewall that only allows communications on port 123 (NTP)***. You try to go to *Github* but the requests get dropped by the firewall and the page doesn't load.

So what do you do?

1. On your local linux machine, you install the tool ***rinetd*** with the command `apt install rinetd`.
1. You open */etc/rinetd.conf* with your favorite editor.
1. Suposing that your work IP is *1.2.3.4*, the IP of the *Github* webpage is *5.6.7.8* and the IP of your home machine is *a.b.c.d*, you add the following line:
```
#bindadress    bindport    connectaddress  connectport
 1.2.3.4       123         5.6.7.8         80
```
That line tells your home system to redirect (<i>forward</i>) the packets that come from 1.2.3.4 on port 123 (the port that the firewall allows) to 5.6.7.8 on port 80, essentially **redirecting the requests you make from your computer at work (on port 123) to your machine at home (on port 123), to the Github webpage (on port 80)**.

Now, while being at work, you can navigate to *GitHub* by going to `https://a.b.c.d:123`.

## 2 - SSH Tunneling

Like the name says, *SSH tunneling* is creating encrypted (secure) "tunnels" using the SSH protocol. This, for instance, can be used to **encrypt traffic that goes over an insecure network**, therefore making it secure.

### 2.1 - Local port forwarding
Let's use the same example as with port forwarding.

Instead of installing *rinetd*, you do the following things:
1. You set up an ***SSH server*** on your local linux machine, to listen on port 123 (the one that is allowed by the firewall between you and your computer at work). This can be done editting the configuration file `/etc/ssh/sshd_config`.
1. On your work computer, you run this command: `ssh user@a.b.c.d -p 123 -L 8080:5.6.7.8:80`

What that command does is:
* Connect to your home machine (*a.b.c.d*) via *SSH* (with or without ssh-key, it doesn't matter).
* Open the port 8080 on your work computer.
* **Redirect the requests to the port 8080 on your work computer through SSH to your home machine, and then through the port 80 to the GitHub (*5.6.7.8*) webpage**, and the same thing but backwards with the webpage responses.

This, apart from being more practical than the previous example (because it is more likely to have an SSH server than having *rinetd* installed on your system), **it encrypts all the traffic between your work and your home computer**, thus making it safe, even if the network is insecure. 

Now, while being at work, you can navigate to *GitHub* by going to `https://127.0.0.1:8080`.

### 2.2 - Remote port forwarding
We'll change up a little bit the previous example to explain remote port forwarding.

Let's say that there is an *FTP* server (running on port 21) in the corporate network, and a coworker asks me if I can help him with a bug in the *FTP* server that he's having trouble solving.
Right now I can't help him, because **the firewall blocks all connections that don't go through port 123**, and the FTP server is listening on port 21.

What I can do is this:
1. As the previous example, set up an ***SSH server*** on your home machine that will listen on port 123.
1. Tell my coworker to run the following commmand: `ssh user@a.b.c.d -p 123 -R 21:127.0.0.1:21`

What this is going to do is create an ***SSH* tunnel** that will communicate a remote port in my home machine (`21:`) with the local port (`:21`) on the corporate server (`127.0.0.1`).

Now I can access the corporate *FTP* server running on port 21 from my machine with this command: `ftp 127.0.0.1`

### 2.3 - Dynamic port forwarding
We have now seen local and remote port forwarding, and those are really useful features of the SSH protocol, **but what happens if you need to open a range of ports?** In that situation they aren't very practical, because you will need to be running as many commands as ports you'd like to forward. 

For those situations there's yet another feature, that feature is **dynamic port forwarding**.

To be able to use this feature, you will need to have a proxy server too. One that I like using is ***proxychains***. You can install it by running the command: `apt install proxychains`.

Note: You can change the behaviour of *proxychains* modifying the file `/etc/proxychains.conf`, by default, it listens on port 9050, the *Tor* port.

Now that we're set up, let's use an example to explain dynamic port forwarding.

Let's suppose that the coworker of the last example now calls us because he needs help diagnosing two servers that are not working properly. **Each server has various services that we'll need to check**, so trying to do this kind of job using remote port forwarding would take a long time and you would have to run a lot of commands. So instead of that, you use dynamic port forwarding:

The first thing to do is to tell my coworker to run this command:

`ssh user@a.b.c.d -p 123 -f -N -D 9050`

Notes:
* "`-f`" makes the session go to background. Useful for port forwarding.
* "`-N`" tells the *SSH* client that we won't be sending commands. Useful for port forwarding.
* "`-D <port>`" tells the *SSH* client that we'll be doing dynamic port forwarding on the specified port.
* "`-p 123`" is used because the firewall only allows that port and we have the *SSH server* listening on that port too.

Now **I'm connected to the corporate network via my local port `9050`**.

For example, if the IP of the first server is *10.10.5.11*, I can ssh in preceding the ssh command with "proxychains": `proxychains ssh admin@10.10.5.11`

And, for example, if you wanted to check the *http* service of that server, you can do that by runnning the command: `proxychains firefox`. **It will open firefox through *proxychains***, and now you could navigate to `http://10.10.5.11/`, because now you have access to everything that is inside of the internal, corporate network.

You could even run an *nmap* scan, but I will warn you that *nmap* through *proxychains* takes a really long time to finish.

If you want **a pentesting example case of dynamic port forwarding**, check [**this**](https://netsec.ws/?p=278).
