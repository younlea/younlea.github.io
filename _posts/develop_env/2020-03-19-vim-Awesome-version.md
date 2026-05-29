---
title: "vim Awesome version"
excerpt_separator: "<!--more-->"
date: 2020-03-19 06:42:46 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

vim 한번 설치하면 그냥 쓰던거 쓰는데.. 하나로 이쁘게 모아준 버젼이 있네 . ^^;

<https://github.com/amix/vimrc>

## How to install the Awesome version?

### Install for your own user only

The awesome version includes a lot of great plugins, configurations and color schemes that make Vim a lot better. To install it simply do following from your terminal:

**git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim\_runtime**

**sh ~/.vim\_runtime/install\_awesome\_vimrc.sh**

스타일을 바꾸고 싶을때~~~

**:colorscheme darkblue**

![image](/assets/images/posts/6620729/c0028014_5e7533766e86b.png)

file 찾기 : Ctrl+f

이전에 열었던 파일 보기 : mru

tree 보기 : NERD

![image](/assets/images/posts/6620729/c0028014_5e75345f00860.png)

**엄. Tlist 가 없는듯... 설치하자.**

git에서 받고 사용하던데로 :tlist를 호출하면 된다.

pi@raspberrypi:**~/.vim\_runtime $ git clone https://github.com/vim-scripts/taglist.vim.git my\_plugins/taglist**

Cloning into 'my\_plugins/taglist'...

remote: Enumerating objects: 120, done.

remote: Total 120 (delta 0), reused 0 (delta 0), pack-reused 120

Receiving objects: 100% (120/120), 108.01 KiB | 0 bytes/s, done.

Resolving deltas: 100% (30/30), done.

좋다.. 흐흐흐

<https://vimawesome.com/>  요런곳도 있구요.

![image](/assets/images/posts/6620729/c0028014_5e752e1235de4.png)