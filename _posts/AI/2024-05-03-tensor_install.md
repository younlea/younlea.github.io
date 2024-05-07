---
title: "install tensor to  ubuntu "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# Nvidia Graphic Driver 설치
nvidia-smi 

설치가능 버젼 확인     
- ubuntu-drivers devices 

```
younleakim@younleakim-400TEA-400SEA:~$ ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001F82sv0000144Dsd000080B3bc03sc00i00
vendor   : NVIDIA Corporation
model    : TU117 [GeForce GTX 1650]
driver   : nvidia-driver-470-server - distro non-free
driver   : nvidia-driver-545 - distro non-free
driver   : nvidia-driver-535-server - distro non-free
driver   : nvidia-driver-535 - distro non-free recommended    <<<<<<<<<<<<<<<
driver   : nvidia-driver-470 - distro non-free
driver   : nvidia-driver-450-server - distro non-free
driver   : nvidia-driver-535-server-open - distro non-free
driver   : nvidia-driver-545-open - distro non-free
driver   : nvidia-driver-535-open - distro non-free
driver   : nvidia-driver-418-server - distro non-free
driver   : xserver-xorg-video-nouveau - distro free builtin
```
install
sudo apt-get install nvidia-driver-535


# CUDA 설치
## 버젼확인 
nvidia-smi -> CUDA version : 12.2    
[version 다운로드 링크](https://developer.nvidia.com/cuda-toolkit-archive)    
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.2.2/local_installers/cuda-repo-ubuntu2204-12-2-local_12.2.2-535.104.05-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-12-2-local_12.2.2-535.104.05-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-12-2-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
sudo apt install nvidia-cuda-toolkit    
```
```
실행파일로 설치 하는 방법. 
wget https://developer.download.nvidia.com/compute/cuda/12.2.2/local_installers/cuda_12.2.2_535.104.05_linux.run
sudo sh cuda_12.2.2_535.104.05_linux.run
설치후 .bashrc에 아래 코드 추가 하고 source .bashrc 실행해야 함. 
export PATH="/usr/local/cuda-12.2/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-12.2/lib64:$LD_LIBRARY_PATH"
```

최종 설치 확인 : nvcc -V     
```
~$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Aug_15_22:02:13_PDT_2023
Cuda compilation tools, release 12.2, V12.2.140
Build cuda_12.2.r12.2/compiler.33191640_0
```

ref : [blog1](https://sanghyunpark01.github.io/ubuntu/tips/Ubuntu_GDriver/)    
      [blog2](https://sanghyunpark01.github.io/ubuntu/tips/Uubntu_Cuda/)

## Cudnn version 확인
CUDA 버젼에 맞는 cuDNN, python, tensorflow version 확인    
[tensorflow version 확인](https://www.tensorflow.org/install/source?hl=ko#gpu)      
```
버전	           파이썬 버전	컴파일러	     빌드 도구	 cuDNN 쿠다
텐서플로우-2.16.1	3.9-3.12	클랭 17.0.6	바젤 6.5.0	8.9	 12.3
텐서플로우-2.15.0	3.9-3.11	클랭 16.0.0	바젤 6.1.0	8.9	 12.2
```
[Cudnn download](https://developer.nvidia.com/rdp/cudnn-archive)     
> cudnn-local-repo-ubuntu2204-8.9.7.29_1.0-1_amd64.deb

# cudnn & tensorflow install (using conda)

```
>> Miniconda install <<
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
conda config --set auto_activate_base false
>>reboot
conda create --name=tf python=3.10
conda activate tf
>> conda install <<
conda install -c conda-forge cudnn
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
>> tensorflow install <<
-- python3 -m pip install TensorFlow (안된다.)
conda install tensorflow
python -c "import TensorFlow as tf; print(tf.random.normal([5, 5]))"
conda deactivate
```
[ref link](https://www.cherryservers.com/blog/install-tensorflow-ubuntu)

# tensorflow version 확인
```
python3 -c 'import tensorflow as tf; print(tf.__version__)'
pip list | grep tensor

$ conda list | grep tensor
tensorboard               2.15.2             pyhd8ed1ab_0    conda-forge
tensorboard-data-server   0.7.0           py310h52d8a92_0
tensorboard-plugin-wit    1.8.1           py310h06a4308_0
tensorflow                2.15.0          cuda120py310h9360858_3    conda-forge
tensorflow-base           2.15.0          cuda120py310heceb7ac_3    conda-forge
tensorflow-estimator      2.15.0          cuda120py310h549c77d_3    conda-forge

```
# tensorflow vesrion 변경
```
# gpu 버전이 아닌 경우
pip install --upgrade tensorflow==버전
# gpu 버전인 경우
pip install --upgrade tensorflow-gpu==버전

# 업그레이드 예시
pip install --upgrade tensorflow==2.7.0
# 다운그레이드 예시
pip install --upgrade tensorflow==1.15.0

# codna로 업글하니까된다. 
conda install tensorflow==2.15.0

```

# 에러 케이스   
```
>> ssl error case <<
CondaSSLError: Encountered an SSL error. Most likely a certificate verification issue.
>> 해결책
conda config --set ssl_verify false

>> tensorflow 설치 이슈 <<
ERROR: Could not find a version that satisfies the requirement TensorFlow (from versions: none) ERROR: No matching distribution found for TensorFlow
>> 해결책
conda install tensorflow

>> cuda 삭제
sudo apt-get --purge remove 'cuda*'
sudo apt-get autoremove --purge 'cuda*'
```

