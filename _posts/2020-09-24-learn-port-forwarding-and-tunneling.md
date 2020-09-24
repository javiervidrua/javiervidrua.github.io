---
layout: post
title:  "Learn port forwarding and tunneling"
date:   2020-09-24 00:00:00 +0200
categories: jekyll update
---

*Port redirection*, *tunneling* and *traffic encapsulation* are a **must-know** if you are a *sysadmin* or *pentester*, as <u>they provide the ability to change the direction of the network traffic, and encapsulate a protocol inside a different one</u>, allowing the operator to, for example, **access restricted networkt, bypass firewall protections and much more**.

Let's start talking about *port forwarding*.

## Port forwarding (redirection)
Let's explain *port forwarding* using a simple example:

Supose that you are at work on your computer, and you want to navigate to *Github*, but you can't because <u>there is a firewall that only allows communications on port 53 (DNS)</u>. You try to go to *Github* but the requests get dropped by the firewall and the page doesn't load.

So what do you do?

1. On your local machine, you install the tool **rinetd** with the command `apt install rinetd`
1. You open */etc/rinetd.conf* with your favorite editor.
1. Suposing that your work IP is *1.2.3.4*, the IP of the *Github* webpage is *5.6.7.8* and the IP of your home machine is *a.b.c.d*, you add the following line:
```
#bindadress    bindport    connectaddress  connectport
 1.2.3.4       53          5.6.7.8         80
```
That line tells your home system to redirect (<i>forward</i>) the packets that come from 1.2.3.4 on port 53 (the port that the firewall allows) to 5.6.7.8 on port 80, essentially **redirecting the requests you make from your computer at work to your machine at home, to the Github webpage**.

Now, while being at work, you can navigate to *GitHub* by going to `https://a.b.c.d:53`