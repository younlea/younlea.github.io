---
title: "Android application 만들기 준비과정.. Install에서 hello world까지.."
excerpt_separator: "<!--more-->"
date: 2012-03-23 11:33:05 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

그냥.. Android phone도 있고.. application을 만들어 볼까하고 책보고 있는거 정리해본다..

필요 사항.

1. JDK (Java Development Kit)

- http://java.sun.com/javase/downloads ([http://www.oracle.com/technetwork/java/javase/downloads/index.html)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

2. Eclipse :

- http://www.eclipse.org/downloads (Eclipse IDE for Java Developers 선택 후 다운로드)

[<http://www.springsource.org/> << 이런 녀석도 있네 ㅡ.ㅡ;]

3. 안드로이드 SDK

- http://developer.android.com/sdk

4. 안드로이드 개발용 이클립스 플러그인 ADT(Android developer Tool)

- <http://developer.android.com/sdk/eclipse-adt.html> 에 나온대로 설치

-> help -> Install New software -> download받는 파일 선택.

- Android Virtual Device Manager를 이용해서 가상 머신을 만든다.

5. android package  설치

- Window -> Android SDK and AVD Manager  에서 원하는  package 설치

package설치시 아래와 같이 에러가 날경우..

1) Misc 아래 force...를 체크해준다.

2) 그래도 안될 경우  Tools -> option -> proxy server와 port를 셋팅 해주면됨.  (pac file을 까 보면 필요한 정보 확인가능)

Failed to fetch URL https://dl-ssl.google.com/android/repository/repository-6.xml, reason: Connection to https://dl-ssl.google.com refused

6. File -> New -> Project를 클후 Android Project를 선택하고 제작시 path문제가 있다.

- 간단하게 Windows 환경변수에 path가 있는데 여기에 본인들의 SDK가 깔린 directory를 넣어주면 해결이 된다.

설치가이드 동영상

<http://www.youtube.com/watch?v=SVZ1P35xgNQ>

<http://www.androidpub.com/>  -> 개발자 공간

<http://www.androidpub.com/1050>

목표

1. 아버지 단축다이얼

2. 딸아이 동영상  youtube 플레이 및 스트리밍 저장..

3. 아버지 야생화 어플