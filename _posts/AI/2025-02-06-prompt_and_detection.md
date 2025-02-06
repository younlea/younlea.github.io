---
title: "프롬프트 기반 이상감지 솔루션셔"
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - write here

toc : true
toc_sticky : true
---

작업 현장에서 하나의 카메라로 이상 상황을 자동으로 감지하는 솔루션은 이미 개발되고 있으며, 다양한 기술적 접근법과 사례가 존재합니다. 아래는 관련 기술과 연구를 요약한 내용입니다:

## **기술적 접근**
1. **머신러닝 및 딥러닝 기반**:
   - Convolutional Neural Networks (CNNs)와 Long Short-Term Memory (LSTM) 네트워크를 활용하여 비정상적인 행동이나 사건을 감지하는 시스템이 개발되었습니다. 이러한 시스템은 CCTV 영상을 프레임 단위로 분석하며, 이상 상황이 감지되면 경고 메시지를 전송하거나 해당 이벤트를 고화질로 저장합니다[2][10].
   - 약하게 라벨링된 데이터(weakly labeled data)를 활용해 비정상적인 이벤트를 감지하는 방법도 제안되었습니다. 이 접근법은 비디오 레벨에서 정상/비정상 여부만 표시된 데이터를 사용하여 특정 시간대의 이상 상황을 자동으로 학습하고 감지합니다[7][16].

2. **엣지 컴퓨팅 및 저자원 환경**:
   - Raspberry Pi와 Jetson Nano 같은 엣지 디바이스에서 동작 가능한 경량화된 이상 탐지 시스템이 개발되었습니다. 이는 클라우드 기반 솔루션에 의존하지 않고, 로컬에서 실시간으로 데이터를 처리하여 성능과 자원 효율성을 높입니다[4].

3. **비지도 및 반지도 학습**:
   - 비지도 학습(Unsupervised Learning)과 반지도 학습(Semi-Supervised Learning)을 통해 사전에 정의되지 않은 다양한 유형의 이상 상황을 탐지할 수 있는 시스템이 구현되었습니다. 이는 기존의 지도 학습 방식보다 더 유연하게 작동하며, 새로운 상황에 적응할 수 있습니다[2][16].

## **적용 사례**
1. **산업 현장 및 작업장**:
   - 작업장에서 안전 위반, 장비 오작동, 또는 비정상적인 행동을 감지하여 사고를 예방하거나 대응 시간을 단축하는 데 사용됩니다[6][9].
   - 예를 들어, 특정 기계의 진동 패턴을 분석하거나 작업자의 행동을 모니터링하여 이상 징후를 포착하는 시스템이 제안되었습니다[19].

2. **공공 장소 및 교통 관리**:
   - 교통사고, 폭력 사건, 도난 등의 비정상적인 이벤트를 감지하기 위한 솔루션이 개발되어 공공 안전을 강화하고 있습니다[5][7].

3. **저조도 환경 개선**:
   - 저조도 환경에서도 이상 상황을 정확히 감지할 수 있도록 이미지 향상 기술(RUAS 및 Zero-DCE++ 등)이 적용된 시스템도 연구되고 있습니다[4].

## **한계와 도전 과제**
- **데이터 부족**: 현실 세계의 다양한 이상 상황을 충분히 반영하는 대규모 데이터셋의 부족이 문제로 지적됩니다[1][7].
- **오탐률 감소**: 정상 행동과 비정상 행동 간의 경계가 모호한 경우가 많아 높은 정확도를 유지하면서 오탐(false positive)을 줄이는 것이 과제입니다[7].
- **실시간 처리**: 고해상도 영상 데이터를 실시간으로 처리하려면 높은 계산 자원이 필요하며, 이를 해결하기 위한 경량화 기술이 요구됩니다[4].

현재 기술 발전 속도를 고려할 때, 하나의 카메라만으로도 작업장의 안전성과 효율성을 높이는 자동화된 이상 탐지 솔루션은 점점 더 정교해지고 실용적으로 구현되고 있습니다.

