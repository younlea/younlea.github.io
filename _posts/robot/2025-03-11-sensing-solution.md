---
title: "실제 환경 인식 솔루션 비교"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

# 실제 환경 인식 솔루션 비교

아래는 SAM2.1과 최근 발표된 주요 환경 인식 솔루션들의 특징과 공식 홈페이지 링크를 정리한 내용입니다.

---

## 1. **SAM2.1**
- **특징**: 이미지 및 비디오 세분화에 최적화된 모델로, 실시간 객체 추적과 세분화가 가능.
- **필요 리소스**: GPU 기반 학습, 대규모 데이터셋 필요.
- **성능**: 다양한 도메인에서 높은 일반화 성능 제공, 가려진 객체와 유사한 객체도 정밀하게 세분화.
- **오픈소스 여부**: 오픈소스 (Apache 2.0 라이선스).
- **홈페이지 링크**: [Meta SAM2.1 공식 페이지](https://ai.meta.com/blog/segment-anything-2/)

---

## 2. **SVNet 3D Perception Network**
- **특징**: 2D 카메라 데이터를 3D 환경 맵으로 변환하여 자율주행 및 ADAS에 최적화.
- **필요 리소스**: 일반 카메라 및 SurroundVision 지원.
- **성능**: 다양한 기후와 복잡한 환경에서 높은 적응력 제공.
- **오픈소스 여부**: 비공개 (상용 솔루션).
- **홈페이지 링크**: [StradVision SVNet 페이지](https://autotechinsight.ihsmarkit.com/main/news/proc/create-pdf?id=5279601)

---

## 3. **RGo Robotics Perception Engine**
- **특징**: Visual SLAM과 AI를 결합하여 실시간 위치 추적 및 장애물 감지.
- **필요 리소스**: 카메라, IMU, GNSS 등 다양한 센서 활용 가능.
- **성능**: 동적 환경에서도 인간 수준의 3D 인식 제공.
- **오픈소스 여부**: 비공개 (상용 솔루션).
- **홈페이지 링크**: [RGo Robotics Perception Engine](https://www.roboticstomorrow.com/article/2023/05/ai-powered-perception-engine-for-mobile-robots/20505)

---

## 4. **oToSLAM**
- **특징**: 멀티 카메라 기반 SLAM으로 LiDAR 없이 정밀한 위치 추적 및 매핑 가능.
- **필요 리소스**: 4개의 자동차용 카메라와 저전력 ECU 필요.
- **성능**: 최대 1cm 정확도의 위치 추적 가능, 실내외 모두에서 작동.
- **오픈소스 여부**: 비공개 (상용 솔루션).
- **홈페이지 링크**: [oToSLAM Vision-AI Positioning System](https://www.otobrite.com/product/otoslam-vision-ai-positioning-system)

---

## 5. **YOLO 기반 Object Detection**
- **특징**: 단일 RGB 카메라만으로 실시간 객체 탐지 및 위치 정보 제공.
- **필요 리소스**: 단일 RGB 카메라.
- **성능**: 빠른 처리 속도와 높은 객체 탐지 정확도 제공.
- **오픈소스 여부**: 오픈소스 (GitHub에서 제공).
- **홈페이지 링크**: [YOLOv5 GitHub 페이지](https://github.com/ultralytics/yolov5)
