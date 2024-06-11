---
title: "github 협업하기 "
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - write here

toc : true
toc_sticky : true
---

# github로 협업하기
1. fork
2. git clone
3. git remote add upstream [fork 한 github 주소]
4. git checkout [작업 branch]
5. code 작성 및 git stash
6. git fetch upstream [작업 branch]
7. git rebase upstream/[작업 branch]
8. git push origin [작업 branch]

## organization에서 작업할때
1. 각자 자신의 저장소로 fork를 한다.
2. local에 clone해서 코드를 받는다.
3. local에서 작업할 곳을 checkout을 이용해서 branch 생성해서 작업한다.
4. 코드 작성후 upstream에 반영을 해야 하는데 이때 아래 절차로 작업을 하면 된다. 
4-1. 해당 fork한 저장소를 git remote를 사용해서 등록한다.
4-2. 작업 코드를 git stash를 사용해서 backup 해 놓는다.
4-3. git fetch upstream [작업 branch]
4-4. git rebase upstream/[작업 branch]
4-5. git push origin [작업 branch]
5. git stash pop으로 backup한 코드를 다시 불러온다.
6. git add. git commit, git push origin [작업 branch]를 사용해서 넣는다.
7. github page에서 PR을 해서 upstream에 넣는다. 
