---
title: "Nvidia jetson nano - setting and vnc"
excerpt_separator: "<!--more-->"
date: 2020-03-10 05:17:55 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

ref : <https://raspberry-valley.azurewebsites.net/NVIDIA-Jetson-Nano/>

# NVIDIA Jetson Nano

We have started digging deeper into other devices, not only Raspberry Pi. With a great interest in deep learning and AI, we decided to try out the Jetson Nano and learn. This page is an NVIDIA Jetson Nano home page for Raspberry Valley.

---

**NOTICE**: This page is under construction. We are investigating the device and it's usage. Work in progress. Please come back regularly, you'll find updates as we try out things.

---

![jetson nano](https://raspberry-valley.azurewebsites.net/img/NVIDIA-Jetson-Nano-01.jpg)

## Installation

Follow the official [Getting Started Guide](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit) to setup your Jetson Nano, add your OS (Ubuntu) and install base libraries. The guide is really good and no more comments are needed here. You can plugin the device now and run the system.

Further, we install some basic utilities. This setup is purely optional, and depends on your preferences. Here is the list so far:

```
sudo apt install nano
```

### Purging Libre Office

This step is rather optional, but if you want to get some extra space and do not intend to use the Nano as a desktop computer, we suggest some purging.

```
# Purge libre office (gain approximately 250 MB extra)sudo apt remove --purge libreoffice* -ysudo apt-get clean -ysudo apt autoremove -ysudo apt-get update
```

### Samba

We make it a custom to install [Samba](https://raspberry-valley.azurewebsites.net/Samba/) for better sharing on Windows networks. Our guide will give you more insight.

### Python Setup

Our [Python](https://raspberry-valley.azurewebsites.net/Python/) guide gives you some more details, but we wish to install some pre-requisites for Python 3 development, and virtual environments.

```
# install pre-requisites for Python 3 developmentsudo apt install -y python3-pip# below are basic development pre-requisites.# The latest Jetson Nano image already contain 'build-essential' and 'python3-dev' sudo apt install build-essential libssl-dev libffi-dev python3-dev## Time to setup virtual environments now, based on your personal preferences...
```

We recommend to setup virtual environments as well. See our [Python](https://raspberry-valley.azurewebsites.net/Python/) page for more details.

### Docker

Your Jetson Nano already contains Docker. We have covered Docker in our [Docker on Raspberry Pi](https://raspberry-valley.azurewebsites.net/Docker-on-Raspberry-Pi/) article and have outlined the setup (including Jetson Nano details). All you need to do is configuration (enable running without sudo) and probably update Docker. Here is how:

```
# run docker without sudosudo usermod -aG docker your_username# update Dockersudo apt-get --only-upgrade install docker.io
```

Restart - that's all there is to it!

### Wifi Setup

The Jetson Nano is picky when it comes to a good Wifi connection. The recommended solution is to use an **Intel Dual Band Wireless-Ac 8265**. You can also get away with some selected Wifi dongles. Here is an example of a setup with an [Edimax Device](https://desertbot.io/blog/jetson-nano-usb-headless-wifi-setup-edimax-ew-7811un). We got away also with a Realtek dongle.

Setting up Wifi is done through the desktop connections, as you would on any Ubuntu device. Check the guide above or search for more. There's plenty available.

Here is a bunch of commands which might come in useful for checking your Wifi network via the terminal.

- **lsusb** - List info on your USB wifi adapter
- **nmcli device wifi list** - List info on your wifi signal
- **nmcli connection show** - List connections
- **iwconfig** - List info your your wifi connections
- **ifconfig wlan0** - List info about the wlan0 wifi connection

### Remote Desktop - XRDP

This is our preferred method of accessing the Jetson Nano (or Raspberry Pi) from a Windows computer, via RDP. We have already covered XRDP in our article [Remote Access to Raspberry Pi](https://raspberry-valley.azurewebsites.net/Remote-Access-to-Raspberry-Pi/), and in our Troubleshooting guide: [When Things Go Wrong](https://raspberry-valley.azurewebsites.net/When-Things-Go-Wrong/). In principle, the approach is the same.

You don't need to read up on the Raspberry Pi articles mentioned above. To access your Jetson Nano remotely, simply do the following:

- On the Jetson Nano terminal, install XRDP:

```
sudo apt install xrdp
```

- Reboot the Jetson Nano
- Open RDP on your Windows Desktop (aka Remote Desktop Connection), type in the IP address or Hostname of your Jetson Nano and after entering credentials, you have achieved remote access. Done!

---

Please expect a possible bug. For some, the XRDP will work great, but others will be asked to re-enter the password once you login; if you see *Authentication Required to Crealte Managed Color Device*, then read on to fix this bug.

This is a rather infamous bug and solutions found around the internet usually spawn other problems. What works for us is a fix found in this great post: [xRDP – How to Fix the Infamous system crash popups in Ubuntu 18.04 (and previous versions)](http://c-nergy.be/blog/?p=12043). Check it out, or simply follow the solution we have extracted below.

The solution is to create a proper PolKit file, which tells the system that any known user can perform actions defined in the \*colord.xml policy file. You will need superuser access to do so.

- Create a file **45-allow-colord.pkla** with the following content:

```
[Allow Colord all Users]Identity=unix-user:*Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profileResultAny=noResultInactive=noResultActive=yes
```

- Copy the file to the folder below: it has restricted access

```
 /etc/polkit-1/localauthority/50-local.d/
```

- To validate all works great, check the `/var/crash` directory for crash logs. It is a good idea to remove the crash logs first - `sudo rm /var/crash/*`

---

### Remote Desktop - VNC

If you try to share your desktop from the Jetson Nano, the process fails; running *Desktop Sharing* fails with an error. This recipe shows you how to fix the issue, and connect remotely via VNCViewer. Having said that, we still prefer connecting via RDP (XRDP), which we find faster and more convenient.

To fix the issue, follow the steps below:

- First, we will edit the `org.gnome.Vino` schema, as it has a missing parameter called `enabled`. Open the schema:

```
sudo nano /usr/share/glib-2.0/schemas/org.gnome.Vino.gschema.xml
```

Add the missing key (any location will do):

```
<key name='enabled' type='b'>   <summary>Enable remote access to the desktop</summary>   <description>   If true, allows remote access to the desktop via the RFB   protocol. Users on remote machines may then connect to the   desktop using a VNC viewer.   </description>   <default>false</default></key>
```

- Compile the new Gnome schema configuration:

```
sudo glib-compile-schemas /usr/share/glib-2.0/schemas
```

- Update the *Desktop Sharing* settings. Your application should work now. Launch it from your Jetson Nano desktop.

  - Enable **Allow other users to view your desktop**
  - Enable the subsection **Allow other users to control your desktop**
  - Turn off the feature **You must confirm each access to this machine**
  - Setup the password in the section **Require the user to enter this password**
  - Close the *Desktop Sharing* settings. You are done here
- Setup the VNC server to autostart

  - Open the *Startup Application Preferences* panel
  - Add your VNC (Vino) entry: Add a name ('Vino'), a description (any text which makes sense to you) and the command: `/usr/lib/vino/vino-server`. Close the app
- Disable encryption for the VNC server: unfortunately, at the time of this writing, we have to live without it. In the terminal, type the following:

```
gsettings set org.gnome.Vino require-encryption falsegsettings set org.gnome.Vino prompt-enabled false
```

- reboot
- after reboot, you can use any **VNCViewer** from your laptop to connect to the shared screen. Needless to say, the speed is what it is and if you can, use the previous, XRDP recipe.

**Note**: as we are crippling the security setup for remote connections via VNC, we have to be aware and enable the VNC feature (this whole section) only if needed.

## Tips

This section provides helpful tips for working with your Jetson Nano.

### List Cameras

You'll be working with cameras a lot. Either by plugging in a Raspberry Pi camera, or a USB camera of your choice. Or several. Many examples have a camera name as a parameter, so it's a good idea to know where to find the names.

To list cameras connected to your device, run the following:

```
# Run installation of v4l-utils, but the base image contains it already. Just in casesudo apt install v4l-utils# List connected camerasv4l2-ctl --list-devices
```

## Troubleshooting

This section provides gotchas we've encountered when using the device.

### matplotlib fails to install

When installing matplotlib for python3, you will find out that the installation fails. This is a common problem and the solution is straightforward, not only on Jetson Nano, but on Ubuntu as well. Just install the following:

```
sudo apt-get install python-dev libfreetype6-dev
```

## Links

- [NVIDIA Deep Learning and AI portal](https://www.nvidia.com/en-gb/deep-learning-ai/)
- [Jetson Nano Developer Kit](https://developer.nvidia.com/embedded/jetson-nano-developer-kit)
- [Jetson Nano Wiki](https://elinux.org/Jetson_Nano)
- [jetson-inference](https://github.com/dusty-nv/jetson-inference) - the base tutorials and code to get started fast (Github)
- [jetbot](https://github.com/NVIDIA-AI-IOT/jetbot) - a robot aware of it's surrounding, using the Jetson nano (github). This is the NVIDIA robot showcase
- [jetson-presentations](https://github.com/dusty-nv/jetson-presentations) - various presentations on the Jetson Nano. Check out the 'Hello AI World' slides if you're getting started with your board
- [Connecting Edimax USB Wifi](https://desertbot.io/blog/jetson-nano-usb-headless-wifi-setup-edimax-ew-7811un) - the device doesn't come with Wifi/Bluetooth. You are recommended to use an intel chip, but can connect others as well
- [Getting Started with the NVIDIA Jetson Nano Developer Kit](https://blog.hackster.io/getting-started-with-the-nvidia-jetson-nano-developer-kit-43aa7c298797) - a great overview of the Jetson Nano. We borrowed a few ideas from Alasdair
- [Getting Started with the NVIDIA Jetson Nano](https://www.pyimagesearch.com/2019/05/06/getting-started-with-the-nvidia-jetson-nano/) - another great getting started tutorial by Adrian
- [Robot Projects](https://desertbot.io/) - A nice blog site with many recipes for (not only) Jetson Nano
- [Nano Playground](https://github.com/uutzinger/nano_playground) - various scripts to install stuff on your Jetson Nano. Notice you can also install [ROS](https://raspberry-valley.azurewebsites.net/ROS/) from one of the scripts - we are big fans! (you can find a similar ROS Installation Script on [JetsonHacksNano](https://github.com/JetsonHacksNano/installROS) with other interesting Jetson Nano stuff)
- [JetsonHacks](https://www.jetsonhacks.com/) - Find guides on 'Developing for NVIDIA Jetson'. Here are the related [jetsonhacksnano](https://github.com/JetsonHacksNano) github repositories and [Jetson Hacks Youtube Channel](https://www.youtube.com/channel/UCQs0lwV6E4p7LQaGJ6fgy5Q)
- [Jetson GPIO](https://github.com/NVIDIA/jetson-gpio) - setup GPIO on Jetson Nano (by NVIDIA)
- [3D Printed Case for Jetson Nano](https://www.thingiverse.com/thing:3603594) - the one we use :)
- [Open data cam](https://github.com/opendatacam/opendatacam) - a nice application for monitoring (and counting) traffic: cars, pedestrians, bikes ...
- [Face Recognition Doorbell Project](https://medium.com/@ageitgey/build-a-hardware-based-face-recognition-system-for-150-with-the-nvidia-jetson-nano-and-python-a25cb8c891fd)