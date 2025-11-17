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
* **주요 모델:**
    * **Model T:** 간단하고 내구성이 좋은 4축(underactuated) 그리퍼입니다.
    * **Model O:** 좀 더 복잡한 작업을 위해 설계된 모델입니다.
* **제공 파일:** **STL** (3D 프린팅용), **URDF** (시뮬레이션용), 하드웨어 조립 가이드, 제어 코드 등 거의 모든 자료가 공개되어 있습니다.
* **링크:**
    * **[Yale GRAB Lab OpenHand 공식 페이지](https://grablab.yale.edu/openhand)**
    * **[Yale OpenHand GitHub 리포지토리](https://github.com/Yale-GRAB-Lab/OpenHand)**

---

## 2. InMoov Hand

![InMoov Hand](http://inmoov.fr/wp-content/uploads/2018/11/HandV3-500x375.jpg)

* **특징:** 전신을 3D 프린터로 제작하는 휴머노이드 로봇 'InMoov'의 손 부분입니다. 전 세계적으로 가장 유명한 3D 프린팅 로봇 프로젝트 중 하나입니다.
* **구조:** 사람의 손과 유사한 5개의 손가락을 가지고 있으며, 서보 모터와 힘줄(tendon) 방식으로 구동됩니다.
* **제공 파일:** **STL** 파일이 매우 상세하게 제공되며, InMoov 로봇 전체의 **URDF** 파일에 손 부분이 포함되어 있습니다.
* **링크:**
    * **[InMoov 공식 웹사이트](http://inmoov.fr/)**
    * **[InMoov GitHub 리포지토리](https://github.com/InMoov/InMoov)**

---

## 3. KIT (Karlsruhe Institute of Technology) 핸드

* **특징:** 독일 칼스루에 공과대학(KIT)에서 개발한 여러 로봇 핸드 모델이 있습니다. 특히 인간의 손과 유사한 고성능 로봇 핸드 연구로 유명합니다.
* **제공 파일:** 연구용으로 공개된 모델들의 `_description` 패키지(ROS)에 **URDF**와 **STL**(주로 `meshes` 폴더) 파일이 포함된 경우가 많습니다.
* **링크:**
    * **[KIT-GRAS Lab GitHub](https://github.com/KIT-GRAS)** (KIT의 Grasping and RObotics Lab 리포지토리로, 다양한 로봇 및 그리퍼 프로젝트의 시작점입니다.)

---

## 💡 시뮬레이션용 벤치마크 모델

개발 프로젝트는 아니지만, 시뮬레이션 환경(MuJoCo, RViz)에서 벤치마크나 참고용으로 가장 많이 쓰이는 로봇 핸드들의 URDF/STL입니다.

### Robotiq Grippers (예: 2F-85, 3-Finger Gripper)

* **특징:** 산업용 및 연구용으로 매우 널리 쓰이는 그리퍼입니다.
* **제공 파일:** ROS용 `robotiq_description` 패키지에 URDF와 STL 파일이 포함되어 있습니다.
* **링크:**
    * **[ros-industrial/robotiq GitHub](https://github.com/ros-industrial/robotiq)**

### Shadow Robot Hand (Shadow C6M Hand)

* **특징:** 매우 정교하고 자유도가 높은(20+ DoF) 로봇 핸드입니다.
* **제공 파일:** 하드웨어는 상용이지만, 시뮬레이션용 모델(**URDF/STL**)은 `shadow_robot_ethercat` 리포지토리 내의 `shadow_hand_description` 패키지 등을 통해 공개되어 있습니다.
* **링크:**
    * **[shadow-robot-company/shadow_robot GitHub](https://github.com/shadow-robot-company/shadow_robot)** (관련 ROS 패키지들을 포함하는 메타 리포지토리입니다.)

---

## 🚀 검색 및 활용 팁

1.  **GitHub 검색:** "robot hand urdf", "gripper stl description", "dexterous hand ros" 등의 키워드로 검색해 보세요.
2.  **ROS 패키지:** ROS(Robot Operating System)를 사용하는 프로젝트는 시뮬레이션 모델을 `[robot_name]_description`이라는 패키지로 관리하는 경우가 많습니다. 이 패키지 안의 `urdf` 폴더와 `meshes/stl` 폴더를 확인해 보세요.
3.  **MuJoCo 변환:** URDF 파일은 MuJoCo에서 직접 로드하거나 XML 형식으로 변환하여 사용할 수 있습니다.

