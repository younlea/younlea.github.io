---
title: "git diff 및 vimdiff"
excerpt_separator: "<!--more-->"
date: 2012-12-12 17:06:45 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

git diff 를 vimdiff로 열수 있도록 setting 하는 방법

----------------------------------------------------------------------------

[출처]http://technotales.wordpress.com/2009/05/17/git-diff-with-vimdiff/

----------------------------------------------------------------------------

Step 1: add this to your .gitconfig

----------------------------------------

[diff]

external = git\_diff\_wrapper

[pager]

diff =

----------------------------------------

Step 2: create a file named git\_diff\_wrapper, put it somewhere in your $PATH

----------------------------------------

#!/bin/sh

vimdiff "$2" "$5"

----------------------------------------

I still have access to the default git diff behavior with the --no-ext-diff flag. Here’s a function I put in my bash configuration files:

----------------------------------------

function git\_diff(){

git diff --no-ext-diff -w "$@" | vim -R -

}

----------------------------------------

[--no-ext-diff : to prevent using vimdiff]

[-w : to ignore whitespace]

[-R : to start vim in read-only mode]

[- : to make vim act as a pager]

----------------------------------------

----------------------------------------------------------------------------

git으로 commit id 안넣고 head에서 비교하게 하는 방법.

# Current branch vs. parent

git diff HEAD^ HEAD

# Current branch, diff between commits 2 and 3 times back

git diff HEAD~3 HEAD~2

Prior commits work something like this:

# Parent of HEAD

git show HEAD^1

# Grandparent

git show HEAD^2

There are a lot of ways you can specify commits:

# Great grandparent

git show HEAD~3

#GIT 수정된 파일 이름만 보여주기

git diff HEAD^ HEAD --name-only     (HEAD대신 commit id를 넣어줘도 그 id 기준으로 비교가 가능하다.)

#GIT 지정한 파일만 수정된 내용 보여주기

git diff HEAD^ HEAD xxx.c

#GIT 특정 버젼에서 특정 파일들만 수정된 내용 보여주기

git diff HEAD^ HEAD $(find -name \*.c)  << c파일 수정된 것들만 비교해서 보여주기

gitk : GUI

reference.

http://progit.org/book/ (most chapters; it explains the model behind Git and answers most of typical questions)

and then immediately http://gitready.com/ (usage tips).

vimdiff 모두 닫기.

ctrl-w c - Close this window

ctrl-w o - close all Other windows (mnemonic - Only)

:qall

vimdiff 같은 부분 펼치기  z + o

vimdiff 같은 부분 접기 z + c

\