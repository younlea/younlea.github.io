---
title: "enum to string"
excerpt_separator: "<!--more-->"
date: 2011-01-31 16:05:04 +0900
categories:
  - develop_env
tags:
  - etc

toc : true
toc_sticky : true
---

**우선 아래 방법은 변수로 해서 전체를 바꾸는건 불가능. ㅜㅜ**

**다른 방법을 찾아봐야 것다. 에휴.**

갑자기 enum 을 string으로 바꿀수 있을까? 라는 생각이 갑자기 든다..

모 결국.. C에서도 가능하다라는게 답이다.

사용 방법은 #define 문이다.. 흐흐

**#define f(x) #x   << 요렇게 하면 x에 값을 넣으면 그 enum값이 string으로 나오게 된다.**

위 #는 매크로 인자를 문자열로 바뀌주는 역할을 한다.

예제는 아래와 같다..

#include <stdio.h>

**#define f(x) #x**

typedef enum

{

test1,

test2,

test3

}enum\_test;

void main(void)

{

**char \* test;**

**test = f(test1);**

printf("%d \n",test1);

printf("%s \n",test1);

**printf("%s \n",test);**

}

결과값은 아래와 같다..

0

(null)

test1

Press any key to continue

이 기능이 왜 필요할까??

흠.. 요세 dll을 만드는데 관련해서 header는 그대로인데 간혹가다 dll을 가져다 쓰는 팀에서 enum값에 추가를 해버려서

나중에 동일한 header로 처리가 불가능하게 된다..

그래서 아예 파서를 만들어 놓고 dll안에서 외부에서 준 string을 enum값으로 파싱할때 쓰면 좋을것 같아서 찾아봤다.

아웅.. 재밌다..^^; 그런데 string을 enum으로는 어떻게 바꾸지?

모 이건 enum 선언되어 있으니까 비교해서 넣어주면 될것같군.. 우선 패스.. ^^;

자.. 새로운 방법?? 은 아닌것 같지만.. 조금 이쁘게 바꿀수는 있을것 같다.

typedef enum

{

one,

two

}number;

char \*num[2] = { "one", "two"}

좀 지저분 해지것지만 그래도 switch case 문을 남발하는것 보다는 나을듯 한다..

어찌됏던 배열 포인터를 사용해서 string을 넣고.

관련 enum string이 넘어오면 비교하는 구문을 작성하면 될듯하다..

대충짜면 아래랑 같은디..

#include <stdio.h>

#include <string.h>

typedef enum

{

one,

two,

three,

four

}number;

#define f(x) #x

int String\_Counter(char \* str)

{

for(int i = 0; i<255 ; i++)

{

if(\*str++ == NULL)

return i+1;

}

return i;

}

void main(void)

{

char \*temp1[4] = {"one",

"two",

"three",

"four"};

char \* before = "two";

for(int i =0 ; i < 4 ; i++)

{

if(!memcmp(before, temp1[i], String\_Counter(before)\*sizeof(char)))

{

printf("%d \n",i);

}

}

}

흠. 그냥 enum 안에 있는 값을 자동으로 string으로 바꿔주는건 없을까??

C#에는 있다는데.. ㅜㅜ

좀더 찾아보자고.. 아우.

\