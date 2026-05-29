---
title: "Tinyduino(타이니두이노) - 03-3. bluetoothchat program"
excerpt_separator: "<!--more-->"
date: 2014-11-10 23:47:39 +0900
categories:
  - embedded
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

android developer site에 bluetooth 친절한 설명이 있다.\
http://developer.android.com/guide/topics/connectivity/bluetooth.html\
예전에는 위 주소에 있었는데 없길래 좀 찾아보니 android sample code(bluetoothchat)는 아래 주소에서 받을수 있다.

https://android.googlesource.com/platform/development/+/eclair-passion-release/samples/BluetoothChat/

![image](/assets/images/posts/5854010/c0028014_5460cf97684f7.png)\
위 tgz를 클릭하면 다운로드가 되며 tar.gz 으로 압축이 되어 있어서 윈도우에서는 7zip을 이용해서 압축을 풀면 된다. 

자... 그럼 이제 빌드를 해봐야죠 ^^;

보통 workspace에 압축을 풀고 아래 처럼 하는데... 움..\
![image](/assets/images/posts/5854010/c0028014_5460d393b3b73.png)\
![image](/assets/images/posts/5854010/c0028014_5460d399992d1.png)\
![image](/assets/images/posts/5854010/c0028014_5460d39f3b7b8.png)\
![image](/assets/images/posts/5854010/c0028014_5460d3a30418d.png)\
![image](/assets/images/posts/5854010/c0028014_5460d3a5ce83f.png)\
딱.. 아래와 같이 에러가 뜹니다. workspace에 같은게 있다는건데.. 생전 마들지 않은게 있다고하니 답답하네요.\
![image](/assets/images/posts/5854010/c0028014_5460d3aa0b001.png)\
인터넷 검색하면 바로 답이 나와 있습니다.\
그냥 workspace말고 다른데 만들어서 로드하면 되네요. 아래 처럼 ^^;\
![image](/assets/images/posts/5854010/c0028014_5460d3dba353d.png)

헐.. 이제 eclipse에 올렸더니.. error만 68개.. 이건 몰까요? ^^;\
![image](/assets/images/posts/5854010/c0028014_5460d47012fdd.png)\
이전에 다 해결했던 문제인데 다시 보니 신세계네요.

자.. 이건.. 아래와 같이 project만든 버젼에 대한 library가 없어서 생긴 이슈인데요...\
import에서 android.bluetooth를 못 찾는데.. api 3은 설치 했는데 ㅡ.ㅡ; 이제 찾아봐야 겠네요 ㅡㅜ\