---
layout: post
title:  "Kali Linux on WSL with GUI"
date:   2020-10-26 00:00:00 +0200
categories: jekyll update
---
## Install *Kali linux*

You can install Kali-linux from the Microsoft Store with a few clicks, or alternatively, you can install it [**from the Powershell**](https://javiervidrua.github.io/jekyll/update/2020/10/17/managing-wsl-distributions.html).

## Install *kali-win-kex*

Now launch *Kali* and execute these commands:
```bash
sudo apt update && sudo apt install kali-win-kex -y
```

The last two steps can easily be done as shown in this image:

![install](/images/kali-win-kex-install.png)

## Run *kex*

All done! Now you can run (from *Kali*) the following command:
```bash
kex -s
```

First you will get something like this:

![start](/images/kali-win-kex-start.png)

Then this:

![vnc](/images/kali-win-kex-vnc.png)

And when you enter your password the desktop will appear!

![desktop](/images/kali-win-kex-desktop.png)

## Use Kali as normal

Now you can open graphical tools like *Burpsuite*, *Bettercap* or *Cherrytree*.