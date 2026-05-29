---
title: "Android more key 없는 버젼 대응"
excerpt_separator: "<!--more-->"
date: 2016-04-06 01:18:27 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

예전 버젼의 경우 HW more key가 있어서 이를 누르면 아래 함수가 호출 되었었다.

onCreateOptionsMenu

그런데... 특정 버젼 이후 more key가 컨셉이 바껴서 동작이 안된다.

예전 코드의 경우 이쪽에 메칭되어 있어서.. .이를 호출하는 방법을 찾아 보니...

openOptionsMenu();  << 를 호출하면 되는것을 확인했다.

이제 다시 안드로이드 app을 공부하며.. 정리를 시작한다..