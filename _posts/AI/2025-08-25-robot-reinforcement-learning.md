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
<iframe width="560" height="315" src="https://www.youtube.com/embed/xAXvfVTgqr0" frameborder="0" allowfullscreen></iframe>

a walk in the park learning to walk in 20 minutes with model-free reinforcement learning    
<iframe width="560" height="315" src="https://www.youtube.com/embed/YO1USfn6sHY" frameborder="0" allowfullscreen></iframe>

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
RFM (robot foundation model)    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/6d9b3106-dbcb-474d-946a-8d159be14dbe" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/90c3e129-ae6b-4609-9d29-4338383aa2d9" />
### 2016 google foundation model
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/fa8cbbcd-b80a-4970-b14a-ef9845065dad" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/bf387b08-688f-4a1c-83f7-58ac27aead8c" />
### QT-Opt
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/e3f90f53-1296-4e37-9b8b-420604d54507" />
### MT-Opt
task 나눠서, 잡을 물체를 정해서 잡는 훈련        
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/6db71e79-b82a-42c5-adc7-b33c58497d78" />
### BC-Z
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/3b584699-4b75-4a49-a750-fa8350595317" />
### RT-1 (robotics transformer)
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/11ac737a-7e4c-48c7-9ecc-3684f0f32f4a" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/8f6ea23c-74e9-4c85-b5a8-f590d01cb1e3" />

### RT-2
pretrain-vlm 을 가져다가 학습 (vision language action)    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/0bb04e4d-9816-4147-acb8-b327ec436870" />

### ALOHA and ACT   
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/c54382fe-d5a0-43ea-be0e-e7dbc8367dc0" />     
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/911966f0-a7d7-4d8d-b551-b1fe9990f404" />    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/72b967d4-dd29-4807-9431-d43022c7a3d2" />    
ACT : imitation learning algorithm   
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/e8244993-e323-4653-b633-154d39fee1b5" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/277f808e-f6c3-4889-af27-54cc005f4e22" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/fee8f51c-3618-43c2-84ed-1ca3bffb1756" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/c7bb3090-a09c-43a5-90e6-7dd7d97d2387" />

### mobile aloha
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/2277028f-e613-49f2-87c7-1a68ddd0bea8" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/f274043b-4e52-4951-a16d-178a648d9438" />

### diffusion policy (2023)    
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/74ab48a2-d43e-4019-baee-c30a98fe3f99" />
<img width="885" height="218" alt="image" src="https://github.com/user-attachments/assets/e3f6df52-7c46-47f7-be0b-0c259550a64f" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/8dceff0d-4378-407a-acac-8d578034a818" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/59cc82ea-1b26-4414-bc6a-4f308a573d25" />

### scaling robotic datasets       
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/1c4798af-b1fa-4779-b53f-94c4c2ed3f77" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/56ced518-e421-48cc-8e4f-87bb7f029292" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/91c23287-262c-4afe-bed3-fe8ebe0b4544" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/21d74dd8-db1a-4d8c-ba8f-4e7ff6bf3365" />

### RT-X model
RT-1, RT-2, OpenX 
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/7179eb02-f702-4760-9339-93acc2e0efae" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/712de049-6bbb-4ccc-8b8c-8a6a87fb0046" />

### Octo
transformer + diffusion    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/46de2f34-881b-46e8-b853-df4681db0633" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/b4971be3-03f7-44c6-a6ed-ab6e6786dcf1" />

### OpenVLA (open source, llama 사용)    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/670d53ab-d876-4163-b3aa-222a1753e12f" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/0c41ac26-0f21-47de-8cac-ea2fc707ae17" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/fa8fa14c-f9a0-4222-8899-e0bbe6c6270c" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/20834071-3606-4a4c-871b-f270ef362d7e" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/ef0b617f-2ee0-465f-8b11-5a5bbcfcb40f" />

### OpenVLA-OFT
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/307b88db-0f00-4371-8c51-b75c32340e13" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/2a14d929-f705-4f79-ae1a-bb29a391abc0" />

### SOTA VLA - open model
10000 시간의 데이터     
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/712ddbcb-40d7-46aa-a830-d62b2ac145ef" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/a2bb5952-80ff-4e6d-ba96-f5d49f9f83be" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/24213960-c28d-4428-a8e3-0df9c5cc83ba" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/f1e56676-4f7a-4119-b71c-723b5319351a" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/eabcced4-044e-4901-9496-68efb8a96d3c" />
py fast     
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/386a4df5-be6f-4053-bb16-a153b3bd88bc" />
py zero    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/8efcaa48-5dd1-422e-83b7-a3b1db8d69bb" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/c6b29159-c3ba-4815-b33e-42172c1e7825" />
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/f1c7f154-00ed-4536-b649-acc29eed7283" />

<img width="885" height="270" alt="image" src="https://github.com/user-attachments/assets/a412e792-2568-47e5-b74c-3edbc1ea571e" />


