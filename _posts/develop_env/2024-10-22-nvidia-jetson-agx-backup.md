---
title: "nvidia jetson agx backup and resore method "
excerpt_separator: "<!--more-->"
categories:
  - develop_env
tags:
  - write here

toc : true
toc_sticky : true
---

NVIDIA Jetson AGX 시스템에 **Timeshift**와 같은 데스크톱용 백업 솔루션을 사용하는 것은 적절하지 않을 수 있습니다. Timeshift는 일반적으로 x86 기반 리눅스 시스템에서 사용되며, Jetson AGX는 ARM 아키텍처 기반이기 때문에 호환성 문제나 최적화된 사용이 어려울 수 있습니다.

Jetson AGX 시스템에서 백업 및 복구를 위해 사용하는 몇 가지 일반적인 방법은 다음과 같습니다:

### 1. **NVIDIA 제공 Flashing 도구**
   NVIDIA에서 제공하는 Jetson Flashing 도구를 사용하여 시스템을 백업하고 복구하는 방법이 있습니다. 이를 통해 Jetson AGX 시스템에 설치된 소프트웨어 이미지를 플래시할 수 있습니다.
   - **NVIDIA SDK Manager**: SDK Manager를 사용하여 Jetson AGX에 새로운 이미지를 플래시하거나, 커스텀 이미지를 만들어 백업을 할 수 있습니다.

### 2. **Disk/Partition Clone (dd 명령어)**
   `dd` 명령어를 사용하여 Jetson AGX의 전체 디스크나 특정 파티션을 백업할 수 있습니다. 이 방법은 원시 디스크 복제를 기반으로 하므로 모든 데이터를 그대로 보존합니다.
   - 예시:
     ```bash
     sudo dd if=/dev/mmcblk0 of=/path/to/backup.img
     ```
     이를 통해 eMMC나 외장 스토리지에 있는 데이터를 이미지 파일로 백업하고, 필요시 복원할 수 있습니다.

### 3. **TegraFlash**
   NVIDIA의 Tegra 장치를 지원하는 TegraFlash 도구를 사용하여 Jetson 시스템의 펌웨어나 시스템 이미지를 백업 및 복구할 수 있습니다.

### 4. **Jetson Cloning Script**
   Jetson 플랫폼에서 실행할 수 있는 다양한 스크립트를 사용해 전체 시스템을 백업하는 방법도 있습니다. 예를 들어, **jetson-backup**과 같은 커뮤니티 제작 스크립트를 사용할 수 있습니다.

### 5. **NVIDIA의 오피셜 도큐먼트**
   Jetson AGX를 위한 공식적인 백업 및 복구 절차는 NVIDIA의 Jetson 포럼 및 문서에서 확인할 수 있습니다. NVIDIA는 개발자들이 백업 및 복구를 용이하게 할 수 있는 여러 가지 가이드를 제공합니다.

이 방법들 중 하나를 선택하여 Jetson AGX 시스템을 백업하고 복구할 수 있습니다. Timeshift는 Jetson의 아키텍처와 맞지 않을 수 있으므로 위의 방법들이 더 적합할 것입니다.
