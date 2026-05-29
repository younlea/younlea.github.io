---
title: "visual studio pacakge 종속성(nuget)"
excerpt_separator: "<!--more-->"
date: 2020-07-28 13:42:02 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

좀더 찾아봐야 하겠는데..

MFC로 C++만 하던 나에게.. 새로운 세계?? C# winform을 하면서 종속성 관련 자동으로 package 딸려오는게 있는듯하여

찾아서 정리 하는중..

visual studio 2019에서는 찾아보니 nuget.org?? 라는 걸 쓰면 되는것 같다.

도구 ->  NUget package 관리자 -> 솔루션 nuget 패키지 관리 -> 추가..

예전에는 빌드할때 static이나 dynamic이냐에 따라서 패키지를 포함하고 안하고 했었는데..

이제는 여기에 넣어주면 코드만 보내도 알아서 패키지를 받아서 빌드를 할수 있을것 같다.

추가적인 테스트를 해서 넣어 보도록 하겠다.