---
title: "mobile hotspot 만들기"
excerpt_separator: "<!--more-->"
date: 2021-10-13 04:21:03 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

보통은 wifi AP에 PC와 디바이스를 물려서 ssh로 연결을 하는데..

AP를 셋팅하기 귀찮을때..

이럴때는 디바이스에서 softap기능을 구현해서 PC에서 해당 디바이스(host)로 연결하기도 한다.

움 그런데 해당 디바이스가 softap기능 올리기가 에메하면..

반대로 PC쪽에서 softap를 올려서 device에서 붙는 걸로도 가능한데.. ^^;

이 경우에는 device에서 PC쪽 ip를 넣고 접속하는 기능을 구현해야 한다.

모 어찌됐던 결국에는 사용하기 편하게 만들어야 하는데. 훔..일단 PC쪽에서 셋팅하는 방법들을 찾아보았다.

ref (tizen) : <https://docs.tizen.org/application/native/guides/connectivity/softap/>

ref (windwos) : <https://www.tp-link.com/us/support/faq/2021/>

ref (debian&ubuntu) : <https://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point/>