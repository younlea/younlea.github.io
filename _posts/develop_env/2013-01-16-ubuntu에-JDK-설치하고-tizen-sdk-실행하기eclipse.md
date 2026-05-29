---
title: "ubuntu에 JDK 설치하고 tizen sdk 실행하기(eclipse)"
excerpt_separator: "<!--more-->"
date: 2013-01-16 12:05:58 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

oracle jdk 검색하면 나오는 곳으로 가서 JDK 최신을 받는다.

받은 자료 압축 풀어서 특정 폴더에 카피하기.

tar zxvf jdk-7u11-linux-i586.tar.gz

sudo mkdir -p /usr/lib/jvm/jdk1.7.0\_11

sudo mv jdk1.7.0\_11/- /usr/lib/jvm/jdk1.7.0.\_11

설치

sudo update-alternatives --install "/usr/bin/java" "java" "/usr/bin/jvm/jdk1.7.0\_11/bin/java" 1

sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/bin/jvm/jdk1.7.0\_11/bin/javac" 1

sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/bin/jvm/jdk1.7.0\_11/bin/javaws" 1

설치 완료 ㅡ.ㅡ;

version 확인 : java -version

아니면...

sudo apt-get install openjdk-7-jdk

그런데 이전 버젼이 있다... 그러면 대략 난감..

이상하게 java version이 이전껄로 나온다.... 자. .그럼 아래와 같이 하면 된답니다.

sudo update-alternatives --config java

0  .....

1  .....

2  .....

java version 선택하는건데 여기서 원하는 버젼을 선택하고 끝내면 됩니다 ^^;

eclipse가 삽질을 할때가 있다.. 실행안되고 아무것도 안나온다..

**workspace/.metadata/.plugin/ 아래 삭제... 이거 android app할때도 그렇고.. 이것도 이 모양이네.. 제길슨... ㅜㅜ**

\