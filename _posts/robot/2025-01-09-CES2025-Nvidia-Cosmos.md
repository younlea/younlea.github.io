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

엔비디아가 CES 2025에서 발표한 **NVIDIA Cosmos™**는 물리적 AI 시스템(로봇 및 자율주행 차량 등)의 개발을 가속화하기 위해 설계된 혁신적인 플랫폼입니다. 이 플랫폼은 **World Foundation Models(WFMs)**, 고급 토크나이저, 신뢰성 보장 가드레일, 그리고 AI 가속 데이터 처리 파이프라인으로 구성되어 있습니다. 아래에서 주요 특징과 활용 방안을 요약합니다.

## 주요 특징

1. **World Foundation Models (WFMs)**:
   - WFMs는 물리 기반 시뮬레이션과 합성 데이터 생성을 통해 현실 세계를 정밀하게 모델링합니다.
   - 텍스트, 이미지, 비디오, 센서 데이터 등을 입력으로 받아 물리적 상호작용과 공간 관계를 반영한 가상 환경을 생성합니다.
   - 로봇 및 자율주행 차량 개발에 최적화된 사전 학습된 모델을 포함하며, 맞춤형 모델 구축 및 미세 조정도 가능합니다[1][2][3].

2. **합성 데이터 생성**:
   - NVIDIA Omniverse와 연계하여 3D 시나리오를 기반으로 제어 가능한 포토리얼리스틱 비디오를 생성할 수 있습니다.
   - 다양한 도로 조건(예: 눈길)이나 산업 환경(예: 창고)을 시뮬레이션하여 대규모 훈련 데이터를 제공합니다[2][3].

3. **고급 토크나이저**:
   - Cosmos Tokenizer는 이미지와 비디오 데이터를 고효율로 압축 및 처리하며, 기존 기술 대비 8배 높은 압축률과 12배 빠른 처리 속도를 제공합니다.
   - 이를 통해 트랜스포머 모델의 훈련 및 추론 비용을 크게 절감할 수 있습니다[3][4].

4. **데이터 처리 파이프라인**:
   - NVIDIA Blackwell GPU를 활용하여 20만 시간 분량의 비디오 데이터를 단 14일 만에 처리할 수 있습니다(전통적인 CPU 기반 파이프라인은 3년 이상 소요).
   - NeMo Curator를 통해 효율적인 데이터 큐레이션 및 라벨링이 가능하며, 모델 훈련과 최적화를 지원합니다[4][5].

5. **멀티버스 시뮬레이션**:
   - Cosmos와 Omniverse를 사용해 다양한 미래 결과를 시뮬레이션하여 AI 모델이 최적의 경로를 선택하도록 지원합니다.
   - 이는 예측 유지보수, 장애물 회피 등 자율적 의사결정에 유용합니다[2][10].

6. **오픈 모델 라이선스**:
   - Cosmos WFMs는 오픈 라이선스로 제공되어 개발자들이 자유롭게 다운로드하고 활용할 수 있습니다(Hugging Face 및 NVIDIA NGC 카탈로그에서 제공)[1][4][10].

## 활용 사례

1. **자율주행 차량**:
   - 실제 도로 주행 데이터를 기반으로 가상 주행 시나리오를 생성해 훈련 데이터 품질을 향상시킵니다.
   - 위험한 상황을 안전하게 테스트할 수 있는 시뮬레이션 환경을 제공합니다[7][13].

2. **로봇 공학**:
   - 창고 또는 제조 환경에서 로봇의 동작을 테스트하고 최적화하는 데 사용됩니다.
   - 물리적 상호작용과 객체 영속성을 고려한 정책 모델 평가가 가능합니다[3][9].

3. **비디오 검색 및 이해**:
   - Cosmos는 시공간 패턴을 이해하여 특정 훈련 시나리오(예: 혼잡한 창고 상황)를 쉽게 검색할 수 있도록 지원합니다[2][3].

## 시장 영향
Cosmos는 Uber, XPENG 등 주요 기업들이 초기 채택자로 참여하면서 물리 AI 개발의 새로운 표준으로 자리 잡고 있습니다. 이 플랫폼은 대규모 데이터 처리와 모델 훈련 비용을 획기적으로 절감하며, 개발자들에게 강력한 도구를 제공함으로써 로봇 및 자율주행 차량 분야의 혁신을 가속화할 것으로 기대됩니다[4][12].   

