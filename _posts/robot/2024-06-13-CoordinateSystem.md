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
직교 좌표계 (x,y,z)   

## 오일러
roll, pitch, yaw로 표현되고 회전에 특화된 좌표계, 짐벌락 이슈가 있다.     

## 쿼터니안
오일러 좌표계에 있는 짐벌락이슈가 없는 좌표계. (사람이 인지하기 쉽지 않다는 단점이 있지만 현재 나온 계산 솔루션들이 해결해 주니까 ㅡ.ㅡ; 이걸로 쓰는게 좋을듯)    

-> 참고로 쿼터니안도 싱귤러리티 이슈는 가지고 있다고 한다. (짐벌락은 싱귤러리티에 포함이 되는 개념이다.)    

```
쿼터니언(Quaternion)은 3차원 회전을 표현하는 수학적 도구로, 4개의 실수 성분으로 구성된 4차원 복소수입니다. 쿼터니언은 회전의 연산이 효율적이고, 각 변환 간의 계산이 수월하여 3D 그래픽스, 로봇 공학, 항공 우주 등 다양한 분야에서 널리 사용됩니다.

쿼터니언의 정의

쿼터니언  q 는 다음과 같이 표현됩니다:
 q = w + xi + yj + zk 
여기서  w, x, y, z 는 실수이고,  i, j, k 는 허수 단위입니다. 이는 다음과 같이 벡터와 스칼라 부분으로 나눌 수 있습니다:
 q = (w, \mathbf{v}) 
여기서  \mathbf{v} = (x, y, z) 는 벡터 부분,  w 는 스칼라 부분입니다.

쿼터니언의 기본 연산

	1.	덧셈:
두 쿼터니언  q_1 = (w_1, \mathbf{v}_1) 와  q_2 = (w_2, \mathbf{v}_2) 의 덧셈은 성분별로 수행됩니다:
 q_1 + q_2 = (w_1 + w_2, \mathbf{v}_1 + \mathbf{v}_2) 
	2.	곱셈:
두 쿼터니언  q_1 과  q_2 의 곱셈은 다음과 같이 정의됩니다:
 q_1 q_2 = (w_1 w_2 - \mathbf{v}_1 \cdot \mathbf{v}_2, w_1 \mathbf{v}_2 + w_2 \mathbf{v}_1 + \mathbf{v}_1 \times \mathbf{v}_2) 
여기서  \mathbf{v}_1 \cdot \mathbf{v}_2 는 벡터의 내적,  \mathbf{v}_1 \times \mathbf{v}_2 는 벡터의 외적입니다.
	3.	켤레:
쿼터니언  q 의 켤레는 다음과 같이 정의됩니다:
 q^* = (w, -\mathbf{v}) 
	4.	노름(Norm):
쿼터니언  q 의 노름은 다음과 같이 계산됩니다:
 \|q\| = \sqrt{w^2 + x^2 + y^2 + z^2} 
	5.	역수:
쿼터니언  q 의 역수는 다음과 같이 계산됩니다:
 q^{-1} = \frac{q^*}{\|q\|^2} 

3D 회전을 위한 쿼터니언

쿼터니언을 사용하여 3D 공간에서의 회전을 표현할 수 있습니다. 회전을 표현하는 단위 쿼터니언  q 는 노름이 1인 쿼터니언으로, 다음과 같이 정의됩니다:
 q = \cos\left(\frac{\theta}{2}\right) + \sin\left(\frac{\theta}{2}\right) (xi + yj + zk) 
여기서  \theta 는 회전 각도,  (x, y, z) 는 회전 축의 단위 벡터입니다.

벡터 회전

벡터  \mathbf{v} 를 쿼터니언  q 로 회전시키려면, 먼저 벡터를 순수 쿼터니언으로 변환한 후, 다음과 같은 쿼터니언 곱셈을 사용합니다:
 \mathbf{v}{\prime} = q \mathbf{v} q^* 
여기서  \mathbf{v} 는  (0, \mathbf{v}) 로 표현된 순수 쿼터니언입니다.

쿼터니언의 장점

	1.	간결성: 쿼터니언은 회전 행렬(9개의 요소)보다 적은 4개의 요소만으로 회전을 표현할 수 있습니다.
	2.	누적 오류 감소: 쿼터니언을 사용하면 수치적 안정성이 높아지고, 연속된 회전 연산에서 누적 오류가 적습니다.
	3.	슬러빅 현상 방지: 오일러 각을 사용할 때 발생할 수 있는 기하학적 특이점 문제(슬러빅 현상)을 피할 수 있습니다.
```
예시 코드 (Python)

다음은 쿼터니언을 사용하여 벡터를 회전시키는 Python 예제입니다:
```python
import numpy as np

def quaternion_multiply(q1, q2):
    w1, x1, y1, z1 = q1
    w2, x2, y2, z2 = q2
    return np.array([
        w1*w2 - x1*x2 - y1*y2 - z1*z2,
        w1*x2 + x1*w2 + y1*z2 - z1*y2,
        w1*y2 - x1*z2 + y1*w2 + z1*x2,
        w1*z2 + x1*y2 - y1*x2 + z1*w2
    ])

def quaternion_conjugate(q):
    w, x, y, z = q
    return np.array([w, -x, -y, -z])

def rotate_vector(v, q):
    q_v = np.concatenate(([0], v))
    q_conj = quaternion_conjugate(q)
    q_v_rotated = quaternion_multiply(quaternion_multiply(q, q_v), q_conj)
    return q_v_rotated[1:]

# 예시 회전: 90도 (π/2 라디안) 회전 축 (0, 0, 1)
theta = np.pi / 2
axis = np.array([0, 0, 1])
q = np.concatenate(([np.cos(theta / 2)], np.sin(theta / 2) * axis))

# 회전할 벡터
v = np.array([1, 0, 0])

# 벡터 회전
v_rotated = rotate_vector(v, q)

print("Original vector:", v)
print("Rotated vector:", v_rotated)
```

# 자코비안 행렬식 

# FK & IK
## FK
## IK
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
# solution 
## Moveit
## OpenLave
## orocos KDL
## RBDL(Rigid Body Dynamics Library)
[RBDL](https://rbdl.github.io/index.html)      
## drake  

# 경로 생성 방법

# 충돌회피 경로 생성 방법 및 솔루션