출처
[1] [PDF] An Anomaly Detection Dataset for Events in a Single Camera https://openaccess.thecvf.com/content/ACCV2020/papers/Pranav_A_Day_on_Campus_-_An_Anomaly_Detection_Dataset_for_ACCV_2020_paper.pdf
[2] [PDF] Abnormal Event Detection Using CCTV Camera https://ijisrt.com/assets/upload/files/IJISRT21JUN477.pdf
[3] Workplace Video Surveillance Policy: Do's and Don'ts | Kisi https://www.getkisi.com/blog/workplace-video-surveillance-policy
[4] Anomaly Detection on the Edge Using Smart Cameras under Low ... https://www.mdpi.com/1424-8220/24/3/772
[5] Automatic Detection of Abnormal Behaviour and Threats in crowded ... https://cordis.europa.eu/project/id/218197/reporting/fr
[6] 5 Ways Enterprise Surveillance Enhances Workplace Safety and ... https://www.lvt.com/blog/5-ways-enterprise-surveillance-enhances-workplace-safety-and-risk-management
[7] [PDF] Real-World Anomaly Detection in Surveillance Videos https://openaccess.thecvf.com/content_cvpr_2018/papers/Sultani_Real-World_Anomaly_Detection_CVPR_2018_paper.pdf
[8] Hybrid Video Anomaly Detection for Anomalous Scenarios in ... - arXiv https://arxiv.org/html/2406.06423v1
[9] Video surveillance systems to enhance occupational safety: A case ... https://www.researchgate.net/publication/288857885_Video_surveillance_systems_to_enhance_occupational_safety_A_case_study
[10] Top 7 Anomaly Detection Models for Video Surveillance - LinkedIn https://www.linkedin.com/pulse/top-7-anomaly-detection-models-video-surveillance-fora-soft-llc--aheze
[11] Towards Scalable Abnormal Behavior Detection in Automated ... https://ieeexplore.ieee.org/document/9565550/
[12] CCTV System - SINGLE CAMERA with 1 x 50m Cable - Prosol UK https://www.prosol.co.uk/product/cctv-system-single-camera-with-1-x-50m-cable/
[13] Advancing Video Anomaly Detection: A Concise Review and a New ... https://arxiv.org/html/2402.04857
[14] Real-time camera anomaly detection for real-world video surveillance https://www.researchgate.net/publication/224258100_Real-time_camera_anomaly_detection_for_real-world_video_surveillance
[15] IP Security Cameras | Bosch Security and Safety Systems I Global https://www.boschsecurity.com/xc/en/solutions/video-systems/ip-security-cameras/
[16] Semi-Supervised Anomaly Detection in Video-Surveillance Scenes ... https://www.mdpi.com/1424-8220/21/12/3993
[17] A Self-Adaptive Automatic Incident Detection System for Road ... https://www.mdpi.com/1424-8220/24/6/1822
[18] Video surveillance system (VSS) for construction sector - MOM https://www.mom.gov.sg/maintenance/index.html
[19] Single Centralized Overhead conceptual camera Solution. https://www.researchgate.net/figure/Single-Centralized-Overhead-conceptual-camera-Solution_fig5_332540368
[20] [PDF] ABNORMAL BEHAVIOR DETECTION IN AUTOMATED ... http://www.jatit.org/volumes/Vol95No19/29Vol95No19.pdf


프롬프트를 입력하여 작업장에서 이상 상황을 탐지하는 방식은 이미 연구되고 있으며, 다양한 솔루션이 제안되고 있습니다. 특히, 텍스트 기반 프롬프트와 비디오 데이터를 결합한 접근법이 주목받고 있습니다. 아래는 관련 기술과 사례를 요약한 내용입니다.

