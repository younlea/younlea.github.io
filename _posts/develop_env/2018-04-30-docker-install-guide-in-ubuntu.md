---
title: "docker  install guide in ubuntu"
excerpt_separator: "<!--more-->"
date: 2018-04-30 14:48:07 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Docker install in ubuntu 16.04\
----------------------------------------------------------------------------------------------------------------------------------------------\
**$ sudo apt-get install curl**\
----------------------------------------------------------------------------------------------------------------------------------------------\
**$ curl -fsSL https://get.docker.com/ | sudo sh**\
----------------------------------------------------------------------------------------------------------------------------------------------\
# Executing docker install script, commit: 36b78b2\
+ sh -c apt-get update -qq >/dev/null\
W: The repository 'http://download.tizen.org/tools/latest-release/Ubuntu\_16.04  Release' is not signed.\
+ sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null\
+ sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null\
+ sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge" > /etc/apt/sources.list.d/docker.list\
+ [ ubuntu = debian ]\
+ sh -c apt-get update -qq >/dev/null\
W: The repository 'http://download.tizen.org/tools/latest-release/Ubuntu\_16.04  Release' is not signed.\
+ sh -c apt-get install -y -qq --no-install-recommends docker-ce >/dev/null\
+ sh -c docker version\
Client:\
 Version:    18.04.0-ce\
 API version:    1.37\
 Go version:    go1.9.4\
 Git commit:    3d479c0\
 Built:    Tue Apr 10 18:20:32 2018\
 OS/Arch:    linux/amd64\
 Experimental:    false\
 Orchestrator:    swarm

Server:\
 Engine:\
  Version:    18.04.0-ce\
  API version:    1.37 (minimum version 1.12)\
  Go version:    go1.9.4\
  Git commit:    3d479c0\
  Built:    Tue Apr 10 18:18:40 2018\
  OS/Arch:    linux/amd64\
  Experimental:    false\
If you would like to use Docker as a non-root user, you should now consider\
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run\
         containers which can be used to obtain root privileges on the\
         docker host.\
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface\
         for more information.\
----------------------------------------------------------------------------------------------------------------------------------------------\
complete install docker. \
 

-----------------------------------------------------------------------------------------------------------------------------------------------\
reference\
[init guide](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)\
[docker\_command](https://docs.docker.com/engine/reference/commandline/docker/)\
[docker\_proxy setting](https://blog.itanoss.kr/ko/%EC%9A%B0%EB%B6%84%ED%88%AC-16-04%EC%97%90%EC%84%9C-docker-%ED%94%84%EB%A1%9D%EC%8B%9C-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/)

[docker install on ubuntu 16.04](http://iamartin-gh.herokuapp.com/ubuntu-16-04-docker-install/)