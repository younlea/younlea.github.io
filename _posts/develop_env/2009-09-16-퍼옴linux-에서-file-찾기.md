---
title: "[퍼옴]linux 에서 file 찾기."
excerpt_separator: "<!--more-->"
date: 2009-09-16 22:15:46 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

**find** 라는 명령으로, 디스크에 저장된 각종 파일/디렉토리를 **검색**할 수 있습니다.

### 파일 찾기 (파일명 검색)

현재 디렉토리에서, pl 확장자를 가진 모든 파일 찾기\

find -name '\*.pl'\

(현재 디렉토리 밑의 하위 디렉토리까지 다 찾습니다.)

루트에서부터, 즉 전체 하드에서, pl 확장자를 가진 모든 파일 찾기\

find / -name '\*.pl'\

전체 하드 디스크에서, 파일명이 ab 로 시작하는 모든 파일 찾기\

find / -name 'ab\*'\

전체 하드 디스크에서, 파일명이 .bash 로 시작하는 모든 파일 찾기\

find / -name '.bash\*'\

전체 하드 디스크에서, 파일명이 .bash 로 시작하는 모든 파일 찾기\
+ ls 명령 형식으로 출력\

find / -name '.bash\*' -ls\

뒤에 -ls 라는 옵션을 붙이면 됩니다.

### 디렉토리명 찾기

전체 하드 디스크에서, 디렉토리 이름이 et 로 시작하는 모든 디렉토리 찾기\

find / -name 'et\*' -type d\

주의! 옵션 순서를 바꾸면 에러가 납니다.

grep도 있는데... \

**1. 문자열 찾기(영어 전용)**

> # grep -rw "찾는문자열" ./

**2. 문자열 찾기**

> # grep -i -l "찾는문자열" \* -r 2> /dev/null
>
> 2>/dev/null : 에러출력을 /dev/null 로 보내라는 의미

**3. 문자열 찾기(한영 공용)**

> # find . -exec grep -l "찾는문자열" {} \; 2>/dev/null

**4. 문자열 찾기(한영, 대소문자 무시)**

> # find . -exec grep -i -l "찾는문자열" {} \; 2>/dev/null
>
> 옵션 i는 대소문자를 무시하라는 의미

**5. 문자열 찾은 후 치환**

> # find . -exec perl -pi -e 's/찾을문자열/바꿀문자열/g' {} \; 2>/dev/null

**6. 파일 찾기**

> # find / -name 파일명 -type f

**7. 파일 찾기(대소문자 무시)**

> # find / -iname 파일명 -type f

**8. 디렉터리 찾기**

> # find / -name 파일명 -type d

**9. 디렉터리 찾기(대소문자 무시)**

> # find / -iname 파일명 -type d

**10. 하위 디렉터리에서 모든 파일 찾기**

> find . | xargs grep '파일명'

\