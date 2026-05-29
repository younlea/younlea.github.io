---
title: "Invalid project description"
excerpt_separator: "<!--more-->"
date: 2012-08-19 15:09:34 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

받은 TEST용 APP Source project를 내쪽에서 열때....

보통 File -> Import -> General -> Existing Projects into Workspace -> 디렉토리 선택... 하면 되는데..

이게.... 오랫동안 안하면.. 아래 처럼 하게 된다..

File -> New -> Project -> Android -> Android Project from Existing Code....

이렇게 하면.. "Invalid project description"이 뜨게 되는것 같다. 에휴...

모 이런 경우...

Source code를 workspace 밖으로 이동해서

File -> New -> Project -> Android -> Android Project from Existing Code -> 디렉토리 선택하면 해결된다.

이후 Workspace로 복사해서 import 시켜도 정상적으로 동작하네요..

\