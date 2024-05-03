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
```

최종 설치 확인 : nvcc -V     


ref : [blog1](https://sanghyunpark01.github.io/ubuntu/tips/Ubuntu_GDriver/)    
      [blog2](https://sanghyunpark01.github.io/ubuntu/tips/Uubntu_Cuda/)


# tensorflow install

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
[ref link](https://www.cherryservers.com/blog/install-tensorflow-ubuntu)
