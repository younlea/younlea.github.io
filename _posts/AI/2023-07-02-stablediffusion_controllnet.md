---
title: "stable diffusion에 controlnet 설치하기 "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - stablediffusion
  - controlnet

toc : true
toc_sticky : true
---

# stable diffusion에서 controlnet 사용하기
webui에서 아래 기능들을 사용할수 있다. 
[controlnet github](https://github.com/Mikubill/sd-webui-controlnet)
[openpose github](https://github.com/fkunn1326/openpose-editor)


## 설치
![image](https://github.com/younlea/younlea.github.io/assets/1435846/a7e12c2a-0a7d-4488-b888-59b70ece7252)
install내용에 아래 github 주소를 넣는다.   
```
https://github.com/Mikubill/sd-webui-controlnet
```
![image](https://github.com/younlea/younlea.github.io/assets/1435846/e058782a-b5c8-4556-9bb7-1bde87afb00d)

인스톨 완료...   
![image](https://github.com/younlea/younlea.github.io/assets/1435846/8205a52c-681a-49bd-85de-cd3888a33aa6)

```
error : AssertionError: extension access disabled because of command line flags
해결책 : --enable-insecure-extension-access (실행시 추가)
```
재시작.. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/ead6881f-09bc-4e0f-9105-c3ff3a837ee2)

아래처럼 새로 생긴게 확인이 됩니다. 
![image](https://github.com/younlea/younlea.github.io/assets/1435846/8610d6ba-4fd4-4a1d-8c0a-21523b3afcdb)

![image](https://github.com/younlea/younlea.github.io/assets/1435846/4c5f2cc2-5483-42b9-8b03-d35ba6139b00)


![image](https://github.com/younlea/younlea.github.io/assets/1435846/57fc8908-83e3-465e-b6e0-59173fa0621f)

설치후 추가 모델들을 허깅페이스에 가서 받아서 아래 폴더에 카피 합니다.  
[https://huggingface.co/lllyasviel/ControlNet/tree/main/models](https://huggingface.co/lllyasviel/ControlNet/tree/main/models)
```
받을 파일들
control_sd15_canny.pth
control_sd15_depth.pth
control_sd15_openpose.pth
control_sd15_scribble.pth

카피해 넣어야 할 곳 : \stable-diffusion-webui\extensions\sd-webui-controlnet\models\
```

추가로 아래 패키지도 설치합니다.  
```
pip install opencv-python
```
[ref](https://cantips.com/3789)   
[ref](https://coconuts.tistory.com/1027)   

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ky2yHWWYfpg" frameborder="0" allowfullscreen></iframe>
