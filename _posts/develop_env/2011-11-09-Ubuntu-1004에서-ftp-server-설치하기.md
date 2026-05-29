---
title: "Ubuntu 10.04에서 ftp server 설치하기.."
excerpt_separator: "<!--more-->"
date: 2011-11-09 17:42:32 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

우분투에서 ftp서비스중 추천이 vsftp 더군요..

system -> synaptic Package Manager

Quick search로 vsftp를 찾아서 설치를 한다.

또는 sudo apt-get install vsftpd 로 설치하면 됨.

$sudo vim /etc/vsftpd.conf를 수정하면 셋팅을 바꿀수 있다.

$ 수정후 재 시작은 sudo /etc/init.d/vsftpd restart 이다.

ftp://xxx.xxx.xxx.xxx 로 접속하면.. 음.. ID랑 PW를 넣어야지 접속이 되는군..

home/Id 밑으로 다 보이게 된다.

자세한 사항은 아래 블로그를 보시면 됩니다. (이분 글을 보고 후딱 했죠)

<http://blog.tinywolf.com/267>

<http://www.smilepeople.co.kr/IT/564389>  : vsftpd.chroot\_list에 사용자를 추가로 적어야 한다.

움.. 우선 아무나 접속할 폴더를 만들어서 접속하게 하려고 한다.

/etc/vsftpd.conf 에서 아래와 같이 수정..

anonymous\_enable=YES

anon\_other\_write\_enable=YES

anon\_upload\_enable=YES

anon\_root=/home/ftp        <= 만들 폴더는 755 옵션으로 만들어야 한다.

anon\_mkdir\_write\_enable=YES

write\_enable=YES

셋팅하고 아래와 같이 해주고 나서 /etc/init.d/vsftpd restart 해주면 됨.

$sudo chown ftp.ftp ~/ftp/upload  << 이건 ftp.ftp가 아니라 id.id로 해야 하네 ㅡ.ㅡ 제길..

$sudo chmod 777 -R ~/ftp/upload

log는 아래에 남게 됩니다.

/var/log/vsftpd.log