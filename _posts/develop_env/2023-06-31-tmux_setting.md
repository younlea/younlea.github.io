---
title: "tmux 초기 설치후 prefix 값 변경 "
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - tmux

toc : true
toc_sticky : true
---

# tmux 설치후 ctrl+b 를 ctrl+a로 변경
tmux에서 기본 키 바인딩을 "Ctrl + B"에서 "Ctrl + A"로 변경하려면 tmux.conf 파일을 수정해야 합니다. 방법은 다음과 같습니다.  

터미널을 열고 다음 명령을 입력하여 텍스트 편집기에서 tmux.conf 파일을 엽니다.  
```
vi ~/.tmux.conf
```
파일이 없으면 생성됩니다. 그렇지 않으면 기존 파일을 편집합니다.  
파일에 다음 줄을 추가하여 접두사 키를 "Ctrl + B"에서 "Ctrl + A"로 다시 매핑합니다.  
```
set-option -g prefix C-a
```
파일을 저장하고 텍스트 편집기를 종료합니다.  
변경 사항을 적용하려면 tmux 세션을 다시 시작하거나 터미널에서 다음 명령을 사용하여 tmux.conf 파일을 다시 로드하십시오.  
```
tmux source-file ~/.tmux.conf
```
이 단계 후에 "Ctrl + B" 대신 tmux의 접두사 키로 "Ctrl + A"를 사용할 수 있어야 합니다.  
