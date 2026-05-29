---
title: "arducamera lens issue about red tint."
excerpt_separator: "<!--more-->"
date: 2020-11-16 16:04:47 +0900
categories:
  - develop_env
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

아두카메라를 사용할때 이상하게 화면이 붉게 나오는 현상이 있어서 삽질하다가...

해당 업체의 사이트에서 해결책을 찾았다

<https://www.arducam.com/docs/cameras-for-raspberry-pi/native-raspberry-pi-cameras/lens-shading-calibration/>

그런데 새로 빌드를 하려고 하니.. 무언가 빠트려서.. ㅡ.ㅡ;

코드 3 부분을 수정해야 하는데 한 부분의 위치를 가이드를 안해줬다.

대충 세번째 부분을 아래 영역 함수 영역에 넣으면 해결이 된다.

create\_camera\_componet

해결한 파일은 정리해서 올리도록 하겠다.

\