---
title: "install tensor to  ubuntu "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# Nvidia Graphic Driver 설치
nvidia-smi 

설치가능 버젼 확인     
- ubuntu-drivers devices 

```
younleakim@younleakim-400TEA-400SEA:~$ ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001F82sv0000144Dsd000080B3bc03sc00i00
vendor   : NVIDIA Corporation
model    : TU117 [GeForce GTX 1650]
driver   : nvidia-driver-470-server - distro non-free
driver   : nvidia-driver-545 - distro non-free
driver   : nvidia-driver-535-server - distro non-free
driver   : nvidia-driver-535 - distro non-free recommended    <<<<<<<<<<<<<<<
driver   : nvidia-driver-470 - distro non-free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-535-server-open - distro non-free
driver   : nvidia-driver-545-open - distro non-free
driver   : nvidia-driver-535-open - distro non-free
driver   : nvidia-driver-418-server - distro non-free
driver   : xserver-xorg-video-nouveau - distro free builtin
```

install
sudo apt-get install nvidia-driver-535




ref : [blog1](https://sanghyunpark01.github.io/ubuntu/tips/Ubuntu_GDriver/)
      [blog2](https://sanghyunpark01.github.io/ubuntu/tips/Uubntu_Cuda/)
