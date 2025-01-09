---
title: "Nvidia Cosmos 발표"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

1. World Foundation Models (WFMs)
   - WFMs는 물리 기반 시뮬레이션과 합성 데이터 생성을 통해 현실 세계를 정밀하게 모델링합니다.
   - 텍스트, 이미지, 비디오, 센서 데이터 등을 입력으로 받아 물리적 상호작용과 공간 관계를 반영한 가상 환경을 생성합니다.
   - 로봇 및 자율주행 차량 개발에 최적화된 사전 학습된 모델을 포함하며, 맞춤형 모델 구축 및 미세 조정도 가능합니다

2.합성 데이터 생성
   - NVIDIA Omniverse와 연계하여 3D 시나리오를 기반으로 제어 가능한 포토리얼리스틱 비디오를 생성할 수 있습니다.
   - 다양한 도로 조건(예: 눈길)이나 산업 환경(예: 창고)을 시뮬레이션하여 대규모 훈련 데이터를 제공합니다

3. 고급 토크나이저
   - Cosmos Tokenizer는 이미지와 비디오 데이터를 고효율로 압축 및 처리하며, 기존 기술 대비 8배 높은 압축률과 12배 빠른 처리 속도를 제공합니다.
   - 이를 통해 트랜스포머 모델의 훈련 및 추론 비용을 크게 절감할 수 있습니다[3][4].

4. 데이터 처리 파이프라인
   - NVIDIA Blackwell GPU를 활용하여 20만 시간 분량의 비디오 데이터를 단 14일 만에 처리할 수 있습니다(전통적인 CPU 기반 파이프라인은 3년 이상 소요).
   - NeMo Curator를 통해 효율적인 데이터 큐레이션 및 라벨링이 가능하며, 모델 훈련과 최적화를 지원합니다

5. 멀티버스 시뮬레이션
   - Cosmos와 Omniverse를 사용해 다양한 미래 결과를 시뮬레이션하여 AI 모델이 최적의 경로를 선택하도록 지원합니다.
   - 이는 예측 유지보수, 장애물 회피 등 자율적 의사결정에 유용합니다

6. 오픈 모델 라이선스
   - Cosmos WFMs는 오픈 라이선스로 제공되어 개발자들이 자유롭게 다운로드하고 활용할 수 있습니다(Hugging Face 및 NVIDIA NGC 카탈로그에서 제공)

ㅁ 기대 효과
Cosmos는 Uber, XPENG 등 주요 기업들이 초기 채택자로 참여하면서 물리 AI 개발의 새로운 표준으로 자리 잡고 있습니다. 이 플랫폼은 대규모 데이터 처리와 모델 훈련 비용을 획기적으로 절감하며, 개발자들에게 강력한 도구를 제공함으로써 로봇 및 자율주행 차량 분야의 혁신을 가속화할 것으로 기대됩니다.

ㅁ 기존 Nvidia에는 isaac sim, open source에는 Gazebo와 같은 시뮬레이터는 있었는데 속도 및 환경 범위 자체가 혁신적으로 차이가 나는것으로 보여서 추후 제품을 만들기 전에 시뮬레이션으로 사전 개발을 할수 있어서 개발 시간 단축에 많은 기여를 할 것으로 사료 됨. 

ㅁ 원문 소스

[1] NVIDIA Launches Cosmos to advance the development of Robots 
https://www.youtube.com/watch?v=eNT7YNmHxF8
[2] Advancing Physical AI with NVIDIA Cosmos World Foundation
  https://developer.nvidia.com/blog/advancing-physical-ai-with-nvidia-cosmos-world-foundation-model-platform/
[3] NVIDIA Launches Cosmos World Foundation Model Platform to ... 
https://www.edge-ai-vision.com/2025/01/nvidia-launches-cosmos-world-foundation-model-platform-to-accelerate-physical-ai-development/
[4] NVIDIA Unleashes Cosmos: Revolutionary AI Platform for ... 
https://www.stocktitan.net/news/NVDA/nvidia-launches-cosmos-world-foundation-model-platform-to-accelerate-74st2annquyd.html
[5] NVIDIA Cosmos for Developers 
https://developer.nvidia.com/cosmos
[6] NVIDIA launches Cosmos platform for AI & robotics - IT Brief Asia 
https://itbrief.asia/story/nvidia-launches-cosmos-platform-for-ai-robotics
[7] NVIDIA Announces Foundational World Model, Cosmos 
https://radiancefields.com/nvidia-announces-foundational-world-model-cosmos
[8] NVIDIA Launches Cosmos World Foundation Model Platform to ... 
https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-world-foundation-model-platform-to-accelerate-physical-ai-development
[9] NVIDIA unveils Omniverse upgrades, Cosmos foundation model ... 
https://www.therobotreport.com/nvidia-unveils-omniverse-upgrades-launches-cosmos-foundation-model-ces-2025/