## **프롬프트 기반 이상 탐지 기술**
1. **Prompt-based Feature Mapping Framework (PFMF)**:
   - PFMF는 비디오 이상 탐지에서 프롬프트를 활용하여 정상적인 특징을 이상적인 특징 공간으로 매핑하는 네트워크를 제안합니다. 이 프레임워크는 가상 데이터셋에서 학습된 이상 프롬프트를 사용하여 실제 환경에서 발생할 수 있는 다양한 유형의 이상을 생성합니다. 이를 통해 기존 데이터셋의 한계를 극복하고, 장면별로 특화된 이상 상황을 감지할 수 있습니다[1].

2. **Learn Suspected Anomalies from Event Prompts (LAP)**:
   - LAP는 텍스트 기반 이벤트 프롬프트를 이용해 비디오에서 의심스러운 이상 상황을 학습하는 프레임워크입니다. 잠재적인 이상 이벤트를 설명하는 텍스트 프롬프트 사전을 구축하고, 이를 비디오 캡션과 비교하여 이상 유사도를 계산합니다. 이 방식은 시각적-의미적 특징을 결합하여 더 세밀한 이상 탐지를 가능하게 합니다[2][3].

3. **Few-shot 및 Zero-shot 학습**:
   - 일부 연구는 CLIP과 같은 모델을 활용해 텍스트 프롬프트와 시각적 데이터를 결합하여 소량의 데이터로도 새로운 이상 상황을 탐지할 수 있는 방법론을 제안합니다. 이러한 접근법은 작업장에서 사전 정의되지 않은 새로운 이상 행동도 감지할 수 있는 가능성을 제공합니다[7].

## **기술적 장점**
- **유연성**: 텍스트 프롬프트를 통해 사용자가 감지하려는 특정 이상 상황(예: "작업자가 쓰러짐", "장비 오작동")을 정의할 수 있습니다.
- **확장성**: 기존 데이터셋에 없는 새로운 유형의 이상 상황도 프롬프트를 통해 학습 및 탐지가 가능합니다.
- **효율성**: 약지도 학습(weakly supervised learning)이나 소량의 데이터로도 높은 성능을 낼 수 있습니다.

## **적용 가능성**
이러한 기술은 작업장 안전 모니터링, 공공 안전, 스마트 시티와 같은 다양한 분야에 적용될 수 있습니다. 특히 작업장에서 특정 이벤트(예: 기계 고장, 작업자 부상 등)를 자동으로 감지하고 경고를 제공하는 데 유용합니다.

따라서, 프롬프트 기반 접근법은 단일 카메라로 작업장의 이상 상황을 탐지하는 데 매우 효과적인 솔루션으로 자리 잡고 있으며, 앞으로 더 많은 발전이 기대됩니다.

