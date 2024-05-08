---
title: "miniconda "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# miniconda란?
가상환경 기능이 내제된 개발 환경 구축에 용이한것으로 보인다. 
activate 하는 공간에 설치된 것들을 그대로 쓸수 있어서 python package나 개발 패키지 버젼이 다를때 사용이 용이함. 

# miniconda 설치
```
>> Miniconda install <<
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
conda config --set auto_activate_base false
>>reboot

conda create --name=tf python=3.10
conda activate tf

>> cudnn install <<
conda install -c conda-forge cudnn
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

>> tensorflow install <<
conda install tensorflow
python -c "import TensorFlow as tf; print(tf.random.normal([5, 5]))"
conda deactivate
```
[ref link](https://www.cherryservers.com/blog/install-tensorflow-ubuntu)

## conda에서 설치된 package 확인 하는 방법
```
conda list | grep package 
```

## conda에서 package 설치 하는 방법
```
conda install pacage    
ex : conda install cudatoolkit   
ex : conda install tensorflow==2.15.0   
```
## conda activate

## conda deactivate
