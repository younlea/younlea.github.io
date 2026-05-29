---
title: "ARTIK USB adb"
excerpt_separator: "<!--more-->"
date: 2017-06-05 16:00:12 +0900
categories:
  - embedded
tags:
  - etc

toc : true
toc_sticky : true
---

ARTIK에서 부팅후 USB port로 디버깅을 하려고 할때...

USB to serial에 연결하지 않고 USB에 연결해서 하는 방법이 필요했다.

부팅할때 아래 cmd를 넣게 되면 ARTIK 부팅후 USB로 adb shell이 enable 된다.^^:

[root@artik ~]# **systemctl start adbd.service**

[root@artik ~]# **systemctl enable adbd.service**

부팅할때 자동으로 ADB가 되게 하기 위해서 아래와 같이 하면 됩니다.

cd /etc/rc.d/

touch rc.local

chmod 777 rc.local

vi rc.local

--> 아래 내용 넣습니다.

#!/bin/sh

systemctl start adbd.service

systemctl enable adbd.service

재부팅을 하면 adb가 실행되는것을 확인하실수 있을겁니다.

\