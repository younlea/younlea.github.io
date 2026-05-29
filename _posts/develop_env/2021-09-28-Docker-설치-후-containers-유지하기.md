---
title: "Docker - 설치 후 containers 유지하기.."
excerpt_separator: "<!--more-->"
date: 2021-09-28 06:30:50 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

docker를 이용해서 image를 받고 실행 후 containers에서 이것저것 install 한후 재 시작하면. 엄..

설치한 게 날라간다. .ㅡㅡ; 써글..

docker 자체가 기본 이미지를 유지하는 아이디어로 되어 있는거라...

containers에 작업한 걸 유지 하기 위해선 나만의 image를 저장하면 된다. 마치 git commit 처럼 ^^;

1. 설치..

2. docker ps 로 id 확인

3. docker commit [id] [저장할 이름]

![image](/assets/images/posts/6847566/c0028014_615237fd969bb.png)

확인해 보면

![image](/assets/images/posts/6847566/c0028014_61523804770a9.png)

ref : ~~[http://bahndal.egloos.com/637633](http://bahndal.egloos.com/637633)~~ *(이글루스 블로그, 서버 종료)*

도커(Docker) 이미지(image)를 실행하면 컨테이너(container)가 생성된다.

이미지를 실행한 후에 이런 저런 작업을 해서 변경할 경우 컨테이너의 내용이 변경되는 것이고 이미지는 변경되지 않는다.

(컨테이너가 종료되면 변경사항은 모두 사라진다)

예를 들어 ubuntu 이미지를 실행하고 vim 에디터를 설치하는 상황을 가정해 보자.

# 이미지 목록 확인

sudo docker images

# ubuntu 이미지 실행(-it 옵션, bash 사용)

sudo docker run -it ubuntu

위와 같이 실행하면 ubuntu 이미지로부터 컨테이너가 생성되고, -it 옵션을 주었기 때문에 bash 명령 프롬프트를 통해 컨테이너에 접속 된다.

이제 이 상태에서 아래와 같이 입력해서 vim을 설치했다고 하자.

# S/W 저장소 갱신 (도커 컨테이너)

apt-get update

# vim-tiny 패키지 설치 (도커 컨테이너)

apt-get install vim-tiny

이 변경된 사항을 별도의 이미지로 저장할 수 있다.

컨테이너가 실행 중인 상태를 그대로 두고 우선 별도의 터미널 창을 실행한 후, 현재 실행 중인 컨테이너의 ID를 먼저 파악한다.

# ubuntu 이미지에서 실행한 컨테이너 목록 출력(별도의 터미널창)

**sudo docker ps**

출력 결과에서 ubuntu 이미지에 대응하는 컨테이너의 "CONTAINER ID" 항목을 보자.

예를 들어 이 값이 "c8dc84588c31"이라고 하고, 새로 만들 이미지의 이름을 ubuntu\_test라고 한다면 아래와 같이 docker commit 명령을 실행하면 된다.

# 컨테이너 ID "c8dc84588c31"을 ubuntu\_test 이미지로 저장(별도의 터미널창)

**sudo docker commit c8dc84588c31 ubuntu\_test**

이제 새로운 이미지가 만들어졌는지 docker images 명령으로 확인한다.

# 이미지 목록 출력

sudo docker images

ubuntu\_test 이미지를 실행해 보면, 이 이미지에는 이제 vim 에디터가 포함되어 있음을 확인할 수 있다.

# ubuntu\_test 이미지 실행

sudo docker run -it ubuntu\_test

\