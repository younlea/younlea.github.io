---
title: "[펌]Linux에서 nfs mount 안될때~~~"
excerpt_separator: "<!--more-->"
date: 2012-04-26 09:26:44 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

mount rpc unable to receive errno = connection refused  잘되던 타겟에서 이런 에러가 나서.. 찾아보니..

결국 linux server에서 service nfs start 를 해서 해결

----------------------------------------------------

nfs가 설치가 안되어 있어서 안될 경우도 있는데 이때는 아래 package를 받으면 된다.

ubuntu의 경우 apt-get으로 받는다. 

apt-get install nfs-common

apt-get install nfs-kernel-server

----------------------------------------------------

아래 블로그에서 해결책을 찾았다는 ^^;

 소스 : <http://blog.naver.com/jejezz?Redirect=Log&logNo=50012532464>

1. Linux에서 export에 디렉토리를 등록한다.

sudo vi /etc/exports

[directory] [허용된 IP 영역](options)

예) /home/user      192.168.0.\*(rw,no\_root\_squash,no\_all\_squash,async,no\_subtree\_check)

 

2. Linux에서 nfs demon을 실행시킨다.

service nfs start

export 값을 적용하려고 start를 하면 잘 안될때가 있는데 그때는 아래와 같이 nfs restart를 하면 된다.

> ~$ sudo /etc/init.d/portmap restart

> ~$ sudo /etc/init.d/nfs-kernel-server restart

> ~$ sudo /etc/init.d/portmap restart

 

3. Linux 에서 nfs module을 실행시킨다.

insmod nfs

확인 --> lsmod | grep nfs

 

4. Linux에서 rpcinfo -p 로 rpc 포트 정보를 본다. 다음과 비슷하게 나온다.\
===============================================\
   프로그램 버전 원형   포트\
    100000    2   tcp    111  portmapper\
    100000    2   udp    111  portmapper\
    391002    2   tcp  32768  sgi\_fam\
    100011    1   udp    981  rquotad\
    100011    2   udp    981  rquotad\
    100011    1   tcp    984  rquotad\
    100011    2   tcp    984  rquotad\
    100003    2   udp   2049  nfs\
    100003    3   udp   2049  nfs\
    100021    1   udp  38049  nlockmgr\
    100021    3   udp  38049  nlockmgr\
    100021    4   udp  38049  nlockmgr\
    100005    1   udp  38050  mountd\
    100005    1   tcp  33629  mountd\
    100005    2   udp  38050  mountd\
    100005    2   tcp  33629  mountd\
    100005    3   udp  38050  mountd\
    100005    3   tcp  33629  mountd\
=========================================

 

5. VMWARE 설정\
Edit --> Virtual Network Editor --> NAT --> Edit 버튼 --> Port Forwarding 버튼

이때 위에 있는 portmapper(tcp/udp) 와 nfs(udp), 그리고 mountd(udp) 를 등록(Port Forwarding)한다.\
예)\
Portmapper : TCP incoming port --> ADD 버튼\
Host Port: 111 \
Virtual Machine IP Address : Vmware Linux Ip Address \
Port : 111\
Description : 아무 이름\
OK 버튼 클릭

이런 식으로 4개를 모두 등록한다.

 

6. Target

마지막으로 Target에서\
mount -t nfs 192.168.1.84:/home/jyahn/kp\_bin /mnt/hdd/nfsroot -o udp,nolock,rsize=1024,wsize=1024\
을 입력한다. (rsize와 wsize는 1024이외에는 테스트하지 않았음.... 1024/2048는 성공)

**[출처]** [Target에서 VMware Linux로 NFS 설정](http://blog.naver.com/jejezz/50012532464)|**작성자** [종유니](http://blog.naver.com/jejezz)

\