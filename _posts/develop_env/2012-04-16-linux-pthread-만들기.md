---
title: "linux pthread 만들기~~"
excerpt_separator: "<!--more-->"
date: 2012-04-16 21:47:18 +0900
categories:
  - develop_env
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

linux application 관련 내용은 자주 사용하는 thread에 대해서 구현을 해보았다.

program 설명.

thread를 생성하고 저장되어 있는 값이 thread를 돌면서 바뀌고 thread가 끝나면 program도 종료한다.

//-------------------------------------------------------------------------------------

1 // test thread

2 #include <string.h>

3 #include <stdio.h>

4 #include <pthread.h>

5

6

7 char test[13]="before test";

8

9 pthread\_t threads;

10

11 void \*thread\_test(void \* arg);

12

13

14 void main(void)

15 {

16         int i=10;

17         int rc;

18         int status;

19

20         printf("start test thread\n");

21

22         printf("main pid =  %d \n", getpid());

23

24         printf("before thread create :[test][%s] \n", test);

25

26         /- int pthread\_create(pthread\_t \*thread, const pthread\_attr\_t \*attr,

27         \*                 void \*(\*start\_routine) (void \*), void \*arg);

28         \*-

29         rc = pthread\_create(&threads, NULL, &thread\_test, (void \*)i);

//threads를 thread\_test 함수로 연결하고인자 i를 넘긴다.

30

31         printf("after thread create: test[%s] \n ", test);

32

33         rc = pthread\_join(threads, (void\*\*)&status);

//threads가 끝나길 기다리고 status로 인자값을 받는다.

34

35         printf("get thread join: test[%s] %d \n", test, status);

36

37 }

38

39 void \*thread\_test(void \* arg)

40 {

41         printf("thread pid = %d \n", getpid());

42         printf("in thread\_test received arg : %d\n", (int)arg);

43

44         strcpy(test, "Wow~~");

45

46         pthread\_exit((void \*)12);  //pthread\_joid 에서 받는 값을 인자로 넣어주고 끝낸다.

47 }

//-------------------------------------------------------------------------------------

build : gcc -g thread.c -lpthread

result

$ ./a.out

start test thread

main pid =  9122

before thread create :[test][before test]

after thread create: test[before test]

thread pid = 9122

in thread\_test received arg : 10

get thread join: test[Wow~~] 12

trouble shooting.

gcc complile시 아래 에러가 날 경우... -lpthread 옵션을 줘서 빌드를 하면 해결된다. ^^;

undefined reference to `pthread\_create'

undefined reference to `pthread\_join'

gcc -g thread.c -lpthread