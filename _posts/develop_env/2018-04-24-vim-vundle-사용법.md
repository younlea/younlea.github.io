---
title: "vim vundle 사용법"
excerpt_separator: "<!--more-->"
date: 2018-04-24 09:40:10 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

vim 확장 기능들을 쓰기 위해서 vundle을 쓰게 되었습니다.

관련 정리는 아래 링크에서 참고 하였습니다. \

vim vundle란 : <https://kldp.org/node/125263>

유요한 vundle : <https://bluesh55.github.io/2016/10/09/vim-ide/>

vundle 설치

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

~/.vimrc 에 아래 내용 추가

```
set nocompatible              " be iMproved, required
filetype off                  " required
 
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
 
" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
 
call vundle#end()            " required
filetype plugin indent on    " required
```

추가하고 싶은 plugin에 대해서 위 Plugin 아래에 추가 내용 추가하고 vim 실행후 :PluginInstall 을 실행하면 설치됨

```
set nocompatible              " be iMproved, required
filetype off                  " required
 
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
 
" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
 
" 추가
Plugin 'vim-airline/vim-airline'
 
call vundle#end()            " required
filetype plugin indent on    " required
```

해당 plugin을 지우기 위해서는  추가한 plugin을 .vimrc에서 삭제하고 :PluginClean을 실행하면 된다.

좀 더 자세한 내용은 :help vundle 로 확인 바랍니다.

추가로 vim-airline의 경우 셋팅이 필요합니다.

~/.vimrc 에 아래 항목 추가

```
" for vim-airline
let g:airline#extensions#tabline#enabled = 1 " turn on buffer list
set laststatus=2 " turn on bottom bar
```

20180621 : 추가Plugin 'scrooloose/nerdtree'  "파일 브라우져Plugin 'ctrlpvim/ctrlp.vim'   "파일 찾기  ctrl+p\