---
title: "printf color 사용법"
excerpt_separator: "<!--more-->"
date: 2013-01-04 14:27:49 +0900
categories:
  - develop_env
tags:
  - algorithm
  - 알고리즘

toc : true
toc_sticky : true
---

그냥 Tree를 만들어 보려고 하다가 색깔을 넣어야 해서... 찾다보니...

printf에 기능이 있다... 대단..

printf를 사용해서 특정 색깔을 출력을 할 경우.. 아래 방법을 사용하면 된다.

요약하면.. 아래 세 스텝으로 하면 된다.

**색 설정.**

printf(%c[%d;%d;%dm",0x1B, 특성, 글자색, 바탕색);

(attribute);(fore color);(background color)

printf를 사용해서 **글자 출력...**

**\**

**색 설정 clear.**

printf("%c[%dm", 0x1B, 0);

출처 : http://cc.byexamples.com/2007/01/20/print-color-string-without-ncurses/

Printing color strings uses only printf, without any alternative libraries such as ncurses? Yes, we can do that in any unix based operating system that support ANSI codes, I am not sure whether it works on either windows or mac, have no chance to try at the moment.

By feeding some magic characters to printf, you can manipulate your fore color, background color and even attributes. Lets look at a simple example to print “Hello World” in Bright Red with background black.

#define BRIGHT 1

#define RED 31

#define BG\_BLACK 40

printf("%c[%d;%d;%dmHello World", 0x1B, BRIGHT,RED,BG\_BLACK);

Okay, the code above print the string with bright red but the color setting will remain, to reset back the color to default, refers the code as bellow:

printf("%c[%dm", 0x1B, 0);

The fore color, background color and attributes codes are shown as bellow

Text attributes

0    All attributes off

1    Bold on

4    Underscore (on monochrome display adapter only)

5    Blink on

7    Reverse video on

8    Concealed on

Foreground colors

30    Black

31    Red

32    Green

33    Yellow

34    Blue

35    Magenta

36    Cyan

37    White

Background colors

40    Black

41    Red

42    Green

43    Yellow

44    Blue

45    Magenta

46    Cyan

47    White

How’s the color magic works? 0x1B is a special code that used to do all the color settings. 0x1B is hex code equivalent to decimal 27. With character(27) and a open square blacket “[“, initiate the setting. The rest of the values are (attribute);(fore color);(background color) and it ends the setting with ” m “. So entire thing will be look like this:

printf("%c[%d;%d;%dm",27,1,33,40);

With that the rest of your print line will be in bright yellow with background color black.

I have transparent background, What if I want my default background instead of any color?

Simple, ignore the background color, How?

printf("%c[%d;%dmHello World%c[%dm\n",27,1,33,27,0);

The line above will print bright yellow “Hello World” with default bg color.

\