---
title: "vscode c++ build 및 debug하기"
excerpt_separator: "<!--more-->"
date: 2022-12-23 09:26:51 +0900
categories:
  - develop_env
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

vscode로 C++코드를 빌드하고 테스트 하려고 하면 초기 셋팅을 해야 하는게 귀찮아서 잘 안하다가 다시 셋팅하는법 간단히 정리 합니다.

1. c++ package설치.

![image](/assets/images/posts/7045385/c0028014_63a4f68e3264b.png)

2.  ctrl+shift+p : configure build task 로 tasks.json 셋팅 (이때 windows는 cpp file을 열고 있어야 함.)

![image](/assets/images/posts/7045385/c0028014_63a4f6cd1baa3.png)

컴파일러 선택

![image](/assets/images/posts/7045385/c0028014_63a4f6ed3fabb.png)

빌드는 ctrl + shift + B를 눌러서 빌드하면됨

아니면 ctrl + shift + p를 눌러서 run build task를 검색해서 실행해도 됨.

3. launch.json 만들고 빌드후 debugging. (디버깅 하기)

configure 추가.  (Run -> Add configuration...)  -> launch.json file생성됨

이후 launch.json file을 열고 다시 한번 run -> add configuration을 눌러주면 아래와 같이 선택하는 창이 나옴.

우리는 launch해서 테스트 할꺼니까 (gdb) Launch를 눌러서 진행.

![image](/assets/images/posts/7045385/c0028014_63a4f7340c529.png)

아래와 같이 파일이 만들어 지는 디버깅할 program 명을 넣으라고 나오는 부분에 바꿔서 넣어주면 됨

![image](/assets/images/posts/7045385/c0028014_63a4f5efabd31.png)

![image](/assets/images/posts/7045385/c0028014_63a4f65433667.png)

![image](/assets/images/posts/7045385/c0028014_63a4f63f0adbd.png)

이후 브레이크 포인트 걸어놓고 F5로 실행 및 디버깅 하면 됨

![image](/assets/images/posts/7045385/c0028014_63a4f64566797.png)

\