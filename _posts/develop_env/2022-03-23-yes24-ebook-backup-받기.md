---
title: "yes24 ebook backup 받기"
excerpt_separator: "<!--more-->"
date: 2022-03-23 18:45:59 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

cheat engine [설치](https://www.cheatengine.org/#google_vignette)

1. Cheat engine 실행

![image](/assets/images/posts/6902060/c0028014_623aead7e31b1.png)

ebook을 실행하고

Open process로 ebook을 연다.

![image](/assets/images/posts/6902060/c0028014_623b0b0eb2047.png)

memory view를 누른다

![image](/assets/images/posts/6902060/c0028014_623b0b3d6aadf.png)

memory viewer -> view -> reference function을 선택

![image](/assets/images/posts/6902060/c0028014_623b0bb7a2f3e.png)

referenced function선택(아래 OpenMemW를 선택)

![image](/assets/images/posts/6902060/c0028014_623b0c125ecc1.png)

set breakpoint 잡아 두고. ebook(열었던 책만) 다시 실행

![image](/assets/images/posts/6902060/c0028014_623b0c89049f5.png)

아래 메모리 주소에서 start와 size를 확인한다.

start 주소는 : 0X16168020

size는 : 0x003753E8  ( 여기 Size는  start 주소 바로 다음에 써진거 쓰면 된다. ESI는 종종 바뀐다 ㅡㅡ;)

![image](/assets/images/posts/6902060/c0028014_623b0d1588ef3.png)

이후 memory를 저장한다.

![image](/assets/images/posts/6902060/c0028014_623b10518b5cd.png)

start 주소는 : 0X16168020, end 주소 : 0x164DD408

size는 : 0x003753E8

![image](/assets/images/posts/6902060/c0028014_623b110c06c42.png)

![image](/assets/images/posts/6902060/c0028014_623b112cabf53.png)

![image](/assets/images/posts/6902060/c0028014_623b1134f300c.png)

이렇게 하면 sample.pdf가 저장이 된다.

- 단어장에 추가
  - 다음에 대한 단어 목록이 없습니다영어 → 한국어...
  - 새로운 단어 목록 생성...
- 복사

- 단어장에 추가
  - 다음에 대한 단어 목록이 없습니다아랍어 → 한국어...
  - 새로운 단어 목록 생성...
- 복사

- 단어장에 추가
  - 다음에 대한 단어 목록이 없습니다영어 → 한국어...
  - 새로운 단어 목록 생성...
- 복사

- 단어장에 추가
  - 다음에 대한 단어 목록이 없습니다영어 → 한국어...
  - 새로운 단어 목록 생성...
- 복사