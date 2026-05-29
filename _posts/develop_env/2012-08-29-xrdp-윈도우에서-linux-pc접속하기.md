---
title: "xrdp (윈도우에서 linux pc접속하기)"
excerpt_separator: "<!--more-->"
date: 2012-08-29 15:10:10 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

linux PC에 xrdp를 인스톨 한다. (본인은 ubuntu12.04 버젼임. )

$ sudo apt-get install xrdp

인스톨후 원격제어 연결하면 바탕화면만나오고 아이콘들이 안나오는데 아래 부분 처리해주면 됨.

sudo vim ~/.xsession

`입력 : gnome``-``session``-``-``session``=``ubuntu``-``2d`

`\`

`sudo service xrdp restart 하면 실행됨`

`\`

혹시 아래 에러가 나면 아래 링크에 있는 방법으로 해결해 보면 될듯하다.

failed to load session "ubuntu-2d"

<http://miscelaneatech.blogspot.kr/2012/07/unable-to-log-to-session-unity-problem.html>

sudo apt-get install unity-2d

ubuntu14.04

**$** sudo apt-get install xrdp

**$** sudo apt-get install xfce4

**$** sudo vim  /etc/xrdp/startwm.sh

. /etc/X11/Xsession를 삭제 또는 주석 처리(맨앞에 #추가)하고 아래와 같이 수정한다.

|  |
| --- |
| #!/bin/sh  if [ -r /etc/default/locale ]; then   . /etc/default/locale   export LANG LANGUAGE fi  #. /etc/X11/Xsession . /usr/bin/startxfce4 |

**$**sudo echo xfce4-session > ~/.xsession

**$**sudo /etc/init.d/xrdp restar

windows : mstsc /v:address

움.. 위는 좀 이쁘지 않아서 찾아보니 아래와 같이 하면 좀더 ubuntu UI랑 비슷하게 뜬다. (<http://wincloud.link/pages/viewpage.action?pageId=9175071>)

```
$ sudo apt-add-repository ppa:ubuntu-mate-dev/ppa
$ sudo apt-add-repository ppa:ubuntu-mate-dev/trusty-mate
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install ubuntu-mate-core ubuntu-mate-desktop
 
$ echo mate-session > ~/.xsession
$ sudo service xrdp restart
```