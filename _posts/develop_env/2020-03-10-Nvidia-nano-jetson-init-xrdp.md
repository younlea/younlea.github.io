---
title: "Nvidia nano jetson - init xrdp"
excerpt_separator: "<!--more-->"
date: 2020-03-10 05:23:41 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

움 여기서는 vnc 말고 xrdp 셋팅하는걸 참고 하면 좋아 보인다. ^^;

ref : <https://www.hackster.io/news/getting-started-with-the-nvidia-jetson-nano-developer-kit-43aa7c298797>

Enabling Remote Desktop

Unfortunately the VNC Server will only be running when a user is logged into Jetson Nano on console. If you logout, the server will be stopped. You can’t just unplug your [monitor](https://amzn.to/2UbcSmY), [keyboard](https://amzn.to/2KHelCj), or [mouse](https://amzn.to/2KHdYHV) and run the board in headless mode.

If you want to do that, the easiest way is probably going to be running an [RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) server called `xrdp`. Installation is a lot simpler than setting up VNC.

```
$ sudo apt-get install xrdp
```

After installation has completed, you should go ahead and reboot the Jetson Nano board. Once the reboot has completed you can check installation of `xrdp`was successful by using the command `nmap` from your laptop.

```
$ nmap jetson\
Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-13 01:39 BST\
Nmap scan report for jetson (192.168.1.118)\
```

As you can see since we’re not logged in, our VNC server has been shutdown, however the RDP server is running despite us currently being at the login screen on the physical machine.

While RDP is a proprietary protocol, Microsoft do provide viewers for most platforms for free, including the Mac, which is available in [the Mac App Store](https://itunes.apple.com/gb/app/microsoft-remote-desktop-10/id1295203466?mt=12).

You should go ahead and install it.

![You can install Microsoft Remote Desktop from the Mac App Store](https://hackster.imgix.net/uploads/attachments/981194/1_SQCpSAjHLB8CkSOQm59RlQ.png?auto=compress%2Cformat&w=740&h=555&fit=max)

You can install Microsoft Remote Desktop from the Mac App Store

Open Microsoft Remote Desktop and click on “Add Desktop.”

![](https://hackster.imgix.net/uploads/attachments/981202/1_Kvd_wMTEKpzknythBej6-g.png?auto=compress%2Cformat&w=740&h=555&fit=max)

![](https://hackster.imgix.net/uploads/attachments/981208/1_8n1I2orsgEXBkAEsLF4pPw.png?auto=compress%2Cformat&w=740&h=555&fit=max)

![Setting up the RDP client to connect to the Jetson Nano.](https://hackster.imgix.net/uploads/attachments/981216/1_LuWZWZ1ORfU8kNUMuQxJsg.png?auto=compress%2Cformat&w=740&h=555&fit=max)

Setting up the RDP client to connect to the Jetson Nano.

Once you’ve configured the settings to your liking, you might want to turn off “Start session in full screen” for instance and set a reasonable resolution for the resulting window’ed desktop. Click “Save” and then open the RDP desktop by clicking on the “Jetson Nano” desktop icon.

When you’re connect to the board using RDP the desktop will look somewhat different. That’s because you’ll be seeing a standard Ubuntu desktop, [running Gnome](https://help.ubuntu.com/stable/ubuntu-help/shell-introduction.html.en), rather than the older Unity style desktop that is the default on L4T.

![](https://hackster.imgix.net/uploads/attachments/981221/1_zbA_ArNaR8O0OA_gthFnkw.png?auto=compress%2Cformat&w=740&h=555&fit=max)

![Connecting to a slightly different looking desktop.](https://hackster.imgix.net/uploads/attachments/981226/1_cvW0r8KprfI-zqvgTP4F3Q.png?auto=compress%2Cformat&w=740&h=555&fit=max)

Connecting to a slightly different looking desktop.

> ⚠️Warning You can not be logged in at the physical desktop and open an RDP desktop, conversely if you have an RDP desktop already open you won’t be able to login to the physical desktop. If you have an RDP desktop open and attempt to connect to the Jetson Nano using VNC you will be connected to the RDP session.

If you’re used to VNC when using Remote Desktop you should bear in mind the differences. You’re not viewing the existing Jetson Nano desktop, you’re creating another. That virtual desktop will be persistent until you logout as you would if you were sitting in front of a physical keyboard. If you just close the RDP window and walk away, that doesn’t close the desktop, or log you out. The next time you connect to the RDP server on your Jetson Nano the RDP desktop will look the same.