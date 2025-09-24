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

## PPO(Proximal Policy Optimization)
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

<details>
<summary>1. PPO가 뭘까?</summary>

강화학습에서 **에이전트(agent)**는 환경에서 행동을 하면서 보상(reward)을 얻고,  
그 경험을 통해 **정책(policy, 행동을 선택하는 규칙)**을 점점 더 좋게 만들어 갑니다.  

문제는, 정책을 조금씩 업데이트해야 하는데, **너무 크게 바꾸면 성능이 떨어질 수 있다는 점**입니다.  
👉 예: 원래 잘하던 동작을 완전히 잊어버리거나, 랜덤하게 행동하게 됨.  

그래서 나온 방법이 **PPO(Proximal Policy Optimization, 근접 정책 최적화)**입니다.  
이름 그대로 “정책을 너무 멀리 바꾸지 말고, **가까운(proximal)** 범위 안에서만 최적화하자”라는 아이디어예요.  
</details>

---

<details>
<summary>2. 기존 문제점</summary>

- **Policy Gradient**: 정책을 gradient로 업데이트하는데, 한 번에 너무 크게 바뀔 수 있음 → 불안정.  
- **TRPO(Trust Region Policy Optimization)**: 정책 변화가 너무 크지 않도록 제약을 줌(Trust Region).  
  하지만 수학이 복잡하고 구현이 어렵고 계산량이 큼.  
</details>

---

<details>
<summary>3. PPO의 핵심 아이디어</summary>

PPO는 TRPO를 단순하게 만든 버전이에요.  

- 새로운 정책과 옛날 정책의 비율  

$$
r(\theta) = \frac{\pi_\theta(a|s)}{\pi_{\theta_{\text{old}}}(a|s)}
$$  

을 계산함.  

- 이 비율이 **1에 가까우면** (= 행동 확률이 많이 안 바뀌었으면) 괜찮음.  
- 그런데 너무 멀어지면(예: 2배 이상 바뀌면) 업데이트를 강제로 **클리핑(clipping)** 해서 제한을 둠.  

즉,  

$$
L^{CLIP}(\theta) = \min \Big( r(\theta) \hat{A}, \; \text{clip}(r(\theta), 1-\epsilon, 1+\epsilon) \hat{A} \Big)
$$  

여기서  
- $\hat{A}$ = advantage (이 행동이 평균보다 얼마나 좋은지)  
- $\epsilon$ = 허용 오차 (예: 0.1 ~ 0.2)  

👉 결국, 정책이 너무 급격히 바뀌지 않게 하면서도, 좋은 방향으로 조금씩 개선하도록 함.  
</details>

---

<details>
<summary>4. 비유로 이해하기</summary>

PPO를 **아이의 학습**에 비유해볼게요.  

- 아이가 자전거를 타는 걸 배우고 있음.  
- 갑자기 “오늘은 외발자전거 타!” 라고 하면 너무 어려워서 못 배움 (정책 급격 변화).  
- 대신 “자전거에서 손을 한쪽만 떼고 가보자”처럼 **조금만 변화**를 주면, 안정적으로 늘어남.  

👉 PPO는 이런 식으로 학습을 제한해서 안정성을 확보하는 방법이에요.  
</details>

---

<details>
<summary>5. PPO의 장점</summary>

- 구현이 비교적 간단하다 (TRPO보다 훨씬 쉬움).  
- 안정적이다 (정책이 폭주하지 않음).  
- 성능도 좋은 편이라, 현재 **강화학습에서 가장 널리 쓰이는 방법 중 하나**.  
</details>

---

<details>
<summary>6. PPO 코드 예시 (PyTorch 스타일)</summary>

