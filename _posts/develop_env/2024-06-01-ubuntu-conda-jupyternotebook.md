---
title: "우분투에서 콘다를 사용해서 jupyter notebook 실행"
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - jupyternotebook

toc : true
toc_sticky : true
---

unbuntu에서 쥬피터 노트북을 설정하고 데싸 공부하려는데.. 
python 버젼 관리랑 시험장 버젼을 맞추기 위해 conda로 가상환경을 꾸며서 돌리려고 한다.

# conda 설치
```
pip install conda
```
# jupyter notebook 설치
```
sudo apt install jupyter-notebook
```
# coda init
```
# conda 명령어를 사용해 가상환경 만들기
conda create -n 만들고자하는이름 python=자신이원하는버전(ex, 3) 
```

# conda activate
```
conda activate
```

# jupyter notebook 실행
```
jupyter notebook
```
