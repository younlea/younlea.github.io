---
title: "vlc server to vlc player"
excerpt_separator: "<!--more-->"
date: 2020-07-27 05:14:30 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

I want to streaming my v4l2loopback data to other computer.

This is simple test .

step1. setting ubuntu pc (I already write to v4l2loopback (/dev/video4)

launch vlc.

![image](/assets/images/posts/6680984/c0028014_5f1de36e4d0a5.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3849584d.png)

![image](/assets/images/posts/6680984/c0028014_5f1de396680e5.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3979e3ec.png)

set dest ip address. (this is my macbook local address. 6^^)

![image](/assets/images/posts/6680984/c0028014_5f1de3a1af2ff.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3b7d376d.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3bbce338.png)

start streaming.

step2. read network streaming using other computer(in my case, Mac book)

![image](/assets/images/posts/6680984/c0028014_5f1de3d76463b.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3e83e104.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3e796f3f.png)

![image](/assets/images/posts/6680984/c0028014_5f1de3f0c3f67.png)

now you can see vlc steaming. ^^;

but I face some delay issue. I guess that this is vlc viewer setting issue. ^^

When I change output stream delay.. but.. the situation is same.

need to check about vlc streaming delay (<https://www.groovypost.com/howto/change-vlc-streaming-buffer/>)