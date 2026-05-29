---
title: "USB0 interface 자동으로 고정 IP로 올라오게 하는 법."
excerpt_separator: "<!--more-->"
date: 2012-07-06 10:51:12 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

/etc/network/interfaces

auto lo

iface lo inet loopback

auto usb0

iface usb0 inet static

address xxx.xxx.xxx.xxx

netmask 255.255.255.0

\