---
title: "로봇비지도 이상감지"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---


최근 로봇 동작 이상을 영상으로 감지하는 솔루션 중에서 지도 학습이 아닌 **비지도 학습(unsupervised learning)** 또는 **자기 지도 학습(self-supervised learning)** 방식을 활용한 기술들이 주목받고 있습니다. 아래는 관련된 최신 연구와 기술적 접근법을 정리한 내용입니다.

---

## **1. 최신 솔루션 및 연구**

### **1-1. Single Frame Supervised Video Anomaly Detection (SF-VAD)**
- **핵심 아이디어**: 단일 프레임의 이상 레이블만 사용해 비디오 이상 감지를 수행하는 새로운 접근법입니다. 이는 기존 완전 지도 학습 방식보다 적은 데이터로도 높은 성능을 제공합니다.
- **특징**:
  - 비정상 프레임과 정상 프레임 간의 유사성을 기반으로 이상 패턴을 모델링.
  - Gaussian-prior를 활용해 정상 패턴을 추출.
  - 시간적 특징을 강화하기 위한 경계 정제 모듈 도입.
- **성과**: 기존 비디오 이상 감지 방법 대비 높은 성능과 비용 효율성을 달성.
- [논문 링크](https://openreview.net/forum?id=A18zU6cgQ0)  

---

### **1-2. Generative Cooperative Learning (GCL)**
- **핵심 아이디어**: 생성기와 판별기를 협력적으로 학습시켜 비지도 비디오 이상 감지를 수행합니다.
- **특징**:
  - 생성기는 정상 데이터를 복원하고, 판별기는 이상 데이터를 식별.
  - 두 네트워크가 상호 보완적으로 학습하며 성능을 개선.
  - 완전 비지도 방식으로 복잡한 환경에서도 이상 이벤트를 효과적으로 탐지.
- [논문 링크](https://openaccess.thecvf.com/content/CVPR2022/papers/Zaheer_Generative_Cooperative_Learning_for_Unsupervised_Video_Anomaly_Detection_CVPR_2022_paper.pdf)

---

### **1-3. VLAVAD (Vision-Language Models Assisted Video Anomaly Detection)**
- **핵심 아이디어**: 대규모 사전학습된 비전-언어 모델을 활용하여 비지도 비디오 이상 감지를 수행합니다.
- **특징**:
  - 시퀀스 상태 공간 모듈(S3M)을 통해 시간적 불일치를 탐지.
  - 고차원 시각적 특징을 저차원 의미론적 특징으로 매핑하여 해석 가능성 향상.
  - 대규모 데이터셋(UCF-Crime, ShanghaiTech 등)에서 우수한 성능 입증.
- [논문 링크](https://arxiv.org/abs/2409.14109)

---

### **1-4. Abyss Solutions의 비지도 학습 기반 이상 감지**
- **핵심 아이디어**: 정상 데이터만으로 학습해 환경 변화에 강인한 이상 감지 시스템 개발.
- **특징**:
  - 사전 정의된 이상 사례 없이 정상 상태를 학습하여 새로운 이상 상황 탐지 가능.
  - CCTV, IoT 디바이스, 로봇(예: Boston Dynamics의 Spot)에 적용 가능.
- [자세히 보기](https://abysssolutions.co/case-studies/detecting-anomalies-using-robots-with-a-new-method-of-unsupervised-learning-ai/)

---

## **2. 트렌드 및 기술적 특징**

### **2-1. 비지도 학습(Unsupervised Learning)**
- 지도 학습과 달리 레이블이 없는 데이터로 정상 패턴을 학습하고, 이를 기반으로 이상 상황을 탐지합니다.
- 주요 알고리즘:
  - 오토인코더(Autoencoder): 정상 데이터를 재구성하고 재구성 오류를 기반으로 이상 탐지.
  - Normalizing Flow: 데이터 분포를 모델링하여 이상치를 식별.

### **2-2. 자기 지도 학습(Self-Supervised Learning)**
- 데이터 자체에서 감독 신호를 생성하여 학습하는 방식으로, 레이블 의존성을 줄입니다.
- 예: 로봇이 환경과 상호작용하며 데이터를 수집하고 이를 통해 목표 지향 행동을 학습하는 방법.

### **2-3. 다중 모달 데이터 융합**
- 영상, 센서 데이터, 음향 등 다양한 데이터를 결합하여 더 정밀한 이상 감지를 구현.

---

## **3. 적용 사례**

### **3-1. 제조업**
로봇 팔의 동작에서 발생하는 미세한 이상(예: 진동, 위치 오류)을 실시간으로 감지하여 생산 라인의 안정성을 높임.

### **3-2. 보안 및 공공 안전**
CCTV 영상에서 폭력, 낙상 등의 행동을 자동으로 탐지하여 사고를 예방.

### **3-3. 의료 분야**
수술 로봇의 동작에서 발생하는 이상 이벤트를 실시간으로 감지하여 환자의 안전성을 보장.

---

## **결론**

최근 로봇 동작 이상 감지는 지도 학습 의존도를 줄이고, 비지도 및 자기 지도 학습 방식을 통해 더 효율적이고 일반화된 솔루션으로 발전하고 있습니다. 이러한 기술은 제조업, 보안, 의료 등 다양한 분야에 적용 가능하며, 앞으로 더욱 정교한 시스템이 등장할 것으로 기대됩니다.


아래는 최신 로봇 동작 이상 감지 솔루션과 그들의 제약사항을 시기와 함께 정리한 내용입니다.

---

## **최신 로봇 동작 이상 감지 솔루션**

| **솔루션** | **출시 시기** | **주요 특징** | **제약사항** |
|------------|---------------|----------------|---------------|
| **SF-VAD (Single Frame Supervised Video Anomaly Detection)** | 2025년 1월 | - 단일 프레임 레이블만으로 이상 감지<br>- Gaussian-prior를 활용한 정상 패턴 모델링<br>- 시간적 특징 강화를 위한 경계 정제 모듈 도입[1] | - 단일 프레임 레이블에 의존하므로 복잡한 시간적 이상 탐지에는 한계<br>- 데이터셋의 다양성 부족 시 성능 저하 가능 |
| **GCL (Generative Cooperative Learning)** | 2022년 6월 | - 생성기와 판별기를 협력적으로 학습<br>- 비지도 학습 방식으로 레이블 필요 없음<br>- UCF-Crime, ShanghaiTech 데이터셋에서 높은 성능 입증[2][13] | - 낮은 대비(contrast)의 이상 탐지에 취약<br>- 복잡한 환경에서는 과잉 탐지가 발생할 가능성 |
| **VLAVAD (Vision-Language Models Assisted Video Anomaly Detection)** | 2024년 9월 | - 비전-언어 모델 활용<br>- Sequence State Space Module(S3M)을 통해 시간적 불일치 탐지<br>- 고차원 시각적 특징을 저차원 의미론적 특징으로 매핑[3] | - 대규모 사전학습 모델에 의존하여 높은 계산 비용 발생<br>- 새로운 환경에 대한 적응성 부족 |
| **Abyss Solutions의 비지도 학습 기반 이상 감지** | 2024년 이전 도입 | - 정상 데이터만으로 학습하여 새로운 이상 상황 탐지 가능<br>- 환경 변화에 강인한 시스템 설계<br>- CCTV, IoT, 로봇(Spot) 적용 가능[4] | - 정상 상태 정의가 부정확하면 오탐률 증가<br>- 특정 도메인에 최적화된 경우 일반화 성능 저하 |

---

## **제약사항 분석**

### **1. 데이터 의존성**
- 대부분의 솔루션은 정상 데이터를 기반으로 학습하기 때문에, 정상 상태를 정확히 정의하지 못하면 오탐(False Positive)이 증가할 수 있습니다.
- 예를 들어, GCL은 낮은 대비의 이상(less salient anomalies)을 놓칠 가능성이 있습니다[2].

### **2. 계산 비용**
- VLAVAD와 같은 대규모 비전-언어 모델 기반 솔루션은 높은 계산 비용과 메모리 사용량이 요구됩니다. 이는 실시간 애플리케이션에서 제약이 될 수 있습니다[3].

### **3. 일반화 성능**
- 특정 환경이나 데이터셋(UCF-Crime, ShanghaiTech 등)에 최적화된 경우, 새로운 도메인이나 환경에서 성능이 저하될 수 있습니다. Abyss Solutions의 기술도 특정 도메인에 맞춰진 경우 일반화가 어려울 수 있습니다[4].

### **4. 시간적 복잡성**
- SF-VAD는 단일 프레임 레이블을 활용하기 때문에, 긴 시간적 연속성을 필요로 하는 복잡한 이상 이벤트를 탐지하는 데 한계가 있습니다[1].

---

## **결론**

위 솔루션들은 지도 학습 의존도를 줄이고 비지도 및 자기 지도 학습 방식을 활용하여 효율성과 확장성을 높이고자 합니다. 그러나 데이터 의존성, 계산 비용, 일반화 문제 등은 여전히 해결해야 할 과제로 남아 있습니다. 각 솔루션의 강점과 제약을 고려하여 적합한 기술을 선택하는 것이 중요합니다.

출처
[1] Video Anomaly Detection via Single Frame Supervision | OpenReview https://openreview.net/forum?id=A18zU6cgQ0
[2] [PDF] Generative Cooperative Learning for Unsupervised Video Anomaly ... https://openaccess.thecvf.com/content/CVPR2022/papers/Zaheer_Generative_Cooperative_Learning_for_Unsupervised_Video_Anomaly_Detection_CVPR_2022_paper.pdf
[3] Vision-Language Models Assisted Unsupervised Video Anomaly ... https://arxiv.org/abs/2409.14109
[4] Detecting anomalies using robots with a new method of ... https://abysssolutions.co/case-studies/detecting-anomalies-using-robots-with-a-new-method-of-unsupervised-learning-ai/
[5] [PDF] Learning Anomalies with Normality Prior for Unsupervised Video ... https://www.ecva.net/papers/eccv_2024/papers_ECCV/papers/00941.pdf
[6] Semi-Supervised Anomaly Detection in Video-Surveillance Scenes ... https://www.mdpi.com/1424-8220/21/12/3993
[7] lucazanella/lavad: Official implementation of "Harnessing ... - GitHub https://github.com/lucazanella/lavad
[8] [PDF] New Frontiers in Ocean Exploration - The Oceanography Society https://tos.org/oceanography/assets/docs/32-1_supplement.pdf
[9] Weakly Supervised Video Anomaly Detection and Localization with ... https://arxiv.org/html/2408.05905v1
[10] Generative Cooperative Learning for Unsupervised Video Anomaly ... https://arxiv.org/abs/2203.03962
[11] Quo Vadis, Anomaly Detection? LLMs and VLMs in the Spotlight https://arxiv.org/html/2412.18298v1
[12] fjchange/awesome-video-anomaly-detection - GitHub https://github.com/fjchange/awesome-video-anomaly-detection
[13] Generative Cooperative Learning for Unsupervised Video Anomaly ... https://ieeexplore.ieee.org/document/9879680/
[14] Generative Cooperative Learning for Unsupervised Video Anomaly ... https://ksp.etri.re.kr/ksp/article/read?id=65660
[15] Open-Vocabulary Video Anomaly Detection - Papers With Code https://paperswithcode.com/paper/open-vocabulary-video-anomaly-detection
[16] [PDF] Criss-Crossing Creativity: - Oxford University Research Archive https://ora.ox.ac.uk/objects/uuid:1559cc77-2abb-448e-8ded-803a10aa4c0f/files/rcn69m499t
[17] [PDF] New Benchmark for Supervised Open-Set Video Anomaly Detection https://openaccess.thecvf.com/content/CVPR2022/papers/Acsintoae_UBnormal_New_Benchmark_for_Supervised_Open-Set_Video_Anomaly_Detection_CVPR_2022_paper.pdf
[18] AnilOsmanTur/conditioned_video_anomaly_diffusion - GitHub https://github.com/AnilOsmanTur/conditioned_video_anomaly_diffusion
[19] A Hybrid Approach to Improve the Video Anomaly Detection ... - MDPI https://www.mdpi.com/2079-3197/12/2/19
[20] (PDF) A Review on Financial Fraud Detection using AI and Machine ... https://www.researchgate.net/publication/378147101_A_Review_on_Financial_Fraud_Detection_using_AI_and_Machine_Learning
[21] Future Video Prediction from a Single Frame for Video Anomaly ... https://ui.adsabs.harvard.edu/abs/arXiv:2308.07783
[22] [ICIP 2023] Exploring Diffusion Models For Unsupervised Video ... https://github.com/AnilOsmanTur/video_anomaly_diffusion
[23] Vision-Language Models Assisted Unsupervised Video Anomaly ... https://bmvc2024.org/proceedings/599/
[24] Multi-instance learning anomaly event detection based on Transformer https://www.researchgate.net/publication/370821281_Multi-instance_learning_anomaly_event_detection_based_on_Transformer
[25] Weakly supervised video anomaly detection based on hyperbolic ... https://www.nature.com/articles/s41598-024-77505-4
[26] [PDF] Generative Cooperative Learning for Unsupervised Video Anomaly ... https://openaccess.thecvf.com/content/CVPR2022/papers/Zaheer_Generative_Cooperative_Learning_for_Unsupervised_Video_Anomaly_Detection_CVPR_2022_paper.pdf
[27] [PDF] Rethinking Video Anomaly Detection - A Continual Learning Approach https://openaccess.thecvf.com/content/WACV2022/papers/Doshi_Rethinking_Video_Anomaly_Detection_-_A_Continual_Learning_Approach_WACV_2022_paper.pdf
[28] Anomaly detection: DataRobot docs https://docs.datarobot.com/en/docs/modeling/special-workflows/unsupervised/anomaly-detection.html
[29] Weakly-Supervised Anomaly Detection in Surveillance Videos ... https://arxiv.org/html/2411.08755v1
[30] Deep Learning for Video Anomaly Detection: A Review - arXiv https://arxiv.org/html/2409.05383v1
[31] EVAL: Explainable Video Anomaly Localization - Semantic Scholar https://www.semanticscholar.org/paper/EVAL:-Explainable-Video-Anomaly-Localization-Singla/13edf6a2125465dd88376f3633cf0203de5258b8
[32] Limitations of Unsupervised Learning in Anomaly Detection - LinkedIn https://www.linkedin.com/advice/0/what-limitations-unsupervised-learning-anomaly-detection-nycof
[33] [PDF] Overlooked Video Classification in Weakly Supervised Video ... https://openaccess.thecvf.com/content/WACV2024W/RWS/papers/Tan_Overlooked_Video_Classification_in_Weakly_Supervised_Video_Anomaly_Detection_WACVW_2024_paper.pdf
[34] [PDF] Generalized Video Anomaly Event Detection - Semantic Scholar https://www.semanticscholar.org/paper/Generalized-Video-Anomaly-Event-Detection:-Taxonomy-Liu-Yang/bb51ca71833d42fa58f9adccb2296bdf665cc158
[35] Nasir Ahsan - Abyss Solutions Pty Ltd | LinkedIn https://www.linkedin.com/in/nasirahsan-abyss
[36] Unsupervised Anomaly Detection - Papers With Code https://paperswithcode.com/task/unsupervised-anomaly-detection/latest

