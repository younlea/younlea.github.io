---
title: "OpenCV를 이용한 hand mouse 만들기."
excerpt_separator: "<!--more-->"
date: 2012-08-08 20:17:35 +0900
categories:
  - etc
tags:
  - etc

toc : true
toc_sticky : true
---

usr/lib/pkgconfig에 .pac 파일이 있을 경우..

make file 만들기

INC = `pkg-config --cflags opencv`

LIBS = `pkg-config --libs opencv`

$(TARGET) : $(OBJS)

$(CC) -o $@ $(OBJS) $(LIBS) -lm

<http://blog.naver.com/PostView.nhn?blogId=phs6669&logNo=100135087269&parentCategoryNo=2&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView>

OpenCV2.4.2 사용 코드.

~~`handmouse.c`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*~~`Makefile`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*