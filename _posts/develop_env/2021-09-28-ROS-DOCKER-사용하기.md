---
title: "ROS - DOCKER 사용하기"
excerpt_separator: "<!--more-->"
date: 2021-09-28 03:17:03 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

DOCKER는 움.. 대충 아래 링크 따라서 설치하고...

<https://docs.docker.com/desktop/windows/install/>

이후 ROS에 들어가 보니 docker로 설치할 수 있다고 해서 찾아 봤는데. 어디서 어떻게 설치하지?? 라는 고민을..

일단 powershell에서 아래 스크립트를 실행하면 된다고 한다. (windows wsl2 업데이트로 그냥 powershell에서 쓰면 된다구 ㅜㅜ)

요기서 tag\_name을 넣어야 하는데 요건 아래 링크로 들어가면 설명이 되어 있다.

```
docker pull osrf/ros2:<tag_name>
```

```
List of tags available at https://hub.docker.com/r/osrf/ros2/tags
```

ref : <https://hub.docker.com/r/osrf/ros2>

결국 docker pull osrf/ros:devel 로 땡기면 설치까지 된다.

![image](/assets/images/posts/6847555/c0028014_61520ad07ccf5.png)

설치하고 나서 docker를 보면 아래처럼 osrf/ros2가 생긴 걸 볼 수 있다.

![image](/assets/images/posts/6847555/c0028014_61520b0c8f83c.png)

해당 image 오른쪽에 run을 누르면 아래와 같이 containers/app에 실행된 이미지가 보인다.

![image](/assets/images/posts/6847555/c0028014_61520c1b2fd82.png)

훔 그런데 bash 실행하고 되는게 없네 ㅡ.ㅡ;

훔. 다시 docker 이미지를 땡긴다.

docker pull osrf/ros2:nightly

docker image 실행하고 나서...

#bash

root@e479622bb3b0:/# source /opt/ros/rolling/setup.bash    //<<일단 기본 셋업을 해야 실행 가능함.

root@e479622bb3b0:/# ros2 run turtlesim turtlesim\_node  //QT가 없어서 에러가 난다.

![image](/assets/images/posts/6847555/c0028014_6152296606c3d.png)

windows에서 linux docker image의 gui가 되도록 해주는 package

XcsSrv 설치

<https://dev.to/darksmile92/run-gui-app-in-linux-docker-container-on-windows-host-4kde>

<https://psychoria.tistory.com/739>

xming : <http://www.straightrunning.com/XmingNotes/>

xming - wsl2 연결 가이드 : <https://evandde.github.io/wsl2-x/>

실행시 아래와 같이 실행

PS C:\Users\younl> docker run --rm -it -e DISPLAY=host.docker.internal:0.0 -e QT\_X11\_NO\_MITSHM=1 osrf/ros2:nightly

PS C:\Users\younl> docker run -ti --rm -e DISPLAY=$DISPLAY osrf/ros2:nightly

로 하고 하니 성공적으로 gui까지 뜬다.. ^^;

![image](/assets/images/posts/6847555/c0028014_6152393ac7c7d.png)

ref : <https://ropiens.tistory.com/38>

여기까지 하고 containers를 종료하면.. 다 날라간다는 사실 ㅜㅜ

일단 설치해 놓은 건 저장하자.

docker commit 하면 되는데 설명은 아래 링크에서 ^^;

[https://younlea.github.io/develop_env/Docker-설치-후-containers-유지하기/](https://younlea.github.io/develop_env/Docker-설치-후-containers-유지하기/)

다시 정리하면 실행을 정리하면.. (일단 갑자기 docker desktop이 지워져서 ㅡ.ㅡ;)

1. windows에서 Xlaunch를 실행 (X windows)

2. docker image ls 해서 이미지를 찾는다.

```
PS C:\Users\younl> docker image ls
REPOSITORY        TAG            IMAGE ID       CREATED        SIZE
ros2_sulac_test   latest         fe4c558d882b   22 hours ago   7.76GB
osrf/ros2         nightly        110a4b4260d5   2 months ago   7.18GB
osrf/ros          foxy-desktop   9721d1436224   2 months ago   3.06GB
ubuntu            16.04          f6f49faac5cf   6 months ago   132MB
osrf/gazebo       gzweb8         10fe7dc7a045   3 years ago    2.91GB
```

3. docker image 실행

docker run -ti --rm -e DISPLAY=$DISPLAY ros2\_sulac\_test  << 이상하게 또 안된다 ㅡㅡ

docker run --rm -it -e DISPLAY=host.docker.internal:0.0 -e QT\_X11\_NO\_MITSHM=1 ros2\_sulac\_test  <<이게 되네

4. (docker shell에서)

root@0a315ea0cef0:/# source /opt/ros/rolling/setup.bash

root@0a315ea0cef0:/#  ros2 run turtlesim turtlesim\_node

동작한다. 흠냐.. 이제 gazebo도 해봐야것다. ^^;

\