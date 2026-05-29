---
title: "ubuntu에 chrome desktop 설치"
excerpt_separator: "<!--more-->"
date: 2022-01-12 05:27:13 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Ubuntu를 새로 설치하고 chrome remote desktop을 쓰려는데 안된다.. 모지 ㅜㅜ

아래 순서로 설치하면 해결 됨 ^^:

1. chrome 설치

2. headless 설치 (chrome desktop 들어가면 ssh로 설치하기 선택하면 들어가짐)

<https://remotedesktop.google.com/headless>

시작 누르면 아래 링크 받으라고 나옴.

- Debian Linux: <https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb>

요거 받아서 ubuntu로 보내고 아래 명령어로 설치

$sudo dpkg -i [chrome-remote-desktop\_current\_amd64.deb](https://dl.google.com/linux/direct/chrome-remote-desktop_current_amd64.deb)

3. pin number 셋팅(이제 본인 실행시켜야 하는데 이것도 아래 싸이트에 있음 ^^)

<https://remotedesktop.google.com/headless> (시작 이후 승인 버튼 누르면 뜨는데 그중 있는거 copy & paste 하면 됨)

실행 후 크롬 데스크탑으로 원격 접속 잘됨 ㅋ..

몇일간 VNC로 삽질했는데 이거 좋네 ^^;