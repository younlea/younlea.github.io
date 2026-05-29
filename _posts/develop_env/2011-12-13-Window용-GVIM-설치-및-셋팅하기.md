---
title: "Window용 GVIM 설치 및 셋팅하기.."
excerpt_separator: "<!--more-->"
date: 2011-12-13 17:29:26 +0900
categories:
  - develop_env
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

Linux에서 vi를 쓰다보니... 윈도우에서도 가벼게 쓸수 있을것 같아서... 한번 설치 해봤다.

물론... Source Insight나.. Ultra edit가 우리에게는 있지만.. 그래도.. 한번 써보자.. 쓰다보면.. 자꾸 매력이 느껴진다..

1. GVIM 설치

2. Tlist 설치 (파일의 왼쪽에 function list가 나오게 된다.)

3. grep 설치 (grep이 얼마나 막강한지는 다들 아시리라 믿는다 ^^ 파일 검색툴이다.)

4. ctags 설치

5. cscope 설치

**1. GVIM 설치**

<http://www.vim.org/index.php> 에 접속해서 받을수 있다. ^^;

**2. Tlist 설치 (파일의 왼쪽에 function list가 나오게 된다.)**

<http://www.vim.org/scripts/script.php?script_id=273> 에서 아래 파일을 받고..

| package | script version | date | Vim version | user | release notes |
| --- | --- | --- | --- | --- | --- |
| [taglist\_45.zip](http://www.vim.org/scripts/download_script.php?src_id=7701) | **4.5** | *2007-09-21* | 6.0 | *[Yegappan Lakshmanan](http://www.vim.org/account/profile.php?user_id=244)* | Fix an extra space in the check for exctags. Refresh the taglist window  folds after entering a tab. Escape special characters like backslash in  the tag name when saving a session file. Add an internal function to get  and detect file types. |

C:\Program Files\Vim\vim73 아래에 각각 폴더에 카피를 한다.

plugin/taglist.vim - main taglist plugin file \
doc/taglist.txt    - documentation (help) file

이렇게 하고 실행하면 아래와 같은 에러가 난다.. ctags.vim을 설치해 넣었는데도 말이다.

Exuberant ctags (http://ctags.sf.net) not found in PATH.

결국.. 찾다보니.. 아래처럼 처리를 하면.. Tlist가 동작을 하는걸 확인할수 있었다.

<http://ctags.sourceforge.net/> 에서 윈도우용 압축 파일을 받아서

|  |  |
| --- | --- |
| Source and binary for Windows 98/NT/2000/XP | [ctags58.zip](http://prdownloads.sourceforge.net/ctags/ctags58.zip) |

C:\Program Files\Vim\vim73\ctags.exe 에 복사해 넣는다.

**3. grep 설치 (grep이 얼마나 막강한지는 다들 아시리라 믿는다 ^^ 파일 검색툴이다.)**

**\**

**4. ctags 설치**

**\**

**5. cscope 설치**