---
layout: post
title:  "WSL2 and VirtualBox/VMware at the same time"
date:   2020-11-14 00:00:00 +0200
categories: jekyll update
---
If after installing *WSL2* on a system that had *VMware* or *VirtualBox* previously installed, you get an error that says something like this one:

![CredentialGuard](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.stack.imgur.com%2FKxtXR.png&f=1&nofb=1 "CredentialGuard")

You just found that your system can't have *WSL2* and *VMware* or *VirtualBox* running at the same time, because *WSL2* uses the Hyper-V architecture for its virtualization, and some third party tools such as the mentioned before cannot work when Hyper-V is in use. 

At this point you might be thinking "**Wait, so you're telling me that I have to choose one or the other?!?!**"

The answer is "**No, but you'll have to choose one of two options**"

The options you have are those:
* Manually change which virtual machine plattform will be possible to use at a certain time.

  Open the Powershell as Administrator and run:

  If you want to enable *VMware*:
  ```powershell
  bcdedit /set hypervisorlaunchtype off
  ```

  If you want to enable *Hyper-V* and *WSL2*:
  ```powershell
  bcdedit /set hypervisorlaunchtype auto
  ```
<!--
  This can easily be done:
  1. Create scripts to easily choose what virtualization plattform to use
     
     I have a GitHub project that does this automatically, you only have to run the script with Administration rights.
  1. Once you have done the previous step, now you can run the following commands directly from the Powershell:
     * runvmware: To enable VMware or *VirtualBox*.
     * runhyperv: To enable Hyper-V and *WSL2*.-->

* Update *VMware* or *VirtualBox*.

  Many people have reached out to the developers since *WSL2* became available, and *Oracle* and *VMware* have developed their solutions for this problem.

  In case of *VMware*, they released a new version that solves this problem. You can get more info about that [**here**](https://blogs.vmware.com/workstation/2020/01/vmware-workstation-tech-preview-20h1.html)

  For more info about *VirtualBox*, you can check the [**VirtualBox issue discussions in the WSL repo on GitHub**](https://github.com/MicrosoftDocs/WSL/issues?q=is%3Aissue+virtualbox+sort%3Acomments-desc) and the [**VirtualBox changelog**](https://www.virtualbox.org/wiki/Changelog-6.0)