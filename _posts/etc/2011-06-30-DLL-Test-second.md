---
title: "DLL Test second."
excerpt_separator: "<!--more-->"
date: 2011-06-30 15:13:40 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

흠...요세 이것저것 하다가

VC6.0을 써야 하는데... 제공 받은 Code는 VC2008에서 부터 빌드가 된다.

이것도 이번에 처음 알았는데 VC버젼별로 header file에서 지원 하고 안하고 하더라.. ㅡㅜ

어쩔수 없이 VC2010으로 빌드후 dll로 만들어서 VC6.0프로그래에서 Dll을 로드하도록 짜보고 있다.

1. Load는 정상적으로 된다. 그런데.. debug가 안된다 ㅡ.ㅡ;

지난번 말했듯이 같은 버젼의 VC에서 만들었으면 dll debug mode로 빌드하고 debugging을 하면 code가 보이는데..

이게 다른 버젼의 VC면.. 안되는것 같다. 우선 향후 더 테스트 해보고 결과를 공유하겠다..

(현재까지는 안된다 ㅡㅜ)

2. Main - Load Dll - Load Dll 가능하다.

그러니까.. Dll을 로드했는데 이 Dll에서 다른 Dll을 로드하는건 가능하다.. 직접 해보기 전까지는 긴가민가했는데 잘된다.

3. Dll안의 Class 사용...

현재 삽질하고 있다.. 이것도 결과는 해보고 업데이트 하겠다.