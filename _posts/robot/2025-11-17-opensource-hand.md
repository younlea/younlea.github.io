---
title: "OpenSource hand project"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---
# 🤖 오픈소스 로봇 핸드 프로젝트 (URDF/STL 제공)

로봇 핸드 개발에 참고할 수 있는 훌륭한 오픈소스 프로젝트들을 소개합니다. 이 프로젝트들은 **URDF**와 **STL** 파일이 모두 공개되어 있어 RViz나 MuJoCo 같은 시뮬레이터에서 바로 테스트해볼 수 있습니다.

---

## 1. Yale OpenHand Project

![Yale OpenHand Model O](https://grablab.yale.edu/sites/default/files/styles/project_main/public/2021-03/model_o_main_0.png?itok=z5_G-l_f)

* **특징:** 예일 대학교(Yale University)의 GRAB Lab에서 진행하는 프로젝트로, 3D 프린팅을 기반으로 한 다양한 저가형 로봇 핸드 모델을 제공합니다.
* **제공 파일:** STL (3D 프린팅용), URDF (시뮬레이션용), 하드웨어 조립 가이드, 제어 코드 등이 공개되어 있습니다.
* **링크:**
    * **[Yale GRAB Lab OpenHand 공식 페이지](https://grablab.yale.edu/openhand)** (가장 공식적인 정보가 있는 곳입니다.)
    * **[OpenHand URDF/STL 미러 리포지토리 (JHU)](https://github.com/jhu-lcsr/open_hand_project_model)** (원본 리포지토리가 잘 찾아지지 않아, 존스 홉킨스 대학에서 관리하는 URDF 모델 미러 링크로 대체합니다.)

---

## 2. InMoov Hand

![InMoov Hand](http://inmoov.fr/wp-content/uploads/2018/11/HandV3-500x375.jpg)

* **특징:** 전신을 3D 프린터로 제작하는 휴머노이드 로봇 'InMoov'의 손 부분입니다. 전 세계적으로 가장 유명한 3D 프린팅 로봇 프로젝트 중 하나입니다.
* **제공 파일:** **STL** 파일이 매우 상세하게 제공되며, InMoov 로봇 전체의 **URDF** 파일에 손 부분이 포함되어 있습니다.
* **링크:**
    * **[InMoov 공식 웹사이트](http://inmoov.fr/)**
    * **[InMoov V2 GitHub 리포지토리](https://github.com/InMoov/inmoov2)** (이전 InMoov 리포지토리에서 V2로 업데이트되었습니다.)

---

## 3. 다양한 로봇 URDF 모음 (KIT 핸드 대체)

이전에 알려드린 KIT-GRAS Lab의 GitHub 조직 링크(`KIT-GRAS`)가 404 상태입니다. 대신, 다양한 로봇의 URDF와 시뮬레이션 모델을 모아둔 매우 유용한 리포지토리를 추천해 드립니다.

* **특징:** 수많은 상용 및 연구용 로봇(핸드, 암, 모바일 로봇 등)의 URDF, Xacro, MJCF(MuJoCo) 모델 파일이 체계적으로 정리되어 있습니다.
* **링크:**
    * **[Awesome Robot Descriptions](https://github.com/robot-descriptions/awesome-robot-descriptions)** (다양한 로봇 모델 리스트)
    * **[robot_descriptions.py](https://github.com/robot-descriptions/robot_descriptions.py)** (이 리스트의 로봇들을 MuJoCo, PyBullet 등에서 쉽게 로드할 수 있게 해주는 파이썬 라이브러리)

---

## 💡 시뮬레이션용 벤치마크 모델

### Robotiq Grippers (예: 2F-85)

* **특징:** 산업용 및 연구용으로 매우 널리 쓰이는 그리퍼입니다.
* **제공 파일:** ROS용 `robotiq_description` 패키지에 URDF와 STL 파일이 포함되어 있습니다.
* **링크:**
    * **[ros-industrial-attic/robotiq GitHub](https://github.com/ros-industrial-attic/robotiq)** (기존 `ros-industrial`에서 `ros-industrial-attic` (보관용)으로 이동되었습니다. 링크가 깨졌던 원인입니다.)

### Shadow Robot Hand (Shadow C6M Hand)

* **특징:** 매우 정교하고 자유도가 높은(20+ DoF) 로봇 핸드입니다.
* **제공 파일:** 시뮬레이션용 모델(**URDF/STL**)이 `sr_common` 또는 `sr_hand_description` 같은 패키지에 포함되어 있습니다.
* **링크:**
    * **[shadow-robot GitHub (조직 페이지)](https://github.com/shadow-robot)** (이전 링크의 `-company` 부분이 빠져야 했습니다. 이 조직 페이지에서 `sr_common` 등 관련 리포지토리를 찾으실 수 있습니다.)

---

## 🚀 검색 및 활용 팁

1.  **GitHub 검색:** "robot hand urdf", "gripper stl description", "dexterous hand ros" 등의 키워드로 검색해 보세요.
2.  **ROS 패키지:** ROS(Robot Operating System)를 사용하는 프로젝트는 시뮬레이션 모델을 `[robot_name]_description`이라는 패키지로 관리하는 경우가 많습니다. 이 패키지 안의 `urdf` 폴더와 `meshes/stl` 폴더를 확인해 보세요.
3.  **MuJoCo 변환:** URDF 파일은 MuJoCo에서 직접 로드하거나 XML 형식으로 변환하여 사용할 수 있습니다.


https://youtu.be/ZUz2HDuI77E

