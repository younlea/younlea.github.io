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

## trouble shooting
간혹 안되는 경우가 있는데 x11vnc.service 파일에 오타가 있을수 있으니 확인해 보고   
-auss guess가 제대로 못찾는 경우가 있어서    
.Xauthority 파일을 만들고 service file에서 -auth ~/.Xauthority. 를 넣어주면 동작이 잘됨.    

혹시 status가 문제가 있다면 jurnalctl -u x11vnc.service로 자세한 로그를 확인해 보면 됩니다.     

