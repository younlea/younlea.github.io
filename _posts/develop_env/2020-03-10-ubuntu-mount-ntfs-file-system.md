---
title: "ubuntu mount ntfs file system."
excerpt_separator: "<!--more-->"
date: 2020-03-10 09:14:25 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

삽질도 이런 삽질은 ㅡ.ㅡ;

보통 마운트는  아래와 같이 한다.

sudo mount /dev/sdc1 /ex\_hdd2\_1T

요기를       요기에 마운트해라 .ㅡㅡ

이렇게 마운트하고 owner 나  mode 를 아래와 같이 수정이 가능하다.

chown younlea:younlea ./ex\_hdd2\_1T

chmod 755 ./ex\_hdd2\_1T

그런데 이 모드가 안될때가 있다 ㅡ.ㅡ; 모지 모지.. ㅜㅜ

결구 아래와 같이 mode 를 셋팅가능한거 확인해고.. 그런데 owner 가 안바뀐다.

mount -t ntfs -o umask=022 /dev/sdc1 /ex\_hdd2\_1T

마짐가으로 mount에서 아래와 같이 셋팅가능한걸 확인했다. 참고로 앞에 -o 가 하나씩 붙어 있는거 보이는가?

이거 앞에 하나씩 안붙혀 주면 안되더라 ㅜㅜ

sudo mount -t ntfs -o uid=younlea -o gid=younlea -o umask=022 /dev/sdc1 ex\_hdd2\_1T/

대충 마운트 끝내기 ^^; 완료