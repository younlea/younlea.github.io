# [MuJoCo] 시뮬레이션으로 로봇 모터 선정 검증하기 (Motor Sizing)

로봇을 설계할 때 가장 고민되는 부분 중 하나는 **"이 모터가 과연 내가 원하는 동작을 버틸 수 있을까?"** 입니다. 너무 약한 모터를 쓰면 움직이지 않고, 너무 강한 모터를 쓰면 무겁고 비싸집니다.

오늘은 MuJoCo 시뮬레이터를 활용해 **정해진 동작(Trajectory)을 수행시킴으로써 모터 사양을 검증하는 방법**을 정리해 보았습니다.

---

## 1. 개요: 시뮬레이션의 목적

단순히 동작이 되는지만 보는 것이 아니라, **현실적인 모터의 한계값(토크, 속도, 관성 등)**을 시뮬레이터에 입력하고, 로봇이 이 동작을 따라갈 때 모터가 비명을 지르는지(?) 확인하는 것이 목표입니다.

**핵심 흐름:**
1.  **Input:** 모터 데이터시트의 값을 XML(`mjcf`)에 입력
2.  **Process:** 레코딩된 동작(궤적)을 강제로 수행 (PD 제어)
3.  **Output:** 토크 포화, 위치 오차, 발열(RMS) 그래프 분석

---

## 2. Input: XML에 입력해야 할 모터 파라미터

모터 데이터시트(Spec Sheet)를 보고 아래 5가지 값을 MJCF 파일의 `<actuator>`와 `<joint>` 태그에 정확히 입력해야 합니다.

### 필수 파라미터 리스트

| 파라미터 | XML 속성 | 설명 및 계산법 | 중요도 |
| :--- | :--- | :--- | :--- |
| **최대 토크** | `forcelim` | **Stall Torque** × 기어비 × 효율(0.8)<br>`<actuator forcelim="..." ctrllimited="true">` | ⭐⭐⭐⭐⭐<br>모터 힘의 물리적 한계 설정. 이 이상 힘을 못 쓰게 막음. |
| **감속비** | `gear` | 기어비 (예: 100:1 → `100`)<br>`<actuator gear="...">` | ⭐⭐⭐⭐<br>모터 토크를 증폭시키는 비율. |
| **최대 속도** | `velocity` | **No-load RPM** × 0.1047 (rad/s)<br>`<actuator velocity="...">` | ⭐⭐⭐⭐<br>모터가 낼 수 있는 속도 한계. |
| **로터 관성** | `armature` | **Rotor Inertia** × 기어비²<br>`<joint armature="...">` | ⭐⭐⭐<br>손가락처럼 가벼운 링크 구동 시, 가속 성능 검증에 필수. |
| **마찰력** | `frictionloss` | 정격 토크의 약 5~10%<br>`<joint frictionloss="...">` | ⭐⭐<br>기어박스 내부 저항(Loss) 반영. |

### XML 작성 예시

```xml
<mujoco>
  <worldbody>
    <body name="link_1">
      <joint name="joint_1" axis="1 0 0" armature="0.005" frictionloss="0.1" />
      <geom ... />
    </body>
  </worldbody>

  <actuator>
    <position name="motor_1" joint="joint_1" kp="50" kv="1" 
              gear="100" velocity="5.2" forcelim="1.5" ctrllimited="true"/>
  </actuator>
</mujoco>
```

---

## 3. Process: 검증 시뮬레이션 수행

정해진 궤적(Target Trajectory)을 로봇에게 입력하고, 실제 로봇이 잘 따라오는지 데이터를 수집합니다.

```python
import mujoco
import numpy as np
import matplotlib.pyplot as plt

# 모델 로드
model = mujoco.MjModel.from_xml_path("my_robot.xml")
data = mujoco.MjData(model)

# 검증할 동작 데이터 (예: N개의 스텝)
# trajectory = [0.1, 0.2, 0.3, ... ] 

# 데이터 수집용 리스트
log_target = []
log_actual = []
log_torque = []
log_speed = []

# 시뮬레이션 루프
for target_pos in trajectory:
    # 1. 명령 전달 (PD 제어 목표값 설정)
    data.ctrl[0] = target_pos
    
    # 2. 물리 연산 (Step)
    mujoco.mj_step(model, data)
    
    # 3. 데이터 로깅
    log_target.append(target_pos)
    log_actual.append(data.qpos[0]) # 현재 각도
    log_torque.append(data.actuator_force[0]) # 현재 토크
    log_speed.append(data.qvel[0]) # 현재 속도

# (이후 그래프 분석 진행)
```

---

## 4. Output: 결과를 보고 판단하는 법 (Pass/Fail)

수집된 데이터를 그래프로 그렸을 때, 다음 4가지를 확인하여 모터 선정의 적합성을 판단합니다.

### ① 토크 포화 (Torque Saturation) 확인
가장 중요한 지표입니다. 그래프가 평평하게 잘린 구간이 있는지 확인하세요.

* **Check:** `data.actuator_force` 그래프가 `forcelim` 값에 도달했는가?
* **판단:**
    * 📉 **잘림 발생:** **Fail.** 해당 구간에서 모터 힘이 부족하여 원하는 동작을 수행 불가. (더 센 모터 필요)
    * 〰️ **여유 있음:** **Pass.**

### ② 위치 추종 오차 (Tracking Error) 확인
토크가 부족하거나 속도가 부족하면 동작이 밀립니다.

* **Check:** `Target` 그래프와 `Actual` 그래프의 차이(Error)
* **판단:**
    * **오차가 큼:** 제어 게인(Kp)을 올려본다.
    * **Kp를 올려도 오차가 안 줄어듬:** **Fail.** (모터의 토크나 속도 한계에 도달한 것임)

### ③ 동작 영역 (T-N Curve) 확인
현실 모터는 속도가 빠르면 토크를 못 냅니다.

* **Check:** X축(속도) - Y축(토크) 산점도(Scatter Plot)를 그립니다.
* **판단:**
    * 모든 점이 모터 제조사가 제공하는 **속도-토크 곡선(T-N Curve) 안쪽**에 들어와야 **Pass.**
    * 우상단(고속+고토크) 영역에 점이 찍히면 현실에서는 전압 부족으로 구현 불가.

### ④ RMS 토크 (발열 체크)
순간적인 힘은 세더라도, 계속 큰 힘을 쓰면 모터가 타버립니다.

* **Check:** 토크 데이터의 RMS(제곱평균제곱근) 값 계산.
* **판단:**
    * **RMS 값 < 정격 토크 (Continuous Torque):** **Pass (안전)**
    * **RMS 값 > 정격 토크:** **Fail (과열 위험)**

---

## 5. 결론

MuJoCo에서 `Kp`, `Kv`만 조절해서 동작을 만드는 것은 반쪽짜리 시뮬레이션입니다. **`forcelim` (토크 한계)** 과 **`armature` (관성)** 등 물리적 제약 조건을 정확히 입력했을 때, 비로소 **"이 모터 사도 되나요?"** 에 대한 답을 얻을 수 있습니다.

> **Tip:** 시뮬레이션은 실제보다 약간 보수적으로(Limit을 조금 더 낮게) 잡고 돌리는 것이 안전합니다.
