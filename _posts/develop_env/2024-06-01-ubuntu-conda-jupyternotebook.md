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
# conda init
```
# conda 명령어를 사용해 가상환경 만들기
conda create -n 만들고자하는이름 python=자신이원하는버전(ex, 3) 
```

# conda 만들어진지 확인
```
conda info --envs
```

# conda activate
```
conda activate 만들고자하는이름
```

# jupyter notebook 실행
```
jupyter notebook
```
# 가상환경에서 필요한 패키지 설치하기
```
필요한 패키지 
3.7.4 (tags/v3.7.4:e09359112e, Jul  8 2019, 20:34:20) [MSC v.1916 64 bit (AMD64)]
pandas 0.25.1
numpy 1.18.5
sklearn 0.21.3
scipy 1.5.2
mlxtend	0.15.0.0
statsmodels 0.11.1
xgboost	0.80

인스톨 
conda install pandas=0.25.1
conda install numpy=1.18.5
이거 아니다 ㅡ.,ㅡ; ///////conda install sklearn=0.21.3
conda install scikit-learn=0.21.3
conda install scipy=1.5.2
conda install statsmodels=0.11.1
conda install -c conda-forge mlxtend
conda install conda-forge::xgboost=0.80
conda install anaconda::seaborn
```



# jupyter notebook  커널 연결
```
conda install ipykernel

현재 커널(jupyternotebook)으로 커널 연결
python3 -m ipykernel install --user --name jupyternotebook --display-name ds3
```

# conda 명령어를 사용해 가상환경 비활성화
```
conda deactivate
```

# conda 명령어를 사용해 가상환경 삭제하기
```
conda remove --name 지울가상환경이름 --all
```


