---
title: "Install docker on MAC OS"
excerpt_separator: "<!--more-->"
date: 2018-05-07 15:47:50 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

1st step. Download docker program on docker site.

<https://store.docker.com/editions/community/docker-ce-desktop-mac>

2nd step. check docker installation status.

```
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker
 
 
 
Usage:    docker COMMAND
 
 
 
A self-sufficient runtime for containers
 
 
 
Options:
 
      --config string      Location of client config files (default
 
                           "/Users/YOUNLEA/.docker")
 
  -D, --debug              Enable debug mode
 
  -H, --host list          Daemon socket(s) to connect to
 
  -l, --log-level string   Set the logging level
 
                           ("debug"|"info"|"warn"|"error"|"fatal")
 
                           (default "info")
 
      --tls                Use TLS; implied by --tlsverify
 
      --tlscacert string   Trust certs signed only by this CA (default
 
                           "/Users/YOUNLEA/.docker/ca.pem")
 
      --tlscert string     Path to TLS certificate file (default
 
                           "/Users/YOUNLEA/.docker/cert.pem")
 
      --tlskey string      Path to TLS key file (default
 
                           "/Users/YOUNLEA/.docker/key.pem")
 
      --tlsverify          Use TLS and verify the remote
 
  -v, --version            Print version information and quit
```

docker version check

```
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker --version
 
Docker version 18.03.1-ce, build 9ee9f40
 
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker-compose --version
 
docker-compose version 1.21.1, build 5a3f1a3
 
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker-machine --version
 
docker-machine version 0.14.0, build 89b8332
```

3rd. first image(hello world) download

```
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker run hello-world
 
Unable to find image 'hello-world:latest' locally
 
latest: Pulling from library/hello-world
 
9bb5a5d4561a: Pull complete 
 
Digest: sha256:f5233545e43561214ca4891fd1157e1c3c563316ed8e237750d59bde73361e77
 
Status: Downloaded newer image for hello-world:latest
 
 
 
Hello from Docker!
 
This message shows that your installation appears to be working correctly.
 
 
 
To generate this message, Docker took the following steps:
 
 1. The Docker client contacted the Docker daemon.
 
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 
    (amd64)
 
 3. The Docker daemon created a new container from that image which runs the
 
    executable that produces the output you are currently reading.
 
 4. The Docker daemon streamed that output to the Docker client, which sent it
 
    to your terminal.
 
 
 
To try something more ambitious, you can run an Ubuntu container with:
```

4th. running ubuntu image(If you did not download ubuntu image, docker automatically will be download)

```
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker run -it ubuntu bash
 
Unable to find image 'ubuntu:latest' locally
 
latest: Pulling from library/ubuntu
 
a48c500ed24e: Pull complete 
 
1e1de00ff7e1: Pull complete 
 
0330ca45a200: Pull complete 
 
471db38bcfbf: Pull complete 
 
0b4aba487617: Pull complete 
 
Digest: sha256:c8c275751219dadad8fa56b3ac41ca6cb22219ff117ca98fe82b42f24e1ba64e
 
Status: Downloaded newer image for ubuntu:latest
 
root@14a3716aea2e:/# cat /etc/issue
 
Ubuntu 18.04 LTS \n \l
 
 
 
root@14a3716aea2e:/# ls
 
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
 
root@14a3716aea2e:/# exit
 
exit
```

Now you can running docker anytime use below command

```
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker run -it ubuntu bash
 
 
docker iamge check and process check. 
dogim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker images
 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
 
ubuntu              latest              452a96d81c30        2 weeks ago         79.6MB
 
hello-world         latest              e38bc07ac18e        4 weeks ago         1.85kB
 
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker ps
 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
 
gim-yunlaeui-MacBook-Pro:~ YOUNLEA$ docker ps -a
 
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                          PORTS               NAMES
 
da3f02dc3b45        ubuntu              "bash"              58 seconds ago       Exited (0) 28 seconds ago                           zealous_gates
 
14a3716aea2e        ubuntu              "bash"              About a minute ago   Exited (0) About a minute ago                       boring_mendeleev
 
8f2de908df96        hello-world         "/hello"            2 minutes ago        Exited (0) 2 minutes ago                            jovial_keller
```

ref : <https://nolboo.kim/blog/2016/08/02/docker-for-mac/>

If you want use kali docker image. please check below link.

<https://www.kali.org/news/official-kali-linux-docker-images/>

Actually you can find more image on below web site.

<https://store.docker.com/search?q=kali&type=image&source=community>