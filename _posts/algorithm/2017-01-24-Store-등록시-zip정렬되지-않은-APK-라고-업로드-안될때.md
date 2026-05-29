---
title: "Store 등록시 zip정렬되지 않은 APK 라고 업로드 안될때.."
excerpt_separator: "<!--more-->"
date: 2017-01-24 21:54:51 +0900
categories:
  - algorithm
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

APP 다만들고 올리려고 하니까.. 아래와 같은 에러 메세지가 나오는 경우가 있습니다.

zip 정렬되지 않은 APK를 업로드했습니다. APK에 zip 정렬 도구를 실행한 다음 다시 업로드해야 합니다.

해결책은 아래에 있습니다.

<https://developer.android.com/studio/publish/app-signing.html#align>

`zipalign`을 사용하여 서명되지 않은 APK를 정렬합니다.

```
$ zipalign -v -p 4 my-app-unaligned.apk my-app.apk
```

해당 툴은 build-tools 밑에 있습니다.

인증서 셋팅하는 방법

<http://mainia.tistory.com/587>

\