출처
[1] [PDF] Generating Anomalies for Video Anomaly Detection With Prompt ... https://openaccess.thecvf.com/content/CVPR2023/papers/Liu_Generating_Anomalies_for_Video_Anomaly_Detection_With_Prompt-Based_Feature_Mapping_CVPR_2023_paper.pdf
[2] Learn Suspected Anomalies from Event Prompts for Video ... - arXiv https://arxiv.org/html/2403.01169v1
[3] Learning Suspected Anomalies from Event Prompts for Video ... - arXiv https://arxiv.org/html/2403.01169v2
[4] Anomaly Detection on the Edge Using Smart Cameras under Low ... https://www.mdpi.com/1424-8220/24/3/772
[5] aoso3/Real-Time-Abnormal-Events-Detection-and-Tracking-in ... https://github.com/aoso3/Real-Time-Abnormal-Events-Detection-and-Tracking-in-Surveillance-System
[6] [PDF] An Anomaly Detection Dataset for Events in a Single Camera https://openaccess.thecvf.com/content/ACCV2020/papers/Pranav_A_Day_on_Campus_-_An_Anomaly_Detection_Dataset_for_ACCV_2020_paper.pdf
[7] [PDF] 2024 겨울 세미나 https://vds.sogang.ac.kr/wp-content/uploads/2024/01/2024_%EA%B2%A8%EC%9A%B8%EC%84%B8%EB%AF%B8%EB%82%98_%EA%B9%80%ED%98%84%EB%B9%88.pdf
[8] Anomaly Detection In Surveillance Videos - Papers With Code https://paperswithcode.com/task/anomaly-detection-in-surveillance-videos/latest
[9] Deep Learning-Based Anomaly Detection in Video Surveillance https://www.mdpi.com/1424-8220/23/11/5024
[10] Detection of Unusual Activity in Surveillance Video Scenes Based ... https://www.researchgate.net/publication/360001352_Detection_of_Unusual_Activity_in_Surveillance_Video_Scenes_Based_on_Deep_Learning_Strategies
[11] Anomaly Detection in Videos for Video Surveillance Applications ... https://www.researchgate.net/publication/343751790_Anomaly_Detection_in_Videos_for_Video_Surveillance_Applications_using_Neural_Networks
[12] Why Use AI for Video Anomaly Detection? - Fora Soft https://www.forasoft.com/blog/article/machine-learning-realtime-monitoring
[13] [PDF] llm-assisted real-time anomaly detection for safe visual navigation https://arxiv.org/pdf/2403.12415.pdf
[14] jnagidi/DeepEYE-Video-Surveillance-with-Anomaly-Detection https://github.com/jnagidi/DeepEYE-Video-Surveillance-with-Anomaly-Detection
[15] Top 7 Anomaly Detection Models for Video Surveillance - LinkedIn https://www.linkedin.com/pulse/top-7-anomaly-detection-models-video-surveillance-fora-soft-llc--aheze
[16] [PDF] Anomaly Detection in Workstation Using Deep Learning Techniques https://easychair.org/publications/preprint/4xz8/open
[17] Anomaly behavior detection analysis in video surveillance: a critical ... https://www.spiedigitallibrary.org/journals/Anomaly-behavior-detection-analysis-in-video-surveillance--a-critical/volume-32/issue-04/042106/Anomaly-behavior-detection-analysis-in-video-surveillance--a-critical/10.1117/1.JEI.32.4.042106.full
[18] An Analysis of Artificial Intelligence Techniques in Surveillance ... https://www.mdpi.com/2076-3417/13/8/4956
[19] Advancing Video Anomaly Detection: A Concise Review and a New ... https://arxiv.org/html/2402.04857
[20] Weakly Supervised Video Anomaly Detection and Localization with ... https://dl.acm.org/doi/10.1145/3664647.3681442


### **출시 시기 및 기술 발전**
프롬프트 기반 이상 탐지 기술은 최근 몇 년간 급격히 발전했습니다. 주요 연구와 프레임워크는 다음과 같은 시점에 발표되었습니다:

1. **Prompt-based Feature Mapping Framework (PFMF)**:
   - 2023년 CVPR에서 발표된 PFMF는 비디오 이상 탐지에서 프롬프트를 활용하여 이상 상황을 감지하는 초기 연구 중 하나입니다[10].

2. **Human-Free Prompt-Based Anomaly Detection**:
   - 2024년 6월에 발표된 이 연구는 데이터 기반으로 학습 가능한 프롬프트를 사용하여 인간 개입 없이 이상 상황을 탐지하는 프레임워크를 제안했습니다[1][8].

3. **FAPrompt**:
   - 2025년 1월 ICLR에서 발표된 FAPrompt는 세밀한 이상 탐지를 위해 복합적인 프롬프트 학습을 도입하여 기존 기술보다 3~5% 높은 성능을 달성했습니다[2][22].

4. **AdaCLIP**:
   - 2024년 7월 ECCV에서 소개된 AdaCLIP은 CLIP 모델에 학습 가능한 정적 및 동적 프롬프트를 통합하여 Zero-Shot Anomaly Detection(ZSAD) 성능을 향상시켰습니다[5].

---

