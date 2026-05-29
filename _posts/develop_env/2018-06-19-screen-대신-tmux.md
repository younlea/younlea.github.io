---
title: "screen 대신 tmux ??"
excerpt_separator: "<!--more-->"
date: 2018-06-19 09:45:03 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

콘솔에서 screen을 써왔는데.. tmux 라는 녀석이 있어서 알아보고 있습니다 ^^ (20180619)

일단 설치는 간단합니다

sudo apt-get install tmux

그리고 tmux를 실행하면 되는데 screen prefix가 익숙한 저로서는 ctrl+b를 ctrl+a로 바꾸고 싶더군요..

이리저리 찾아보니... [<https://gist.github.com/shinzui/866897>]에 많은 셋팅중 몇줄만 쓰면 되더군요 ^^;

vim ~/.tmux.conf

-----------------------------------------------

unbind-key C-b

set-option -g prefix C-a

bind-key C-a send-prefix

bind-key C-a last-window

-----------------------------------------------

위와 같이 실행하면 이쁜 콘솔창이 뜨게 됩니다. ^^; 좀더 사용해 보면서 업데이트 하도록 할께요~~

(아.. tmux가 이전에 실행되고 있으면 설정이 먹지 않더군요 ㅜㅜ tmux ls로 열린 tmux가 있는지 확인후 죽이고 하시면 됩니다 )

참고 : <https://wiki.archlinux.org/index.php/tmux#Key_bindings>

tmux 화면 분할.

screen과 비슷하면서 다른. ^^;

세로 분할 : prefix + %     (:split -h)

가로 분할 : prefix + "       (:split)

화면 최대화 및 최소화 : prefix + z

panel name 변경 : prefix + ,

window name 변경 : prefix + $

panel 닫기 : prefix + x

화면 분할된 상태 이동 : prefix + 화살표

화면 분할된 창 크기 변경 : prefix + ctrl + 화살표

tmux plug in : <https://github.com/tmux-plugins>

유용한 plugin ([참고](https://madforfamily.com/post/%EB%82%B4%EA%B0%80-%EB%A6%AC%EB%88%85%EC%8A%A4%EB%A5%BC-%EC%8D%A8%EC%95%BC%EB%A7%8C-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-3-tmux.html) )

```
mkdir -p ~/.tmux/plugins/
cd ~/.tmux/plugins/
git clone https://github.com/tmux-plugins/tmux-yank
git clone https://github.com/tmux-plugins/tmux-resurrect
git clone https://github.com/tmux-plugins/tmux-continuum
git clone https://github.com/tmux-plugins/tmux-sessionist
git clone https://github.com/tmux-plugins/tmux-logging
```

mouse로 크기 조절 (아래 명령어를 치면 동작합니다. tmux v2.1이후)

set -g mouse on

\