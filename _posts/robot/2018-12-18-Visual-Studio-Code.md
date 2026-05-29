---
title: "Visual Studio Code"
excerpt_separator: "<!--more-->"
date: 2018-12-18 00:05:24 +0900
categories:
  - robot
tags:
  - robot
  - 로봇

toc : true
toc_sticky : true
---

Visual Studio Code에서 간단히 C언에 빌드하고 디버깅 하는 방법...

일단 왼쪽 제일 아래 다섯번째 버튼을 눌러서 C빌드 관련 패키지를 받는다. C/C++ 관련 검색하면 나온다. ^^

이후  간단한 코드를 생성하고

#include <stdio.h>

int main(void)

{

int a = 0;

int b = 1;

printf("%d %d", a,b);

}

빌드를 위해서 tasks.json 아래와 같이 추가해 주면 된다.

추가 방법은 Terminal -> Configure Default Build Task를 누르고 수정해 주면 된다.

{

// See https://go.microsoft.com/fwlink/?LinkId=733558

// for the documentation about the tasks.json format

"version": "2.0.0",

"tasks": [

{

"label": "build gcc",

"type": "shell",

"command": "gcc",

"args": [

"${file}",

"-g",

"-o",

"a.out"

],

"group": {

"kind": "build",

"isDefault": true

},

"problemMatcher": [

"$gcc"

]

}

]

}

보시면 아시겠지만.  gcc compile관련해서 스크립트화 한것으로 보인다. ^^

저기 -g 옵션은 디버깅 심볼을 넣기 위해서 넣은 것이다.

이후 디버깅을 위해서는 디버그 버튼을 누르면 브레이크 포인트 까지 가게 되는데 그때도 아래 json file을 추가적으로 수정해 줘야 한다.

launch.json

{

// Use IntelliSense to learn about possible attributes.

// Hover to view descriptions of existing attributes.

// For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387

"version": "0.2.0",

"configurations": [

{

"name": "(lldb) Launch",

"type": "cppdbg",

"request": "launch",

"program": "${workspaceFolder}/a.out",

"args": [],

"stopAtEntry": false,

"cwd": "${workspaceFolder}",

"environment": [],

"externalConsole": true,

"MIMode": "lldb"

}

]

}

위 내용을 봐서도 아시겠지만 program에서 나오는 파일만 위 컴파일러에 설정한것과 맞춰만 주었다.

움 인터넷 찾아보면 셋팅하는 방법들이 많이 나와 있기는 한데.. 간단히 셋팅하는 방법이 없어서 간략히 적어 보았다.

사용법은 좀더 익숙해져야 겠지만 그래도 모든 OS에서 지원해 주니까.. 슬슬 숙달을 해야 할듯 하다. ^^

\