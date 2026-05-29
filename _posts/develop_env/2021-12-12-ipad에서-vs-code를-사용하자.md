---
title: "ipad에서 vs code를 사용하자~"
excerpt_separator: "<!--more-->"
date: 2021-12-12 09:10:55 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

ipad에서 vscode 사용하는 방법~~~

준비물 : vs code 설치 PC + DDNS 되는 AP + ipad

<https://github.com/cdr/code-server>

필자는 우분투라. 아래 방법으로 인스톨을 진행했습니다.

[https://coder.com/docs/code-server/latest/install#debian-ubuntu](http://https://coder.com/docs/code-server/latest/install#debian-ubuntu)

그런데.. 좀 찾아 보니.. 몇가지만 하면 되서.. 다시 정리해 본다.

일단 **우분투 서버에서 설정..**

1. 설치

$curl -fsSL https://code-server.dev/install.sh | sh -s -- --dry-run

$curl -fsSL https://code-server.dev/install.sh | sh

```
younlea@younlea-System-Product-Name:~$ curl -fsSL https://code-server.dev/install.sh | sh -s -- --dry-run
Ubuntu 21.10
Installing v3.12.0 of the amd64 deb package from GitHub.
 
+ mkdir -p ~/.cache/code-server
+ curl -#fL -o ~/.cache/code-server/code-server_3.12.0_amd64.deb.incomplete -C - https://github.com/cdr/code-server/releases/download/v3.12.0/code-server_3.12.0_amd64.deb
+ mv ~/.cache/code-server/code-server_3.12.0_amd64.deb.incomplete ~/.cache/code-server/code-server_3.12.0_amd64.deb
+ sudo dpkg -i ~/.cache/code-server/code-server_3.12.0_amd64.deb
 
deb package has been installed.
 
To have systemd start code-server now and restart on boot:
  sudo systemctl enable --now code-server@$USER
Or, if you don't want/need a background service you can run:
  code-server
younlea@younlea-System-Product-Name:~$ curl -fsSL https://code-server.dev/install.sh | sh
Ubuntu 21.10
Installing v3.12.0 of the amd64 deb package from GitHub.
 
+ mkdir -p ~/.cache/code-server
+ curl -#fL -o ~/.cache/code-server/code-server_3.12.0_amd64.deb.incomplete -C - https://github.com/cdr/code-server/releases/download/v3.12.0/code-server_3.12.0_amd64.deb
######################################################################## 100.0%##O######################################################################## 100.0%
+ mv ~/.cache/code-server/code-server_3.12.0_amd64.deb.incomplete ~/.cache/code-server/code-server_3.12.0_amd64.deb
+ sudo dpkg -i ~/.cache/code-server/code-server_3.12.0_amd64.deb
Selecting previously unselected package code-server.
(Reading database ... 189941 files and directories currently installed.)
Preparing to unpack .../code-server_3.12.0_amd64.deb ...
Unpacking code-server (3.12.0) ...
Setting up code-server (3.12.0) ...
 
deb package has been installed.
 
To have systemd start code-server now and restart on boot:
  sudo systemctl enable --now code-server@$USER
Or, if you don't want/need a background service you can run:
  code-server
```

2. code-server 실행 (younlea 는 현재 제 계정입니다)

$sudo systemctl start code-server@younlea

$sudo systemctl enable code-server@younlea

3. 외부 포트와 패스워드 설정

$vim ~/.config/code-server/config.yaml

필자의 경우 아래와 같이 4000번 포트로 쓰도록 구성함.

bind-addr 0.0.0.0:4000

auth:password

password: XXXXXXXX      << 본인 패스워드 설정

cert:false

**wifi ap에서 DDNS 설정 및 port forwarding 설정**.

**접속**

참고 : <https://wnjoon.tistory.com/106>\
\