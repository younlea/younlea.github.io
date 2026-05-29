---
title: "Samba 대신 Windows에서 nfs로 연결하는 방법"
excerpt_separator: "<!--more-->"
date: 2013-04-13 09:33:46 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

보통 Linux의 폴더를 Windows에서 NFS로 연결할때는 대부분 Samba를 Linux에 설치해서 CIFS/SMD 등의 방식으로

폴더를 연결하게 된다.

그런데 Samba 없이도 연결이 가능한지 확인해 보니.. 된다..

모.. 기존 linux에서 NFS를 이용해서 서로 연결하는 방식이다.

가정

Linux 주소 : 192.168.0.1

Linux user id : sulac

공유 폴더 /home/sulac/nfs (요기는 만들어야 한다. )

Windows 주소 : 192.168.0.2

우선 Linux server에서 setting

/etc/exports 에서 접속하려는 windows쪽 IP 및 권한 설정을 해준다.

/home/sulac/nfs 192.168.0.\*(rw,no\_root\_squash)

이후 아래와 같이 nfs server를 restart해줌

sudo service nfs-kernel-server restart

그럼 Linux는 준비가 되었고....

Windows 셋팅방법이다.

windows에서 command창을 열고 mount라는 명령어를 쓰면되는데...

기본적으로 disable 되어 있어서.. 아래와 같은 순서로 enable 부터 해야한다.

제어판 - 프로그램 - Windows 기능 사용/사용안함 -> nfs용 서비스 enable

이후 command 창에서 mount 사용가능함.

mount 192.168.0.1:/home/sulac/nfs \*

이렇게 하면 가상드라이버가 잡히는걸 확인할수 있을것이다~~ ^^