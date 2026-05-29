---
title: "add user 후 sudo passwd 셋팅안될때 ㅡ.ㅡ;"
excerpt_separator: "<!--more-->"
date: 2012-12-04 10:12:37 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

특수문자를 포함한 이름을 넣어야 할 경우... 이름 추가한 후에 sudo passwd 설정해도 잘 안되서...

sudo vi /etc/group 에서.. 이전 ID 값을 xxx.xx 라는 새로운 계정 이름으로 수정했다 ㅡ.ㅡ;

이렇게 하면.. 이전 ID값으로 로그인하면 안되고 xxx.xx라는 계정으로 로그인 하면 다 된다.. ㅡ.ㅡ;

보니까. sudoers file에 ID가 안들어 있어서 그런것 같으나. 어짜피 난 .. xxx.xx 라는 아이디를 쓸꺼니까.. 여기서 끝.