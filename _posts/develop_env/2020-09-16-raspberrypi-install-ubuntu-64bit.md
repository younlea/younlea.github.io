---
title: "raspberrypi - install ubuntu 64bit"
excerpt_separator: "<!--more-->"
date: 2020-09-16 04:36:20 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Try to intalll ubuntu 64bit version on raspberrypi4 8GB version .

![image](/assets/images/posts/6698873/c0028014_5f6116f704b52.png)

connect LAN

[guide\_link1](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#3-wifi-or-ethernet)

[guide\_link2](https://blog.naver.com/roboholic84/221701573539)

ID/PW : ubuntu/ubuntu

sudo apt-get update

sudo apt-get install xinit

startx -- << cannot startx...

[guide\_link1](https://ubuntu.com/download/raspberry-pi) << change install binary and method

![image](/assets/images/posts/6698873/c0028014_5f6118e318c1c.png)

very kindly guide -  [링크](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.5&architecture=arm64+raspi4)

After download image, go to [this link](https://ubuntu.com/tutorials/create-an-ubuntu-image-for-a-raspberry-pi-on-macos#2-on-your-macos-machine)(this is install guide using cmd line on mac)

OUNLEA@gimyunlaeuiMBP Desktop % sudo sh -c 'gunzip -c ubuntu-18.04.5-preinstalled-server-arm64+raspi4.img.xz | sudo dd of=/dev/disk3 bs=32m'

I wait some time.... mmmh... some time..

ID/PW : ubuntu/ubuntu

$sudo apt update

$sudo apt upgrade

$sudo apt install xubuntu-desktop

$sudo reboot