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

최종 설치 확인 : nvcc -V     


ref : [blog1](https://sanghyunpark01.github.io/ubuntu/tips/Ubuntu_GDriver/)    
      [blog2](https://sanghyunpark01.github.io/ubuntu/tips/Uubntu_Cuda/)


# tensorflow install

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

```

