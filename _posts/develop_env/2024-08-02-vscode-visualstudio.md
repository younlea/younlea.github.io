---
title: "ubuntu에서 vscode로 visual studio처럼 개발하기 "
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - write here

toc : true
toc_sticky : true
---

# ubuntu에 visual studio code 받기

# code실행 후 셋팅

# 빌드 위해서 파일 생성
terminal -> configure default build task -> other -> 아래 파일 생성. 
## tasks.json
```json
{
    "version": "2.0.0",
    "runner": "terminal",
    "type": "shell",
    "echoCommand": true,
    "presentation" : {"reveal": "always" },
    "tasks": [
        // C++
        {
            "label": "save and compile for C++",
            "command": "g++",
            "args": [
                "-g3",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "group": "build",

            "problemMatcher": {
                "fileLocation": [
                    "relative",
                    "${workspaceRoot}"
                
                ],
                "pattern":{
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        },
        // C
        {
            "label": "save and compile for C",
            "command": "gcc",
            "args": [
                "-g3",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "group": "build",

            "problemMatcher" : {
                "fileLocation": [
                    "relative",
                    "${workspaceRoot}"
                
                ],
                "pattern":{
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        },
        {
            "label": "execute",

            "command": "cd ${fileDirname} && ./${fileBasenameNoExtension}",

            "group": "test"
        }
    ]
}
```

# 디버깅 위해서 파일 입력
## launch.json
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```

[ref1](https://choice-life.tistory.com/11)    
[ref2](https://yjcode.tistory.com/1?category=811393)
