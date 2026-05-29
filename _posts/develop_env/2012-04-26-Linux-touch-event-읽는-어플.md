---
title: "Linux touch event 읽는 어플?"
excerpt_separator: "<!--more-->"
date: 2012-04-26 11:16:08 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

움...touch drvier는 값을 읽어서 input device로 data를 보내는것으로 보인다.

대부분 QT를 써서 하는거라.. 이렇게 직접 읽는건 짜본 사람이 없는것같다 ㅜㅜ

결국 정리하면 /dev/input 에 있는  event0를 읽어서 처리하면 될듯하다..

기본적으로 읽는건 아래처럼 구현하면 된다.

이제 가공을 해볼까나~~

1 #include <stdio.h>

2 #include <string.h>

3 #include <sys/types.h>

4 #include <unistd.h>

5 #include <fcntl.h>

6 #include <linux/input.h>

7 #define EVENT\_BUF\_NUM 64

8

9 void touch\_wait(void)

10 {

11         int print\_count = 0;

12         int read\_byte = 0;

13         struct input\_event event\_buf[EVENT\_BUF\_NUM];  // 64

14         char \*str\_device = "/dev/input/event0"; // or event1

15

16         int evt\_fd = open(str\_device, O\_RDONLY);

17

18         if(evt\_fd <0)

19         {

20                 printf("touch driver open fail\n");

21         }

22

23         printf("%s opened ... \n", str\_device);

24

25         read\_byte = read(evt\_fd, event\_buf, sizeof(struct input\_event)\*EVENT\_BUF\_NUM);

26

27         for(print\_count ; print\_count < EVENT\_BUF\_NUM ; print\_count++)

28                 printf("type %x  code %x  value %x \n", event\_buf[print\_count].type,

event\_buf[print\_count].code,event\_buf[print\_count].value) ;

29

30         close(evt\_fd);

31         printf("%s closed....",str\_device);

32 }

33

34