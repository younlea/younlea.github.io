---
title: "로봇 AI 동작 이상감지 솔루션"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

# AI 기반 로봇 이상 감지 및 충돌 제어 솔루션

산업 현장에서 로봇의 활용이 증가함에 따라, 로봇의 이상 동작 및 충돌을 실시간으로 감지하고 제어하는 AI 기반 솔루션의 중요성이 부각되고 있습니다. 이러한 솔루션들은 하드웨어와 소프트웨어의 통합을 통해 구현되며, 로봇의 안전성과 효율성을 높이는 데 기여하고 있습니다.

## 1. 아하랩스의 LISA 비디오 이상 탐지 솔루션

- **하드웨어**: 로봇의 작업 과정을 촬영하는 비디오 카메라.
- **소프트웨어**: 아하랩스의 산업용 AI 솔루션인 LISA는 비디오 데이터를 분석하여 이상 동작을 감지합니다. 정상 상태의 데이터를 학습한 딥러닝 알고리즘을 통해 실시간으로 로봇의 비정상적인 움직임을 탐지합니다.  
  [공식 사이트](https://ahha.ai/2023/12/22/case_lisavideo_anomaly/?utm_source=chatgpt.com)
- **방법**: 정상적인 로봇 동작의 비디오 데이터를 기반으로 모델을 학습시킨 후, 실시간으로 입력되는 비디오 스트림을 분석하여 이상 징후를 감지합니다.

---

## 2. 세이지리서치의 SaigeVIMS

- **하드웨어**: IP 카메라를 통해 로봇의 작업 환경을 모니터링합니다.
- **소프트웨어**: 기계 학습 기반의 지능형 공정 모니터링 시스템인 SaigeVIMS는 실시간 영상 분석을 통해 로봇의 이상 동작을 감지합니다.  
  [공식 사이트](https://automation-world.co.kr/mobile/article.html?no=66387&utm_source=chatgpt.com)
- **방법**: 카메라로 수집된 영상을 실시간으로 분석하여 로봇의 비정상적인 동작이나 오류를 탐지하고, 이를 통해 즉각적인 대응이 가능합니다.

---

## 3. 써로마인드의 AI 기반 이상 감지 솔루션

- **하드웨어**: 음향, 진동, 전류, 온도 등의 데이터를 수집하는 다양한 센서.
- **소프트웨어**: 써로마인드의 AI 솔루션은 수집된 센서 데이터를 분석하여 로봇 설비의 고장과 장애 여부를 진단하고, 보수 시기를 예측합니다.  
  [공식 블로그](https://m.blog.naver.com/surromind/223303341753?utm_source=chatgpt.com)
- **방법**: 센서로부터 수집된 데이터를 AI 모델이 학습하여 정상 상태와 비정상 상태를 구분하고, 이상 징후를 조기에 감지하여 예방적 유지보수가 가능하게 합니다.

---

## 4. AIDIN Robotics의 힘/토크 센서 기반 충돌 감지 솔루션

- **하드웨어**: 6축 힘/토크 센서 및 초박형 관절 토크 센서.
- **소프트웨어**: 실시간 로봇 모션 제어기와 통합되어 로봇의 힘과 토크 데이터를 모니터링합니다.  
  [공식 문서](https://www.aidinrobotics.co.kr/_files/ugd/ba4131_a5b2bd5a70fb418eacd565420041efb9.pdf?index=true&utm_source=chatgpt.com)
- **방법**: 로봇의 관절과 그리퍼에 장착된 힘/토크 센서를 통해 실시간으로 접촉력과 토크를 측정하여, 예상치 못한 충돌이나 이상 동작을 감지하고 즉각적으로 제어합니다.

---

# 로봇 공정의 비전 기반 이상 감지 솔루션

로봇 공정에서 비전 시스템을 활용한 이상 감지 솔루션은 생산 효율성과 품질 향상에 크게 기여하고 있습니다. 아래는 최근 발표된 주요 솔루션들의 하드웨어 및 소프트웨어 구성과 발표 일자를 정리한 내용입니다.

## 1. OnRobot의 2.5D 비전 시스템 '아이즈(Eyes)'

**발표일**: 2020년 10월 15일

**하드웨어**: '아이즈'는 다양한 로봇 암 또는 독립형 장치와 호환되는 2.5D 비전 시스템으로, 무작위로 배치된 물체를 감지하고 다양한 높이의 물체나 스택된 물체를 위한 심도 인식 기능을 제공합니다. [oai_citation_attribution:3‡onrobot.com](https://onrobot.com/ko/jepum/onrobot-eyes?utm_source=chatgpt.com)

**소프트웨어**: 소프트웨어 업데이트를 통해 복수의 물체에 대한 '원-샷 감지' 기능과 특정 작업물 유형을 요청하는 그리퍼의 클리어런스 조건 설정 등 기능이 추가되었습니다. [oai_citation_attribution:2‡fajournal.com](https://www.fajournal.com/news/articleView.html?idxno=9321&utm_source=chatgpt.com)

## 2. ISRA VISION의 Q-GAGE3D

**발표일**: 2025년 1월 22일

**하드웨어**: Q-GAGE3D는 초소형 3D 로봇 가이드 센서로, 루프, 도어, 후드 및 플랩에 대한 실링 공정을 로봇이 자동으로 정밀하게 가이드할 수 있도록 설계되었습니다. [oai_citation_attribution:1‡isravision.com](https://www.isravision.com/ko-kr/products/q-gage3d?utm_source=chatgpt.com)

**소프트웨어**: 센서 시스템은 미세하고 눈에 보이는 심(seam)을 적용하기 위해 차체 부품의 위치와 방향을 결정하며, 센서가 감지한 위치 편차를 로봇에 제공하여 심을 정확하게 도포할 수 있도록 안내합니다.

## 3. ISRA VISION의 MiniPICK3D

**발표일**: 2025년 1월 22일

**하드웨어**: MiniPICK3D는 자동화된 휠 장착 애플리케이션을 위한 3D 로봇 가이던스 시스템으로, 로봇에 장착된 센서를 통해 휠 허브의 위치를 3D로 측정합니다. [oai_citation_attribution:0‡isravision.com](https://www.isravision.com/ko-kr/products/minipick3d?utm_source=chatgpt.com)

**소프트웨어**: 센서 시스템은 로봇을 가이드하고 위치를 지정하기 위해 휠 허브의 위치를 3D로 측정하고 장착 지점의 정확한 위치를 계산합니다. 그런 다음 로봇은 이 데이터를 사용하여 툴로 휠을 장착합니다.

