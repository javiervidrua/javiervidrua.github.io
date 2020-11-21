---
layout: post
title:  "Managing WSL distributions"
date:   2020-10-18 00:00:00 +0200
categories: jekyll update
---
You can manage your *WSL* distributions from the Powershell, with a just a few simple commands.

## Listing the distributions installed
To list the distributions that are currently installed, run:
``` powershell
wsl -l -v
```
This will output something like this:
``` powershell
PS C:\Users\x> wsl -l -v
  NAME                 STATE           VERSION
* Distribution-name    Stopped         2
```

## Exporting distributions
If you want to export a distribution, very much like creating a snapshot if you were using *VMWare*, you can run:
``` powershell
wsl --export <distribution-name> <filename.tar>
```
This will dump the distribution named "distribution-name" to a file called "filename.tar".

## Importing distributions
Now, if you want to import the previously exported distribution, you can run:
``` powershell
wsl --import <new-distribution-name> <distribution-installation-path> <filename.tar>
```
## Launching distributions
If you want to launch a distro from *Powershell*, you can run:
``` powershell
wsl -d <distribution-name>
```
## Changing the version of a distribution
If you want to change the version of *WSL* that a distribution uses run:
``` powershell
wsl --set-version <distribution-name> <version>
```

## Setting the default distribution
You can set the default distribution to be used when typing `wsl` in the *Powershell* by running:
``` powershell
wsl --setdefault <distribution-name>
```

## Unregistering (removing) distributions
To unregister a distribution you can run:
``` powershell
wsl --unregister <distribution-name>
```

## Example
``` powershell
PS C:\Users\x\Desktop> wsl -l -v
  NAME      STATE           VERSION
* Debian    Stopped         2
PS C:\Users\x\Desktop> wsl --export debian debian-ready-to-go.tar
PS C:\Users\x\Desktop> wsl --import debian-testing .\debian-testing .\debian-ready-to-go.tar
PS C:\Users\x\Desktop> wsl --setdefault debian-testing
PS C:\Users\x\Desktop> wsl -l -v
  NAME              STATE           VERSION
* Debian-testing    Stopped         2
  Debian            Stopped         2
PS C:\Users\x\Desktop> wsl --unregister debian-testing
Removing from the registry...
PS C:\Users\x\Desktop>
```