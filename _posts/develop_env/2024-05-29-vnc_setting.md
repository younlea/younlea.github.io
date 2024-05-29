---
title: "vnc server setting "
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - write here

toc : true
toc_sticky : true
---

# vnc server setting 환경
1. ubuntu 22.04

# setting method
```
sudo apt update
sudo apt install lightdm
sudo reboot

sudo apt install x11vnc
sudo vim /lib/systemd/system/x11vnc.service
------------------------
[Unit]
Description=x11vnc service
After=display-manager.service network.target syslog.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -forever -display :0 -auth guess -passwd my-password
ExecStop=/usr/bin/killall x11vnc
Restart=on-failure

[Install]
WantedBy=multi-user.target
------------------------

sudo systemctl daemon-reload
sudo systemctl enable x11vnc.service
sudo systemctl start x11vnc.service
sudo systemctl status x11vnc.service

x11vnc -usepw
sudo ufw allow 5900/tcp
sudo ufw reload

```

ref : [https://linuxopsys.com/setup-x11vnc-on-ubuntu](https://linuxopsys.com/setup-x11vnc-on-ubuntu)    
