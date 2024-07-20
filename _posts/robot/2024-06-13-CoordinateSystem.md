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
```  


# 자코비안 행렬식 
자코비안 행렬은 로봇의 관절 공간(joint space)와 작업공간(task space)사이의 속도 관계를 나타내는 행렬입니다.   
일반적으로 6xn 행렬로 표시(3차원 위치 + 3차원 방향, n: 관절수) 
자코비안은 어렸을적(?)에 배워서 로봇 포워드 키네메틱스를 풀어서 opengl로 시뮬레이터 만들때 썼던 녀석인것 같다. ^^;    

# FK(forward kinematics) & IK(inverse kinematics)
## FK
로봇의 각 관절 각도나 변위가 주어졌을때, 엔드 이펙터의 위치와 방향을 계산하는 과정    
일대일 대응이 된다.   
주로 시뮬레이터를 만들거나 로봇의 현재위치 추정, 작업공간 분석에서 쓰인다.    
기억이 맞으면 자코비안 행렬식으로 풀었던것 같다. 한땀 한땀.. 라떼는 말이다..     

## IK
원하는 엔드 이펙터의 위치와 방향이 주어졌을때, 이를 달성하기 위한 각 관절의 각도나 변위를 계산하는 과정. 
일대 다.. 그러니까 해가 여러가 나올수 있다.    
로봇의 경로 계획이나 목표 지점으로의 이동, 실시간 로봇 제어에 사용한다. 
실제로 로봇을 제어 할때 쓰는 방법인데 한땀한땀 손으로 푸는건 이제 안하는것 같고 아래 솔루션들을 사용해서 많이들 쓰는것 같다.    

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
[moveit homepage](https://moveit.ros.org/)     
주요 기능 : 모션 계획, 조작, 3D인식, 키네메틱스, 제어 및 네비게이션   
IK 솔버 : KDL, SRVision, TRAC-IK등 옵션 제공   

## drake  
[drake homepage](https://drake.mit.edu/)     
[drake github](https://github.com/RobotLocomotion)   
[drake movie guide](https://drake.mit.edu/gallery.html)     
MIT 로봇 조작 연구실에서 개발한 고급 로보틱스 및 동적 시스템 시뮬레이션 툴킷   
주요 기능 : 동영학 시뮬레이션, 모션 플레닝, 비선형 최적화, 제어 시스템 설계 및 분석, 포워드 및 인버스 키네메틱스    

## orocos KDL   
[orocos hompepage](https://www.orocos.org/index.html)    
[orocos github](https://github.com/orocos)    
주요 기능 : FK, IK, 동역학 계산   
솔버 : 수치적 IK(뉴턴-랩슨 방법)   

## RBDL(Rigid Body Dynamics Library)   
[RBDL](https://rbdl.github.io/index.html)      
주요 기능 : FK, IK, 동역학 계산   
솔버 : 수치적 IK   

## OpenLave
[openlave homepage](http://openrave.org/)    
[openlave github](https://github.com/rdiankov/openrave/tree/master?tab=readme-ov-file)   

# 경로(trajectory) 생성 방법
1. 선형 보간법 : 직선 경로 생성, (ROS : joint_trajectory_controller로 제공)   
2. 다항식 트라젝토리(polynomial trajectory), 3차, 5차 다항식들을 사용한 부드러운 경로 생성    
3. SLERP(spherical linear interpolation) 회전 보간에 사용, Eigen, Opencv등 다수의 수학라이브러리로 제공    
4. RRT(rapidly-exploring random trees) 랜덤 샘플링 기반 경로 계획, (OMPL이나 Moveit 제공)    
5. RRT*(최적화된 RRT)    
6. PRM(probabilistic Roadmap) 사전 계산된 로드맵 기반 경로 계획(OMPL이나 Moveit제공)
7. CHOMP(Covariant Hamiltonian Optimization for Motion Planning) 최적화 기반 경로 계획 (Moveit 제공)
8. STOMP(Stochastic Trajectory Optimization for Motion Planning) 확률적 최적화 기반 경로 계획(Moveit 제공)