```python
import torch
import torch.nn as nn
import torch.optim as optim

class PolicyNet(nn.Module):
    def __init__(self, state_dim, action_dim):
        super().__init__()
        self.fc = nn.Sequential(
            nn.Linear(state_dim, 64), nn.ReLU(),
            nn.Linear(64, 64), nn.ReLU(),
            nn.Linear(64, action_dim)
        )
    
    def forward(self, x):
        return torch.softmax(self.fc(x), dim=-1)

# 정책 네트워크와 옵티마이저
policy = PolicyNet(state_dim=4, action_dim=2)
optimizer = optim.Adam(policy.parameters(), lr=3e-4)

# PPO 업데이트 단계 (클리핑 포함)
def ppo_update(old_log_probs, states, actions, advantages, epsilon=0.2):
    new_probs = policy(states)
    new_log_probs = torch.log(new_probs.gather(1, actions))
    
    ratio = torch.exp(new_log_probs - old_log_probs)
    
    clipped_ratio = torch.clamp(ratio, 1 - epsilon, 1 + epsilon)
    loss = -torch.min(ratio * advantages, clipped_ratio * advantages).mean()
    
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
```
</details>

## SIM to REAL
강화 학습을 실제 로봇으로 하게 되면 비용, 위헙도 측면에서 적합하지 않다. 그래서 SIM을 사용하는게 더 유용합니다.    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/756a0388-b7ee-4b8f-805b-b6e4320fedca" />     

다만 SIM과 실제 환경의 차이가 있어서 이를 위해 보상해줘야 하는 이슈가 있다.     
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/d08f93bf-df0e-43a9-9a6e-42ac8b815b49" />   

