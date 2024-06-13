---
title: "로봇에서 사용하는 좌표계들"
excerpt_separator: "<!--more-->"
categories:
  - robot
tags:
  - robot

toc : true
toc_sticky : true
---

# 좌표계
로봇 제어를 하다 보면 키네메틱스를 풀게 되는데..
이때 각 좌표계에 대해서 배웠던것 같다.
살짝 개념 이해를 위해서 일단 페이지를 판다. 

## 카테시안

## 오일러

## 쿼니터안

## CLIK
close loop inverse kinematics  
```
*Close Loop Inverse Kinematics (CLIK)**는 로봇공학에서 로봇의 말단 효과기(end-effector)가 목표 위치와 자세에 도달하도록 관절 각도를 계산하는 알고리즘입니다. 일반적인 Inverse Kinematics와 다르게, CLIK는 피드백 제어를 통해 목표에 도달할 때까지 반복적으로 관절 각도를 조정합니다.

주요 특징과 원리

	1.	피드백 루프
	•	목표 위치와 현재 위치 사이의 오차를 지속적으로 계산하여 이를 줄이기 위해 관절 각도를 조정합니다.
	•	이는 실제 로봇이 목표에 도달할 때까지 관절 각도를 점진적으로 조정하는 방식으로 동작합니다.
	2.	반복적 접근
	•	초기 추정값에서 시작하여 반복적으로 목표와 현재 위치의 오차를 계산하고 관절 각도를 업데이트합니다.
	•	각 반복(iteration)마다 점진적으로 오차를 줄여나갑니다.
	3.	제이콥비안 행렬(Jacobian Matrix)
	•	로봇의 관절 각도 변화가 말단 효과기의 위치와 자세에 미치는 영향을 나타내는 행렬입니다.
	•	제이콥비안 행렬을 사용하여 오차를 줄이기 위해 필요한 관절 각도 변경량을 계산합니다.

알고리즘의 기본 단계

	1.	초기화
	•	초기 관절 각도 설정.
	•	목표 위치와 현재 위치의 초기 오차 계산.
	2.	제이콥비안 행렬 계산
	•	현재 관절 각도에서의 제이콥비안 행렬 ( J ) 계산.
	3.	오차 계산
	•	목표 위치 ( x_d )와 현재 위치 ( x ) 사이의 오차 ( e = x_d - x ) 계산.
	4.	관절 각도 변경량 계산
	•	오차를 줄이기 위해 필요한 관절 각도 변경량 ( \Delta \theta )를 계산. 이때, 제이콥비안 행렬의 역행렬 또는 의사 역행렬(pseudoinverse)을 사용:

\Delta \theta = J^+ e

	•	여기서  J^+ 는 제이콥비안 행렬의 의사 역행렬입니다.
	5.	관절 각도 업데이트
	•	새로운 관절 각도:

\theta_{new} = \theta_{current} + \Delta \theta

	6.	반복
	•	새로운 관절 각도에서의 위치와 자세 계산.
	•	오차가 허용 가능한 범위 내에 들어갈 때까지 반복.
	7.	종료
	•	목표 위치와 자세에 도달하면 알고리즘 종료.

장점

	•	정확성: 피드백 루프를 통해 목표 위치에 더 정확하게 도달할 수 있습니다.
	•	적응성: 환경 변화나 로봇의 동적 특성에 적응할 수 있습니다.
	•	유연성: 다양한 로봇 구조에 적용할 수 있습니다.

단점

	•	계산 비용: 반복적인 계산이 필요하여 실시간 응용에서는 계산 비용이 높을 수 있습니다.
	•	수렴 문제: 오차가 줄어들지 않거나 목표에 도달하지 못하는 경우가 발생할 수 있습니다.
```
예시 코드 (Python)
```python
import numpy as np

def jacobian_matrix(theta):
    # 제이콥비안 행렬 계산 (예시)
    # 실제 로봇의 kinematics에 따라 다름
    J = np.array([
        [-np.sin(theta[0]), np.cos(theta[0])],
        [np.cos(theta[0]), np.sin(theta[0])]
    ])
    return J

def inverse_kinematics(x_d, theta_init, max_iters=100, tol=1e-3):
    theta = theta_init
    for _ in range(max_iters):
        # 현재 위치 계산 (예시)
        x = np.array([np.cos(theta[0]), np.sin(theta[0])])
        e = x_d - x  # 오차 계산
        if np.linalg.norm(e) < tol:
            break
        J = jacobian_matrix(theta)
        J_pseudo_inv = np.linalg.pinv(J)  # 제이콥비안의 의사 역행렬
        delta_theta = J_pseudo_inv @ e  # 관절 각도 변경량 계산
        theta += delta_theta  # 관절 각도 업데이트
    return theta

# 목표 위치
x_d = np.array([1.0, 0.0])

# 초기 관절 각도
theta_init = np.array([0.5])

# Inverse Kinematics 계산
theta_solution = inverse_kinematics(x_d, theta_init)

print("Solution: ", theta_solution)
```

