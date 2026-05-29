---
title: "rand 함수 와 srand 함수.."
excerpt_separator: "<!--more-->"
date: 2011-06-27 22:48:22 +0900
categories:
  - dataScience
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

음.. 어쩌다 보니 특정 값에 대한 난수를 만들어야 하는 일이 생겼다.

stdlib.h에 선언되어 있는 srand와 rand함수를 사용해서 만들수 있을것으로 보인다.

흠.. rand 함수를 만들기는 시간이 없으니 패스를 하고ㅡ.ㅡ;

rand 함수는 특별한 random 변수sheet를 가지고 있고 이곳에서 데이터를 가지고 온다고 한다.

그리고 srand(int)는 rand 함수에서 사용하는 기준 점을 셋팅한다고 보면 된다.

궁굼한가 그럼 아래처럼 짜보자.. 해보면 알것지만... 모.. 계속 똑같은 값이 나온다.

#include <stdio.h>

#include <stdlib.h>

void main()

{

int temp = 0;

srand(temp);

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

printf("%d \n",rand());

}

결과값.

38

7719

21238

2437

8855

11797

8365

32285

10450

30612

rand 함수의 return값은 1~32767 까지이다..

srand에 들어가는 변수값은 모.. 123123131313까지 넣어도 나온다 ㅡ.ㅡ 요거 잘만들었네..

자 여기서 temp 값이 각각 HW의 고유의 값이라면 그 HW의 고유 MAC address를 random함수를 사용해서 만들수 있을것같다.

간단하게 srand(고유값);

a= rand()%0xff

b= rand()%0xff

c= rand()%0xff

d= rand()%0xff

e= rand()%0xff

f= rand()%0xff

a,b,c,d,e,f 를 연달아서 string cat 해버리면 모... 대충 나올것같다.

모 두개를 만들어야 하면..

초기화 할때.. 나머지 6개도 후다닥 같이 만들어 주면 될것같다 ^^;

아. 첫번째 a는....

a = rand()%0xff;

a = a & 0xfe; 요건 multibyte 대문에 제일 끝 1bit를 짤라내는 거란다.

a =  a | 0x2;  요건 align을 맞추기 위해 끝에서 두번째 bit를 무조건 1로 셋팅하는것 같다.

이렇게 해야한다는것 같다..

흠 대충 아래와 같이 짜도 되것다..

#include <stdio.h>

#include <stdlib.h>

void main()

{

int temp = 0;

int i = 0;

int MacAddress[6]={0,};

char MacAdd[6]={0,};

srand(temp);

for(i =0; i < 6; i++)

{

if(i == 0)

{

MacAddress[i] = (rand() % 0xff);

MacAddress[i] = MacAddress[0] & 0xfe;

MacAddress[i] = MacAddress[0] | 0x2;

}else

{

MacAddress[i] = rand()%0xff;

}

}

for(i = 0; i < 6; i++)

printf("MacAddress[%d] = %x \n",i ,MacAddress[i]);

for(i = 0; i < 6; i++)

itoa(MacAddress[i], MacAdd + i\*2, 16);

printf("MACAddress : %s \n",MacAdd);

}

결과값

MacAddress[0] = 26

MacAddress[1] = 45

MacAddress[2] = 49

MacAddress[3] = 8e

MacAddress[4] = b9

MacAddress[5] = 43

MACAddress 2645498eb943

흠.. 내일 가서 좀더 다듬으면 끝나것네.. 오늘 할꺼 하나 끝 ㅡ.ㅡv

아웅.. 다른거 하나는 언제하쥐?

흠 위에처럼 하면 f의 경우 f로 붙는다 ㅡ.ㅡ 흠냐.. 그래서. 아래처럼 바꿨다.

#include <stdio.h>

#include <stdlib.h>

void main()

{

int temp = 2;

int i = 0;

int MacAddress[6]={0,};

char MacAdd[12]={0,};

srand(temp);

for(i =0; i < 6; i++)

{

if(i == 0)

{

MacAddress[i] = (rand() % 0xff);

MacAddress[i] = MacAddress[i] & 0xfe;

MacAddress[i] = MacAddress[i] | 0x2;

}else

{

MacAddress[i] = rand()%0xff;

}

}

for(i = 0; i < 6; i++)

printf("MacAddress[%d] = %x \n",i ,MacAddress[i]);

for(i = 0; i < 6; i++)

sprintf(MacAdd + i\*2,"%.2x",MacAddress[i]);

printf("%s \n",MacAdd);

}

흠냐 결국 또 삽질을 한게 되더군 ㅡ.ㅡ;

이래서 기초가 중요한거구나 라는 생각이 든다 ㅡㅜ

결국 아래와 같이 했다 ㅡ.ㅡ;

#include <stdio.h>

#include <stdlib.h>

void main()

{

int temp = 2;

int i = 0;

unsigned char MacAddress[6]={0,};

srand(temp);

for(i =0; i < 6; i++)

{

if(i == 0)

{

MacAddress[i] = (rand() % 0xff);

MacAddress[i] = MacAddress[i] & 0xfe;

MacAddress[i] = MacAddress[i] | 0x2;

}else

{

MacAddress[i] = rand()%0xff;

}

}

}

결국 MAC address에 들어가는 주소는 MacAddress 변수하나하나가 된다 ^^;

아구.. 기초부터 다시 해야할듯 ㅡㅡ;