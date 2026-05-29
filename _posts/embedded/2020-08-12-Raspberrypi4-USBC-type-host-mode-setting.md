---
title: "Raspberrypi4 USB-C type host mode setting"
excerpt_separator: "<!--more-->"
date: 2020-08-12 05:36:37 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

I want use USB hub on USB type C port.

The result is success. ^^

Now I can use host mode and power also can use through hub to raspberrypi.

![image](/assets/images/posts/6686498/c0028014_5f3300fe13df0.png)

**$sudo modprobe -r dwc2 && sudo dtoverlay dwc2 dr\_mode=host && sudo modprobe dwc2**

![image](/assets/images/posts/6686498/c0028014_5f32fe0cd6134.png)

one more thing.

If you want setting on booting time. you can add to /boot/config.txt

```
dtoverlay=dwc2,dr_mode=host
```

good luck~~

ref : <https://www.raspberrypi.org/forums/viewtopic.php?t=246348>

Regardless of whether or not you have done this, you can switch into device mode in the shell as root:

Code: [Select all](https://www.raspberrypi.org/forums/viewtopic.php?t=246348#)

```
modprobe -r dwc2 && dtoverlay dwc2 dr_mode=peripheral && modprobe dwc2
```

At this point, you can load the gadget of your choice. When you are done, remove, the gadget driver, and you can switch back to host mode:

Code: [Select all](https://www.raspberrypi.org/forums/viewtopic.php?t=246348#)

```
modprobe -r dwc2 && dtoverlay dwc2 dr_mode=host && modprobe dwc2
```