### **현재 상용화된 솔루션**
다양한 기업이 프롬프트 기반 또는 AI 기반 이상 탐지 솔루션을 상용화하고 있습니다:

1. **LeewayHertz**:
   - LeewayHertz는 NSG Group과 협력하여 유리 제조 공정에서 실시간 이상 탐지를 위한 맞춤형 컴퓨터 비전 솔루션을 개발했습니다. 이 시스템은 생산 효율성을 최적화하고 품질을 향상시키는 데 사용됩니다[37].

2. **Grid Dynamics**:
   - Grid Dynamics는 대량의 데이터를 처리하며 실시간으로 이상 패턴을 감지하고 근본 원인 분석을 자동화하는 AI 기반 솔루션을 제공합니다[33].

3. **Craftworks**:
   - Craftworks는 클라우드 서비스와 오픈소스 머신러닝 기술을 활용하여 맞춤형 이상 탐지 솔루션을 개발합니다. 이 회사는 공급망 관리, 바이오테크 등 다양한 산업에 적용 가능한 솔루션을 제공합니다[34].

4. **ITRex Group**:
   - ITRex Group은 머신러닝과 데이터 마이닝 기술을 활용해 프로세스 모니터링 및 이상 탐지 시스템을 구축하며, 고객 맞춤형 서비스를 제공합니다[34].

---

### **결론**
프롬프트 기반 이상 탐지 기술은 2023년 이후 빠르게 발전했으며, 현재 상용화 단계에 접어들고 있습니다. LeewayHertz, Grid Dynamics, Craftworks와 같은 기업들이 실질적인 응용 사례를 통해 산업 전반에 걸쳐 이러한 기술의 가능성을 입증하고 있습니다.