### FurnitureBench    
[https://github.com/clvrai/furniture-bench?tab=readme-ov-file](https://github.com/clvrai/furniture-bench?tab=readme-ov-file)    
[https://clvrai.github.io/furniture-bench/](https://clvrai.github.io/furniture-bench/)     
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/b250c3ac-8a2e-4ea5-8664-e45e016b83df" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/qfJEivnHVuE?si=jLRwZgiy_7L_7khj&amp;start=33" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     





### robust rocomotion    
rl in the world :daydreamer    
<iframe width="560" height="315" src="https://www.youtube.com/embed/xAXvfVTgqr0" frameborder="0" allowfullscreen></iframe>     

a walk in the park learning to walk in 20 minutes with model-free reinforcement learning    
<iframe width="560" height="315" src="https://www.youtube.com/embed/YO1USfn6sHY" frameborder="0" allowfullscreen></iframe>.   

<iframe width="560" height="315" src="https://www.youtube.com/embed/gM_EjJTq2F8?si=Gpntv4L61HtY5mAU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>      

## 강화 학습을 이용한 로봇 보행 (PPO를 사용)
### 논문- (Learning to walk in minutes using Massively Parallel Deep RL) - 엄청 많은 디바이스로 학습
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/65e42e6e-91fa-4346-9eb3-c9908f347c75" />    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/a7cb0b7e-78bf-4dbc-84c0-74de9a364dc9" />    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/586e4ed5-623d-4232-842f-f28afe316992" />    
reward    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/b49c80b8-703b-4f1f-aa96-ceb054bba4a3" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/786417ed-9b78-4599-b729-68816e94af07" />
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/e1f1b863-ab74-4590-8ed0-73a042beb2e9" />    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/347a86db-00da-426b-a0f1-f0e929707b9d" />    

<iframe width="560" height="315" src="https://www.youtube.com/embed/8sO7VS3q8d0" frameborder="0" allowfullscreen></iframe>.   

### 논문 (RMA) - 어려운 환경에서 실시간으로 동작    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/051d3b51-0808-44a0-a707-7944dc39e5b4" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/31923e5c-9c45-45c8-93de-415880cbe535" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/8ff4c013-98c9-4c64-a8ac-1f7841ef50be" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/94472a42-1990-426a-b0bd-f1a8a98e6663" />   
<iframe width="560" height="315" src="https://www.youtube.com/embed/tpFQR_HUYss" frameborder="0" allowfullscreen></iframe>.   

### 논문 - robot parkour
[robot-parkour.github.io](robot-parkour.github.io).    
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/5f12c344-3e09-4e6b-80e7-b9ee41ab74a5" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/20c9b2a1-77f0-4b92-a6ca-ca1a8badbb43" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/c552c6a2-4f6c-4eeb-869c-9592d2c62fe7" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/574b2176-91af-4fc5-ae94-f606300b24ae" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/3ef5a468-b8a9-4f11-8820-f44ac14c2c75" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/5226c659-51c5-40d8-9ca1-c4ed93668395" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/8fb9b309-d124-4bb2-83eb-f162e40b95bf" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/78787e8c-a989-4ab2-9c35-ce3751f125be" />   
<img width="889" height="495" alt="image" src="https://github.com/user-attachments/assets/ff22e757-5074-46a2-ab94-3e7107ccb9ee" />   

<iframe width="560" height="315" src="https://www.youtube.com/embed/M7lDCSF0KP0" frameborder="0" allowfullscreen></iframe>.   

### 논문 (휴머노이드) robot parkour   
[https://humanoid4parkour.github.io/](https://humanoid4parkour.github.io/).   
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/2468fe75-8633-4256-b06c-c982f164e237" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/0feea12d-d079-4a6b-b5c7-5114476a71bf" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/f532177d-394c-4b1f-b2d7-e8aeed286ba0" />
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/7ff0d2a1-dc1f-4fe2-9728-1688ae32e0f0" />   
<iframe width="560" height="315" src="https://www.youtube.com/embed/9ilYqoQEQeg" frameborder="0" allowfullscreen></iframe>.   


### 논문 (transformer)  
<img width="889" height="503" alt="image" src="https://github.com/user-attachments/assets/16e3291c-1036-4d1a-a599-032e94799ac1" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/6f994f6a-2676-410d-bf01-6ff81da161dc" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/8f0daf1e-0b51-437e-812c-15cb47bc2a47" />
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/a3f7dfb3-7c8a-4910-a550-20892538ab69" />.    
<iframe width="560" height="315" src="https://www.youtube.com/embed/Y8URcPDbPhg" frameborder="0" allowfullscreen></iframe>.   

#### tesla optimus.   
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/bfe2262b-9583-4cf7-971d-d4becf48acd6" />   
[https://x.com/tesla_optimus/status/1922456791549427867](https://x.com/tesla_optimus/status/1922456791549427867).   


#### unitree.  
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/cf788a88-82b1-447c-abf8-78759c7de38c" />   
<iframe width="560" height="315" src="https://www.youtube.com/embed/Nkh6RUocD8c" frameborder="0" allowfullscreen></iframe>.   

<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/b79313a9-eac2-4f47-ac8c-831348b00326" />   
<iframe width="560" height="315" src="https://www.youtube.com/embed/p0xFou30hWI" frameborder="0" allowfullscreen></iframe>.   


#### boston Dynamics
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/64a0e503-3779-45c0-8dc4-2844cd81055c" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/I44_zbEwz_w" frameborder="0" allowfullscreen></iframe>.   

<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/df43ffcd-9802-44be-ab24-8e606fad131b" />   
<iframe width="560" height="315" src="https://www.youtube.com/embed/dFObux6mfTc" frameborder="0" allowfullscreen></iframe>.   



## 로봇 파운데이션 모델
RFM (robot foundation model)    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/6d9b3106-dbcb-474d-946a-8d159be14dbe" />   
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/90c3e129-ae6b-4609-9d29-4338383aa2d9" />   

### 2016 google foundation model
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/fa8cbbcd-b80a-4970-b14a-ef9845065dad" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/V05SuCSRAtg?si=dTTICLA8mFnZtpG7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     

### QT-Opt
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/e3f90f53-1296-4e37-9b8b-420604d54507" />   
<iframe width="560" height="315" src="https://www.youtube.com/embed/W4joe3zzglU?si=9GiJfqOi-gRk0g1a" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     
[논문](https://www.semanticscholar.org/paper/QT-Opt%3A-Scalable-Deep-Reinforcement-Learning-for-Kalashnikov-Irpan/eb37e7b76d26b75463df22b2a3aa32b6a765c672)    

### MT-Opt
task 나눠서, 잡을 물체를 정해서 잡는 훈련        
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/6db71e79-b82a-42c5-adc7-b33c58497d78" />     
<iframe width="560" height="315" src="https://www.youtube.com/embed/i3uUGSko2zY?si=gWvedTTP7qHISfv6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     
[논문](https://www.semanticscholar.org/paper/MT-Opt%3A-Continuous-Multi-Task-Robotic-Reinforcement-Kalashnikov-Varley/3e85d208b1b927fdb69ecf8336c70995818aaebd)     

### BC-Z
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/3b584699-4b75-4a49-a750-fa8350595317" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/f-9Jw3KvPJo?si=wSj8P7w67Qu-W3UI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>    
[논문](https://arxiv.org/abs/2202.02005)    

### RT-1 (robotics transformer)
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/11ac737a-7e4c-48c7-9ecc-3684f0f32f4a" />    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/8f6ea23c-74e9-4c85-b5a8-f590d01cb1e3" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/UuKAp9a6wMs?si=DaoM7FMJTygO6ZD9" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     
[https://robotics-transformer1.github.io/](https://robotics-transformer1.github.io/)     
[논문](https://arxiv.org/abs/2212.06817)     

### RT-2
pretrain-vlm 을 가져다가 학습 (vision language action)    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/0bb04e4d-9816-4147-acb8-b327ec436870" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/gXgmqjthrPw?si=i8xhxXUdyBZFs9ne" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     
[https://robotics-transformer2.github.io/](https://robotics-transformer2.github.io/)    
[논문](https://arxiv.org/abs/2307.15818)    

### ALOHA and ACT   
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/c54382fe-d5a0-43ea-be0e-e7dbc8367dc0" />     
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/911966f0-a7d7-4d8d-b551-b1fe9990f404" />    
<img width="882" height="500" alt="image" src="https://github.com/user-attachments/assets/72b967d4-dd29-4807-9431-d43022c7a3d2" />    
ACT : imitation learning algorithm   
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/e8244993-e323-4653-b633-154d39fee1b5" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/277f808e-f6c3-4889-af27-54cc005f4e22" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/fee8f51c-3618-43c2-84ed-1ca3bffb1756" />
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/c7bb3090-a09c-43a5-90e6-7dd7d97d2387" />
<iframe width="560" height="315" src="https://www.youtube.com/embed/pN4Ig_aTSUo?si=6yrb45hw3vm6mDnl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     
[https://tonyzhaozh.github.io/aloha/](https://tonyzhaozh.github.io/aloha/)     

### mobile aloha
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/2277028f-e613-49f2-87c7-1a68ddd0bea8" />    
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/f274043b-4e52-4951-a16d-178a648d9438" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/X_Ch-4WmiUg?si=CFfMfK703onsoZ4O" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>     
[https://mobile-aloha.github.io/](https://mobile-aloha.github.io/)     

### diffusion policy (2023)    
<img width="888" height="500" alt="image" src="https://github.com/user-attachments/assets/74ab48a2-d43e-4019-baee-c30a98fe3f99" />    
<img width="885" height="218" alt="image" src="https://github.com/user-attachments/assets/e3f6df52-7c46-47f7-be0b-0c259550a64f" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/8dceff0d-4378-407a-acac-8d578034a818" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/59cc82ea-1b26-4414-bc6a-4f308a573d25" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/CtJDROYBmSs?si=X3yhUhhG9VFkt6k8&amp;start=1427" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>    
[https://diffusion-policy.cs.columbia.edu/](https://diffusion-policy.cs.columbia.edu/)     
[https://github.com/real-stanford/diffusion_policy](https://github.com/real-stanford/diffusion_policy)     

### scaling robotic datasets       
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/1c4798af-b1fa-4779-b53f-94c4c2ed3f77" />    
[논문](chrome-extension://oemmndcbldboiebfnladdacbdfmadadm/https://www.roboticsproceedings.org/rss20/p120.pdf)    
[https://droid-dataset.github.io/](https://droid-dataset.github.io/)      

<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/56ced518-e421-48cc-8e4f-87bb7f029292" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/91c23287-262c-4afe-bed3-fe8ebe0b4544" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/21d74dd8-db1a-4d8c-ba8f-4e7ff6bf3365" />    
[https://github.com/google-deepmind/open_x_embodiment](https://github.com/google-deepmind/open_x_embodiment)   
[https://robotics-transformer-x.github.io/](https://robotics-transformer-x.github.io/)     

### RT-X model
RT-1, RT-2, OpenX 
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/7179eb02-f702-4760-9339-93acc2e0efae" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/712de049-6bbb-4ccc-8b8c-8a6a87fb0046" />    

### Octo
transformer + diffusion    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/46de2f34-881b-46e8-b853-df4681db0633" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/b4971be3-03f7-44c6-a6ed-ab6e6786dcf1" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/99667VDGWMg?si=-imQp_mfZx9xQVLg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>    
[https://octo-models.github.io/](https://octo-models.github.io/)     

### OpenVLA [An Open-Source Vision-Language-Action Model]  (open source, llama 사용)    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/670d53ab-d876-4163-b3aa-222a1753e12f" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/0c41ac26-0f21-47de-8cac-ea2fc707ae17" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/fa8fa14c-f9a0-4222-8899-e0bbe6c6270c" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/20834071-3606-4a4c-871b-f270ef362d7e" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/ef0b617f-2ee0-465f-8b11-5a5bbcfcb40f" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/ozhQLNprW2A?si=Ulc8HLYNGQP7_wi6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
[https://openvla.github.io/](https://openvla.github.io/)    
[https://github.com/openvla/openvla](https://github.com/openvla/openvla)    

### OpenVLA-OFT
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/307b88db-0f00-4371-8c51-b75c32340e13" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/2a14d929-f705-4f79-ae1a-bb29a391abc0" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/T3Zkkr_NTSA?si=jENhhtEcqoEURb_e" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
[https://github.com/moojink/openvla-oft](https://github.com/moojink/openvla-oft)    
[https://openvla-oft.github.io/](https://openvla-oft.github.io/)      

### SOTA VLA - open model
[https://www.physicalintelligence.company/](https://www.physicalintelligence.company/)     
10000 시간의 데이터  
#### pyzero
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/712ddbcb-40d7-46aa-a830-d62b2ac145ef" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/a2bb5952-80ff-4e6d-ba96-f5d49f9f83be" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/24213960-c28d-4428-a8e3-0df9c5cc83ba" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/f1e56676-4f7a-4119-b71c-723b5319351a" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/eabcced4-044e-4901-9496-68efb8a96d3c" />    
<iframe width="560" height="315" src="https://www.youtube.com/embed/J-UTyb7lOEw?si=MdJ5K0HlVIrPlV3L" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>    
[https://www.physicalintelligence.company/blog/pi0](https://www.physicalintelligence.company/blog/pi0)    

#### py fast     
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/386a4df5-be6f-4053-bb16-a153b3bd88bc" />   
[https://www.physicalintelligence.company/research/fast](https://www.physicalintelligence.company/research/fast)    

#### py zero    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/8efcaa48-5dd1-422e-83b7-a3b1db8d69bb" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/c6b29159-c3ba-4815-b33e-42172c1e7825" />    
<img width="885" height="499" alt="image" src="https://github.com/user-attachments/assets/f1c7f154-00ed-4536-b649-acc29eed7283" />    
[https://www.physicalintelligence.company/blog/pi05](https://www.physicalintelligence.company/blog/pi05)     

#### Real-Time Action Chunking with Large Models    
[https://www.physicalintelligence.company/research/real_time_chunking](https://www.physicalintelligence.company/research/real_time_chunking)    

<img width="885" height="270" alt="image" src="https://github.com/user-attachments/assets/a412e792-2568-47e5-b74c-3edbc1ea571e" />    


