---
title: "AVR studio5.1로 Atmega128용 binary build 및 다운로드"
excerpt_separator: "<!--more-->"
date: 2012-07-18 02:23:13 +0900
categories:
  - embedded
tags:
  - develop_env
  - embedded
  - 임베디드

toc : true
toc_sticky : true
---

삽질도 이런 삽질은 ㅜㅜ

어찌됐던 AVR studio5.1을 받아서 설치

new project -> c/c++ -> AVRGCC C Executable Project 선택..

이후 code 넣고 build하면 hex 파일 나온다.

------------------------------------------------------------------------------

serial tool로 char를 보내면 다시 그 받은 값을 보내는 코드임

------------------------------------------------------------------------------

#include <avr/io.h>

#include <avr/interrupt.h>

volatile unsigned char TEMP = 0;

ISR(USART0\_RX\_vect)

{

TEMP = UDR0;

}

void TX0\_char(unsigned char data)

{

while((UCSR0A & 0x20)==0);

UDR0 = data;

}

void TX0\_string(char \* string)

{

while(\*string != '\0')

{

TX0\_char(\*string);

string++;

}

}

int main(void)

{

unsigned char RXD;

UBRR0H = 0;    //난 9600으로 해야 정상 적으로 나온다.

UBRR0L = 51;

UCSR0A = 0x00;

UCSR0B = 0x98;

UCSR0C = 0x06;

RXD = UDR0;

sei();

TX0\_string("TEST~~~~ ");

TX0\_char(0x0D);

TX0\_char(0x0A);

while(1)

{

if( TEMP != 0){

TX0\_char(TEMP);

TX0\_char(0x0D);

TX0\_char(0x0A);

TEMP = 0;

}

}

}

------------------------------------------------------------------------------

ponyprog2000으로 다운받으면 됨..

시간이 없어서 그림은 안붙인다.