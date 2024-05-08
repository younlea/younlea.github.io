# miniconda 사용해서 설치

## cudnn & tensorflow install (using conda)

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

## conda 에서 CUDA 설치하기
```
conda install cudatoolkit

확인
conda list cudatoolkit

```
## tensorflow version 확인
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
## tensorflow vesrion 변경
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