NVIDIA Cosmos는 기존의 로봇 시뮬레이션 솔루션들, 특히 Gazebo와 같은 시스템과 비교했을 때 여러 면에서 뛰어난 성능과 효율성을 제공합니다. 이로 인해 개발 일정이 크게 단축될 수 있습니다. 주요 차별점은 다음과 같습니다:   

## **1. 데이터 처리 및 생성 속도의 혁신**
- **가속화된 데이터 처리 파이프라인**: Cosmos는 NVIDIA Blackwell GPU를 활용하여 20만 시간의 비디오 데이터를 14일 만에 처리할 수 있습니다. 이는 CPU 기반 파이프라인에서 3년 이상 걸리는 작업을 대폭 단축한 것입니다[2][4][7].
- **합성 데이터 생성**: Cosmos는 NVIDIA Omniverse와 통합되어 포토리얼리스틱하고 물리적으로 정확한 합성 데이터를 생성합니다. 이를 통해 다양한 환경 조건에서 로봇을 훈련할 수 있으며, 실제 데이터를 수집하는 데 드는 시간과 비용을 절감합니다[2][18].

## **2. 고급 물리 및 그래픽 시뮬레이션**
- **물리적 정확성**: Cosmos는 NVIDIA PhysX 엔진을 사용하여 현실적인 물리 상호작용(중력, 충돌, 관성)을 시뮬레이션합니다. 이는 Gazebo와 같은 기존 솔루션의 물리 엔진(ODE, Bullet 등)보다 더 정교하고 효율적입니다[1][14].
- **고품질 그래픽**: Cosmos는 고해상도 레이 트레이싱을 지원하는 NVIDIA RTX 기반 렌더링 기술을 사용하여 Gazebo보다 훨씬 더 사실적인 그래픽 환경을 제공합니다[1][13].

## **3. 데이터 토크나이징 및 모델 최적화**
- **고급 토크나이저**: Cosmos의 토크나이저는 기존 기술보다 8배 높은 압축률과 12배 빠른 처리 속도를 제공하며, 이를 통해 데이터 준비 및 모델 훈련 비용을 크게 절감합니다[2][4].
- **모델 커스터마이징**: Cosmos는 사전 학습된 World Foundation Models(WFMs)를 제공하며, 이를 특정 애플리케이션에 맞게 미세 조정할 수 있어 새로운 모델을 처음부터 훈련하는 것보다 훨씬 빠르고 효율적입니다[19].

## **4. 시뮬레이션 속도와 확장성**
- **멀티버스 시뮬레이션**: Cosmos는 다양한 결과를 예측하고 최적의 경로를 선택할 수 있도록 돕는 멀티버스 시뮬레이션 기능을 제공합니다. 이는 로봇의 의사결정 및 정책 평가를 가속화합니다[2][14].
- **확장성**: Cosmos는 대규모 데이터 세트를 처리하고 다양한 환경에서 로봇 행동을 테스트할 수 있는 유연성을 제공합니다. 이는 Gazebo가 주로 제한된 환경에서 작동하는 것과 대조적입니다[3][18].

## **5. Gazebo와의 비교**
| 특징                     | Gazebo                          | NVIDIA Cosmos                  |
|--------------------------|---------------------------------|--------------------------------|
| 물리 엔진                | ODE, Bullet 등                 | NVIDIA PhysX                  |
| 그래픽 품질              | 제한적 (OGRE 기반)              | 고해상도 RTX 기반 렌더링       |
| 데이터 처리 속도          | 느림 (CPU 중심)                | 매우 빠름 (GPU 가속)           |
| 합성 데이터 생성          | 제한적                          | 포토리얼리스틱 합성 가능        |
| 멀티버스 시뮬레이션 지원   | 없음                           | 지원                           |

결론적으로, NVIDIA Cosmos는 데이터 처리 속도, 합성 데이터 생성 능력, 그래픽 품질, 그리고 물리적 정밀도 면에서 기존 솔루션들을 능가하며, 이러한 혁신적인 기능들이 개발 일정을 대폭 단축시키는 데 기여합니다.

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
