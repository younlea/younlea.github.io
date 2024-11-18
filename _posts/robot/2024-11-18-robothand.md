---
title: "휴머노이드 hand 구분"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

현재 제공된 이미지를 표에 직접 붙이는 것은 불가능하지만, 각 휴머노이드 핸드의 구조와 손가락 개수별 분류에 맞는 이미지를 찾을 수 있는 링크를 제공할 수 있습니다. 아래는 각 구조 유형과 손가락 개수에 따른 분류와 해당 이미지를 확인할 수 있는 링크입니다.

## 1. **모터 직접 구동형 (Motor-Direct-Driven)**

### 5지 핸드:
- **Robonaut 2 (R2)**: 5개의 손가락을 가진 NASA의 로봇으로, 모터가 직접 손가락을 구동합니다.
  - [Robonaut 2 이미지 링크](https://robotsguide.com/robots/robonaut)

- **Shadow Dexterous Hand**: 5개의 손가락을 가진 고도로 정교한 로봇 손으로, 각 손가락이 독립적으로 제어됩니다.
  - [Shadow Dexterous Hand 이미지 링크](https://robotsguide.com/robots/shadow)

### 제어 방식:
- **힘 피드백**과 **위치 피드백**을 사용하여 정교한 손가락 움직임을 제어합니다.

---

## 2. **건(腱) 구동형 (Tendon-Driven)**

### 5지 핸드:
- **DEXMART**: 인간 손과 유사한 5지 구조로 설계된 로봇 핸드로, 건 구동 방식을 사용합니다.
  - [DEXMART 이미지 링크](https://www.robaid.com/robotics/project-dexmart-sensitive-robotic-hand.htm)

### 기타:
- **DLR Hand II**: 엄지가 없는 4지 구조로, 건 구동 방식을 통해 높은 유연성과 정확성을 제공합니다.
  - [DLR Hand II 이미지 링크](https://www.robotic.de/fileadmin/robotic/borst/BorstEtAl-ICRA03-HandApplications.pdf)

### 제어 방식:
- 건 구동형 시스템은 **텐션 센서**를 통해 케이블의 장력을 측정하고 제어합니다.

---

## 3. **링크 구동형 (Linkage-Driven)**

### 5지 핸드:
- **ILDA Hand**: 인간 손을 모방한 5지 구조로, 링크 기반 설계를 통해 높은 효율성을 제공합니다.
  - [ILDA Hand 이미지 링크](https://www.nature.com/articles/s41467-021-27261-0)

### 기타:
- **T-FLoW 3.0 Hand**: 두 개의 주요 손가락(엄지와 검지)을 사용하는 2지 구조로, 간단한 그립 동작에 최적화되어 있습니다.
  - [T-FLoW Hand 이미지 링크](https://joiv.org/index.php/joiv/article/view/1793/0)

### 제어 방식:
- 링크 구동형 시스템은 **위치 피드백**과 함께 **힘 토크 센서**를 사용하여 각 링크의 움직임과 접촉력을 측정하고 제어합니다.

---

## 요약: 구조 및 손가락 개수별 분류

| 구조 유형          | 대표적인 예시                           | 손가락 개수 | 이미지 링크                              |
|-------------------|----------------------------------------|------------|----------------------------------------|
| 모터 직접 구동형   | Robonaut 2, Shadow Dexterous Hand      | 5지        | [Robonaut 2](https://robotsguide.com/robots/robonaut), [Shadow Dexterous Hand](https://robotsguide.com/robots/shadow) |
| 건(腱) 구동형      | DLR Hand II (4지), DEXMART             | DLR: 4지, DEXMART: 5지 | [DLR Hand II](https://www.robotic.de/fileadmin/robotic/borst/BorstEtAl-ICRA03-HandApplications.pdf), [DEXMART](https://www.robaid.com/robotics/project-dexmart-sensitive-robotic-hand.htm) |
| 링크 구동형        | ILDA Hand (5지), T-FLoW 3.0 Hand (2지) | ILDA: 5지, T-FLoW: 2지 | [ILDA Hand](https://www.nature.com/articles/s41467-021-27261-0), [T-FLoW Hand](https://joiv.org/index.php/joiv/article/view/1793/0) |

이 표를 참고하여 각 휴머노이드 로봇 핸드의 구조와 손가락 개수에 따라 분류된 이미지를 확인할 수 있습니다.

출처
[1] image https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/31412864/c9319c9a-9bb4-400e-820f-6ba556b985f0/0/image.png
[2] [PDF] The Robonaut 2 Hand – Designed To Do Work With Tools https://ntrs.nasa.gov/api/citations/20110023122/downloads/20110023122.pdf
[3] Shadow Hand - ROBOTS: Your Guide to the World of Robotics https://robotsguide.com/robots/shadow
[4] [PDF] DLR Hand II: Experiments and Experiences with an ... https://www.robotic.de/fileadmin/robotic/borst/BorstEtAl-ICRA03-HandApplications.pdf
[5] Project DEXMART sensitive robotic hand - RobAid https://www.robaid.com/robotics/project-dexmart-sensitive-robotic-hand.htm
[6] Integrated linkage-driven dexterous anthropomorphic robotic hand https://www.nature.com/articles/s41467-021-27261-0
[7] Concept and Design of Anthropomorphic Robot Hand with a Finger ... https://joiv.org/index.php/joiv/article/view/1793/0
[8] Robonaut 2 - ROBOTS: Your Guide to the World of Robotics https://robotsguide.com/robots/robonaut


현재 휴머노이드 로봇 기술에서 손과 관련된 주목할 만한 로봇과 기술을 소개합니다. 아래는 관련 정보와 영상 링크입니다:
	1.	테슬라 옵티머스(Optimus Gen-2)
테슬라의 최신 휴머노이드 로봇으로, 손가락의 촉각 감지와 정밀한 물체 조작이 가능하도록 개선되었습니다. 계란을 깨지 않고 들 수 있는 정도로 섬세한 작업을 수행할 수 있습니다. 이 로봇은 산업 및 일반 환경 모두에서 활용 가능하도록 설계되었습니다【8】【9】.
	2.	보스턴 다이내믹스 아틀라스(Atlas)
인간형 운동성을 가진 로봇으로, 복잡한 작업을 수행할 수 있는 고급 센서와 제어 시스템을 갖추고 있습니다. 특히 빠른 동작과 유연성이 강조되어 건설 및 물류 작업 등 다양한 환경에서 사용됩니다【9】【10】.
	3.	포이어 로봇(Fourier GR-1)
생체 모방형 설계와 뛰어난 균형 조절 기술로 사람과 협업할 수 있는 로봇입니다. 주로 의료 재활 분야에서 활용되고 있으며, 다양한 산업 및 가정용 응용도 고려되고 있습니다【10】.
	4.	알로하(Aloha) 로봇
오픈소스 하드웨어 기반으로 설계된 로봇으로, 요리와 같은 가사 작업 수행이 가능합니다. 이 로봇은 독립 작업과 원격 조작 기능을 혼합하여 다양한 환경에서 유용성을 보여주고 있습니다【8】.
	5.	XPeng PX5
XPeng Motors에서 개발한 양족 보행 로봇으로, 복잡한 지형에서도 안정적으로 걷고 장애물을 극복할 수 있습니다. 주로 실내외 탐색 및 일상 활동에 적합하도록 설계되었습니다【10】.
