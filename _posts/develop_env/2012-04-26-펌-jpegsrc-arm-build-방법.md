---
title: "[펌] jpegsrc arm build 방법"
excerpt_separator: "<!--more-->"
date: 2012-04-26 16:37:30 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

우선 jpeg소스를 받아서 압축해제 후 configure후에 컴파일... 카피 끝..

#wget http://www.ijg.org/files/jpegsrc.v6b.tar.gz

#tar xvfz jpegsrc.v6b.tar.gz

#cd jpeg-6b

#CC=arm-linux-gcc ./configure --host=arm-linux --build=i686 --prefix=/컴파일러경로

</usr/local/arm/4.2.2-eabi/usr/>  <<===  본인의 경우는 여기에 넣었다.

#make

#make install

끝...

위에서 --prefix=/ 이 부분에는 크로스컴파일러 경로를 적어주어서...

make install시 카피가 되게 설정합니다...

그리고 마지막으로 headerfile과 라이브러리 직접 카피를 해줘야할듯...

(위에서 make install하면 bin/ man/ 파일만 카피가... ㅡㅡ'')

#cp \*.h /크로스컴파일러/include

#cp libjpeg.a /크로스컴파일러/lib