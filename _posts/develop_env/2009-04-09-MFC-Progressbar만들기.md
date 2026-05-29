---
title: "MFC Progressbar만들기.."
excerpt_separator: "<!--more-->"
date: 2009-04-09 23:36:52 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

CProgressCtrl\* m\_pProgress;

CStatic\* m\_pStatic;

이렇게 컨트롤 변수를 선언 했다고 가정하면...

숨길때

m\_pProgress->ShowWindow(SW\_HIDE);

보이게할때

m\_pProgress->ShowWindow(SW\_SHOW);\

추가로 간단하게 winapi에서 동작하는 프로그램..[퍼옴 from devpia]\
~~`TrochilusProgressbarwithtext.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*

MFC에서 사용할수 있는 프로그래스바창에 문자 뿌리기...\
SetWindowText