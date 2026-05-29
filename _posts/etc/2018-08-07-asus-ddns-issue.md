---
title: "asus ddns issue."
excerpt_separator: "<!--more-->"
date: 2018-08-07 23:44:33 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

500Mbskt 광랜 라이트로 변경후 문제..

asus rt-ac56r에서 잘되던 DDNS가 안된다.

이전과 다른건 500Mb를 지원하기 위해 모뎀을 추가로 장착한건데.. 아래 같은 에러가 나온다 ㅜㅜ

![image](/assets/images/posts/6371600/c0028014_5b69af3c43e80.png)

![image](/assets/images/posts/6371600/c0028014_5b69aec4e7492.png)

Asus Q&A : <https://www.asus.com/support/FAQ/1011725/>

확인해 보니.. 모뎀을 달아서 문제인듯.. 해결책으로 아래와 같은 답변들이 있다.

<https://superuser.com/questions/1094389/not-able-to-setup-ddns-in-the-wireless-router>

-> 요는 모뎀을 bridge타입으로 셋팅하라는건데.. 내일 SKB에 문의해 봐야것다.

<https://www.snbforums.com/threads/ddns-multiple-nat-problem.42708/>

요런 방법도 있다는데.. 잘 모르것데 ㅜㅜ

아 안되면 STB 때려치고 다시 U+로 돌아가야 하나 ㅜㅜ

인터넷에 용자들은 많다... 결국 H614G SK 모뎀 접속해서 NAT를 bridge 모드로 바꿔서 해결..

<http://comterman.tistory.com/1408>

속도가 잘 나올려나~~~

\