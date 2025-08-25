---
title: "로봇 강화 학습"
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

# robot reinforcement learning

## PPO
강화 학습이란 모든 트로젝토리에서 리워드의 합이 최대가 되게 하는 방식을 찾아 내는것    
### policy gradient 
<img width="886" height="494" alt="image" src="https://github.com/user-attachments/assets/df9b3e2a-bf90-4773-ae6e-0ee4ebc4423d" />    
샘플이 많아야 함.   
<img width="886" height="494" alt="image" src="https://github.com/user-attachments/assets/9cc1ccc9-d326-4614-a87a-c1af78faec67" />    
많은 데이터가 필요한 방식을 해결하기 위해 actor critic 방식으로 접근      
state, action, reward     
<img width="886" height="494" alt="image" src="https://github.com/user-attachments/assets/14f13398-5b4b-4b22-a2c9-b24ee1d1c58f" />    
### PPO  
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/ce71bf6c-0d46-44c5-ae48-3b5cb11e498e" />    


## SIM to REAL
강화 학습을 실제 로봇으로 하게 되면 비용, 위헙도 측면에서 적합하지 않다. 그래서 SIM을 사용하는게 더 유용합니다.    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/756a0388-b7ee-4b8f-805b-b6e4320fedca" />
다만 SIM과 실제 환경의 차이가 있어서 이를 위해 보상해줘야 하는 이슈가 있다.     

<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/d08f93bf-df0e-43a9-9a6e-42ac8b815b49" />     
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/b250c3ac-8a2e-4ea5-8664-e45e016b83df" />    



### robust rocomotion    
rl in the world :daydreamer    
https://www.youtube.com/watch?v=xAXvfVTgqr0    

a walk in the park learning to walk in 20 minutes with model-free reinforcement learning    
https://www.youtube.com/watch?v=YO1USfn6sHY    

<iframe width="560" height="315" src="https://www.youtube.com/embed/gM_EjJTq2F8?si=Gpntv4L61HtY5mAU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## 강화 학습을 이용한 로봇 보행 (PPO를 사용)
### 논문   엄청 많은 디바이스로 학습
https://www.youtube.com/watch?v=8sO7VS3q8d0

<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/65e42e6e-91fa-4346-9eb3-c9908f347c75" />    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/a7cb0b7e-78bf-4dbc-84c0-74de9a364dc9" />    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/586e4ed5-623d-4232-842f-f28afe316992" />    
reward    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/b49c80b8-703b-4f1f-aa96-ceb054bba4a3" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/786417ed-9b78-4599-b729-68816e94af07" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/e1f1b863-ab74-4590-8ed0-73a042beb2e9" />    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/347a86db-00da-426b-a0f1-f0e929707b9d" />

### 논문 (RMA) - 어려운 환경에서 실시간으로 동작    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/051d3b51-0808-44a0-a707-7944dc39e5b4" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/31923e5c-9c45-45c8-93de-415880cbe535" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/8ff4c013-98c9-4c64-a8ac-1f7841ef50be" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/94472a42-1990-426a-b0bd-f1a8a98e6663" />

### 논문 - robot parkour
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/5f12c344-3e09-4e6b-80e7-b9ee41ab74a5" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/20c9b2a1-77f0-4b92-a6ca-ca1a8badbb43" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/c552c6a2-4f6c-4eeb-869c-9592d2c62fe7" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/574b2176-91af-4fc5-ae94-f606300b24ae" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/3ef5a468-b8a9-4f11-8820-f44ac14c2c75" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/5226c659-51c5-40d8-9ca1-c4ed93668395" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/8fb9b309-d124-4bb2-83eb-f162e40b95bf" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/78787e8c-a989-4ab2-9c35-ce3751f125be" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/ff22e757-5074-46a2-ab94-3e7107ccb9ee" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/24f7f1a6-5258-41ff-8833-9298e0d19b74" />

### 논문 (휴머노이드) robot parkour 
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/2468fe75-8633-4256-b06c-c982f164e237" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/0feea12d-d079-4a6b-b5c7-5114476a71bf" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/f532177d-394c-4b1f-b2d7-e8aeed286ba0" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/7ff0d2a1-dc1f-4fe2-9728-1688ae32e0f0" />

### 논문 (transformer)   
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/16e3291c-1036-4d1a-a599-032e94799ac1" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/6f994f6a-2676-410d-bf01-6ff81da161dc" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/8f0daf1e-0b51-437e-812c-15cb47bc2a47" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/a3f7dfb3-7c8a-4910-a550-20892538ab69" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/bfe2262b-9583-4cf7-971d-d4becf48acd6" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/cf788a88-82b1-447c-abf8-78759c7de38c" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/b79313a9-eac2-4f47-ac8c-831348b00326" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/64a0e503-3779-45c0-8dc4-2844cd81055c" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/df43ffcd-9802-44be-ab24-8e606fad131b" />


## 로봇 파운데이션 모델
