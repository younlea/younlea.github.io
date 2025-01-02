---
title: "stable diffusion 집에 설치해서 외부에서 접속하기 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - stable_diffusion

toc : true
toc_sticky : true
---

# stable diffusion local 설치 및 외부 접속하기.  

그래픽 카드가 구려서 cpu Only로 로 동작.   

아래옵션으로 실행하고 외부에서 7860 으로 포트포워딩해서 들어옴. 
```
 ./webui.sh --skip-torch-cuda-test --listen --precision full --no-half   
```

```
login mode : --gradio-auth username:password 추가
```

conda에 torch랑 cuda 설치 하는 방법    
```
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
```


> --skip-torch-cuda-test : GPU가 1070정도라서 GPU 사용자하지 않게 설정   
> --listen : 외부망에서 접근 가능하도록 셋팅    
> --precision full --no-half : 어떤에러가 났었는데.. 그거 해결    

![image](https://github.com/younlea/younlea.github.io/assets/1435846/bcfa6f9e-9427-4a37-9aff-a9e2c697c6be)

외부에서 7860으로 포트포워딩해서 들어옴.    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/8f75363c-034e-43f0-a5e3-c50ad1dabb2e)

civitai에서 checkpoint와 vae 받아서 설치    

예제 참고해서 실행    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/6d3abf67-a102-4200-b550-258a0c216076)

진행중.. (시간이 오래 걸린다)    
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d1f226d9-0f98-4c9c-be2a-5031cb843547)

vae 적용 전.
![image](https://github.com/younlea/younlea.github.io/assets/1435846/d7e9c919-1ae1-4adb-9813-5d25433f831a)



### GPU 4070 있는 경우
ID 추가    
```
./webui.sh --listen --enable-insecure-extension-access --gradio-auth kibwa:kibwa --port 7860   
```
특정 포트 열기   
```
./webui.sh --listen --enable-insecure-extension-access --port 7865   
```