출처
[1] Human-Free Automated Prompting for Vision-Language Anomaly ... https://arxiv.org/abs/2406.18197
[2] Fine-grained Abnormality Prompt Learning for Zero-shot Anomaly... https://openreview.net/forum?id=kS27PPs3yR
[3] Human-free Prompted Based Anomaly Detection https://arxiv.org/html/2406.18197v1
[4] Efficient anomaly detection in tabular cybersecurity data using large ... https://www.nature.com/articles/s41598-025-88050-z
[5] [ECCV2024] The Official Implementation for ''AdaCLIP - GitHub https://github.com/caoyunkang/AdaCLIP
[6] One-for-All Few-Shot Anomaly Detection via Instance-Induced ... https://openreview.net/forum?id=Zzs3JwknAY
[7] Log-based Anomaly Detection Using Large Language Models - arXiv https://arxiv.org/html/2411.08561v1
[8] Human-Free Automated Prompting for Vision-Language Anomaly ... https://paperswithcode.com/paper/human-free-prompted-based-anomaly-detection
[9] AAD-LLM: Adaptive Anomaly Detection Using Large Language ... https://arxiv.org/html/2411.00914v1
[10] CVPR 2023 Open Access Repository https://openaccess.thecvf.com/content/CVPR2023/html/Liu_Generating_Anomalies_for_Video_Anomaly_Detection_With_Prompt-Based_Feature_Mapping_CVPR_2023_paper.html
[11] Non-Pattern-Based Anomaly Detection in Time-Series - MDPI https://www.mdpi.com/2079-9292/12/3/721
[12] [PDF] History-based Anomaly Detector: an Adversarial Approach to ... https://www.semanticscholar.org/paper/History-based-Anomaly-Detector:-an-Adversarial-to-Chatillon-Ballester/23ae911ceb9134f3a27719faab5d5d297a2f4e8e
[13] [PDF] Generating Anomalies for Video Anomaly Detection With Prompt ... https://openaccess.thecvf.com/content/CVPR2023/papers/Liu_Generating_Anomalies_for_Video_Anomaly_Detection_With_Prompt-Based_Feature_Mapping_CVPR_2023_paper.pdf
[14] Can Large Language Models be Anomaly Detectors for Time Series? https://ieeexplore.ieee.org/document/10722786/
[15] A Review of Machine Learning based Anomaly Detection Techniques https://www.researchgate.net/publication/253239339_A_Review_of_Machine_Learning_based_Anomaly_Detection_Techniques
[16] [paper] Unsupervised Continual Anomaly Detection with ... - velog https://velog.io/@qtly_u/paper-Unsupervised-Continual-Anomaly-Detection-with-Contrastively-learned-Prompt
[17] M-3LAB/awesome-industrial-anomaly-detection - GitHub https://github.com/M-3LAB/awesome-industrial-anomaly-detection?tab=readme-ov-file+https%3A%2F%2Fgithub.com%2Fcqylunlun%2Fglass%3Ftab%3Dreadme-ov-file
[18] AI Anomaly Detection: Applications and Challenges in 2024 https://www.techmagic.co/blog/ai-anomaly-detection/
[19] [PDF] 2024 여름 세미나 https://vds.sogang.ac.kr/wp-content/uploads/2024/06/2024_%EC%97%AC%EB%A6%84%EC%84%B8%EB%AF%B8%EB%82%98_%EC%9D%B4%EC%A4%80%ED%98%B8.pdf
[20] A Log-based Anomaly Detection Framework Using Prompts https://ieeexplore.ieee.org/document/10191948/
[21] Generating Anomalies for Video Anomaly Detection with Prompt ... https://ieeexplore.ieee.org/document/10205098/
[22] Fine-grained Abnormality Prompt Learning for Zero-shot Anomaly... https://openreview.net/forum?id=kS27PPs3yR
[23] Best Anomaly Detection Solutions for 2025 | PeerSpot https://www.peerspot.com/categories/anomaly-detection-tools
[24] What's Happening in Anomaly Detection Technologies? (Feb 2024) https://www.startus-insights.com/innovators-guide/whats-currently-happening-in-anomaly-detection-technologies/
[25] Anomaly Detection | craftworks https://www.craftworks.ai/solutions/anomaly-detection/
[26] Anomaly Detection Research - AI Prompt - DocsBot AI https://docsbot.ai/prompts/research/anomaly-detection-research
[27] [PDF] PromptAD: Zero-Shot Anomaly Detection Using Text Prompts https://openaccess.thecvf.com/content/WACV2024/papers/Li_PromptAD_Zero-Shot_Anomaly_Detection_Using_Text_Prompts_WACV_2024_paper.pdf
[28] Build an AI-Powered Anomaly Detection Application for E ... https://dev.to/lizzzzz/build-an-ai-powered-anomaly-detection-application-for-e-commerce-analytics-2fj
[29] Anomaly Detection Companies | Market Research Future https://www.marketresearchfuture.com/reports/anomaly-detection-market/companies
[30] Human-free Prompted Based Anomaly Detection - ResearchGate https://www.researchgate.net/publication/381737177_Human-free_Prompted_Based_Anomaly_Detection_prompt_optimization_with_Meta-guiding_prompt_scheme
[31] AI-Powered Anomaly Detection 2024 Ultimate Guide | Boost Efficiency https://www.rapidinnovation.io/post/ai-in-anomaly-detection-for-businesses
[32] Human-free Prompted Based Anomaly Detection https://arxiv.org/html/2406.18197v1
[33] Anomaly Detection Solutions with Data Science - Grid Dynamics https://www.griddynamics.com/solutions/anomaly-detection
[34] Anomaly Detection Services & Solutions - ITRex Group https://itrexgroup.com/services/anomaly-detection/
[35] Fine-grained Abnormality Prompt Learning for Zero-shot Anomaly ... https://arxiv.org/html/2410.10289v1
[36] Log-based Anomaly Detection Using Large Language Models - arXiv https://arxiv.org/html/2411.08561v1
[37] Computer Vision-Based Anomaly Detection System - LeewayHertz https://www.leewayhertz.com/computer-vision-based-anomaly-detection-system/
