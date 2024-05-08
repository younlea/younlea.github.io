---
title: "write here "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---


# Target
UBUNTU 22.04
CUDA = 11.08
CUDNN = 8.6.0
Tensorflow = 2.12.0

# tensorflow install
pip3 install tensorflow==2.12.0

# CUDA install
[linstall](https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=runfile_local)
'''
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run
'''
/usr/local/cuda-11.8 에 압축이 풀림
/usr/local/cuda에 심볼릭 링킹됨. 

vim ~/.bashrc 아래 추가
>> export PATH="/usr/local/cuda-11.8/bin:$PATH"
 export LD_LIBRARY_PATH="/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH"
source ~/.bashrc


nvcc -V로 버젼 확인 가능 함. 

# CUDNN install
[download link](https://developer.nvidia.com/rdp/cudnn-archive)    
-> Download cuDNN v8.6.0 (October 3rd, 2022), for CUDA 11.x
->> Local Installer for Linux x86_64 (Tar) 

[install guide](https://docs.nvidia.com/deeplearning/cudnn/archives/cudnn-811/install-guide/index.htm)   l
'''
2.3.1. Tar File Installation
Before issuing the following commands, you'll need to replace x.x and v8.x.x.x with your specific CUDA and cuDNN versions and package date.
Procedure
Navigate to your <cudnnpath> directory containing the cuDNN tar file.
Unzip the cuDNN package.
$ tar -xvf cudnn-linux-x86_64-8.6.0.163_cuda11-archive.tar.xz cuda

Copy the following files into the CUDA Toolkit directory.
$ sudo cp cuda/include/cudnn*.h /usr/local/cuda/include 
$ sudo cp -P cuda/lib/libcudnn* /usr/local/cuda/lib64 
$ sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
'''

