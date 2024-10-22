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


# dd를 이용한 백업 방법으로 진행. 
NVMe SSD(`nvme0n1`)를 마운트한 후, 이 디스크에 Jetson AGX 시스템의 eMMC 또는 특정 파티션을 `dd`로 백업하고 복원하는 절차를 순서대로 설명드리겠습니다.

### 순서대로 진행할 명령어들:

#### 1. **NVMe SSD 확인**
먼저, 시스템에 연결된 NVMe SSD(`nvme0n1`)가 올바르게 인식되었는지 확인해야 합니다.

```bash
lsblk
```

출력에서 `nvme0n1` 디스크가 인식되었는지 확인하세요. 예시:
```
NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
mmcblk0      179:0    0  14.7G  0 disk
├─mmcblk0p1  179:1    0   7.4G  0 part /
nvme0n1      259:0    0 465.8G  0 disk
```

#### 2. **파티션 생성 및 파일 시스템 설정 (필요시)**

만약 **NVMe SSD가 파티션되지 않았거나** 파일 시스템이 없는 경우, 먼저 파티션을 생성하고 파일 시스템을 만들어야 합니다.

1) **파티션 생성**:
```bash
sudo fdisk /dev/nvme0n1
```
- `n`을 눌러 새 파티션을 생성하고 기본 설정을 따릅니다.
- `w`를 눌러 변경 사항을 저장하고 종료합니다.

2) **파일 시스템 생성**:
파티션이 `nvme0n1p1`로 생성되었다면, 파일 시스템을 ext4로 만듭니다.

```bash
sudo mkfs.ext4 /dev/nvme0n1p1
```

#### 3. **NVMe SSD 마운트**

이제 NVMe SSD를 사용할 수 있도록 마운트합니다.

1) **마운트 디렉토리 생성**:
   마운트할 디렉토리를 만듭니다 (예: `/mnt/nvme`).

   ```bash
   sudo mkdir -p /mnt/nvme
   ```

2) **NVMe SSD 마운트**:
   `nvme0n1p1` 파티션을 `/mnt/nvme`에 마운트합니다.

   ```bash
   sudo mount /dev/nvme0n1p1 /mnt/nvme
   ```

3) **마운트 확인**:
   제대로 마운트되었는지 확인합니다.

   ```bash
   df -h
   ```

출력에서 `/mnt/nvme`가 표시되면 NVMe SSD가 정상적으로 마운트된 것입니다.

#### 4. **eMMC 백업 (dd 명령어 사용)**

이제 Jetson AGX 시스템의 eMMC를 NVMe SSD에 백업합니다.

1) **eMMC 전체를 백업**:
   eMMC 디스크가 `/dev/mmcblk0`로 표시되어 있다면, 전체 eMMC를 백업할 수 있습니다.

   ```bash
   sudo dd if=/dev/mmcblk0 of=/mnt/nvme/mmcblk0_backup.img bs=4M status=progress
   ```

   - `if`: 백업할 원본 디스크 (`/dev/mmcblk0`).
   - `of`: 백업할 이미지 파일 경로 (`/mnt/nvme/mmcblk0_backup.img`).
   - `bs=4M`: 블록 크기(속도를 최적화).
   - `status=progress`: 복사 진행 상황을 실시간으로 확인.

2) **특정 파티션만 백업**:
   만약 루트 파티션(`mmcblk0p1`)만 백업하려면 다음과 같이 실행합니다.

   ```bash
   sudo dd if=/dev/mmcblk0p1 of=/mnt/nvme/mmcblk0p1_backup.img bs=4M status=progress
   ```

#### 5. **복원 (dd 명령어 사용)**

백업 파일을 NVMe SSD에서 eMMC로 복원하려면 다음 절차를 따릅니다.

1) **eMMC 전체 복원**:
   eMMC 전체를 복원하려면 다음과 같이 합니다.

   ```bash
   sudo dd if=/mnt/nvme/mmcblk0_backup.img of=/dev/mmcblk0 bs=4M status=progress
   ```

2) **특정 파티션만 복원**:
   루트 파티션(`mmcblk0p1`)만 복원하려면 다음 명령을 사용합니다.

   ```bash
   sudo dd if=/mnt/nvme/mmcblk0p1_backup.img of=/dev/mmcblk0p1 bs=4M status=progress
   ```

#### 6. **마운트 해제 및 종료**

복원이 완료된 후, NVMe SSD를 마운트 해제합니다.

```bash
sudo umount /mnt/nvme
```

### 요약:
1. `lsblk`로 NVMe SSD 확인.
2. 필요한 경우 NVMe SSD에 파티션 생성 및 파일 시스템 설정.
3. NVMe SSD를 `/mnt/nvme`에 마운트.
4. `dd` 명령어로 eMMC를 백업하고, 필요한 경우 복원.
5. 복원 후 NVMe SSD 마운트 해제.

이 과정을 통해 Jetson AGX의 eMMC 또는 특정 파티션을 NVMe SSD에 백업하고 복원할 수 있습니다.
