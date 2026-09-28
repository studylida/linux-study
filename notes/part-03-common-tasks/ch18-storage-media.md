# Ch.18: Storage Media

## 오늘 배울 것

오늘은 **저장장치(디스크, USB 등)를 관리하는 방법**을 배울 거야. 리눅스에서 디스크를 연결하고, 확인하고, 포맷하고, 자동 마운트 설정하는 것까지!

---

## 마운트(Mount)란?

리눅스는 모든 게 하나의 파일 트리야. 윈도우처럼 `D:` 드라이브가 따로 보이지 않아. 저장장치를 쓰려면 트리의 어딘가에 **"붙여야(mount)"** 해:

```
/
├── home/
└── mnt/
    └── usb/   ← USB를 마운트하면 여기서 접근 가능
```

---

## 장치 이름 규칙

리눅스에서 저장장치는 `/dev/` 아래에 파일로 표현돼:

```
/dev/sda       ← 첫 번째 디스크 전체
/dev/sda1      ← 첫 번째 디스크의 1번 파티션
/dev/sdb       ← 두 번째 디스크 (USB 등)
/dev/sdb1      ← 두 번째 디스크의 1번 파티션
/dev/nvme0n1   ← NVMe SSD (최신 방식)
```

알파벳이 하나씩 증가하면서 디스크를 구분하고, 뒤의 숫자는 파티션 번호야.

---

## 상태 확인 명령어

```bash
lsblk          # 블록 장치를 트리 구조로 보여줌 (제일 보기 편해!)
df -h          # 마운트된 장치 + 용량 (human-readable)
mount          # 현재 마운트 목록 전체
```

`lsblk`를 먼저 쓰면 디스크와 파티션의 관계가 한눈에 보여.

---

## 마운트 / 언마운트

```bash
sudo mount /dev/sdb1 /mnt/usb     # 마운트
sudo umount /mnt/usb              # 언마운트 (unmount가 아니라 umount! n 없음!)
sudo umount /dev/sdb1             # 장치명으로도 가능
```

**주의**: `umount`야, `unmount`가 아니야! n이 빠져있어.

---

## 파티션 확인 — fdisk

```bash
sudo fdisk -l             # 모든 디스크/파티션 목록
sudo fdisk -l /dev/sda    # 특정 디스크만 보기
```

---

## 포맷 — mkfs (make filesystem)

```bash
sudo mkfs.ext4 /dev/sdb1    # ext4로 포맷 (리눅스 표준)
sudo mkfs.vfat /dev/sdb1    # FAT32로 포맷 (USB, 호환성용)
sudo mkfs.ntfs /dev/sdb1    # NTFS로 포맷 (윈도우 호환)
```

**주의**: 포맷하면 데이터 전부 날아감!

---

## UUID 확인

```bash
sudo blkid    # 각 파티션의 UUID 출력
```

UUID는 파티션의 **고유 ID**야. 장치명(`sda`, `sdb`)은 꽂는 순서에 따라 바뀔 수 있지만, UUID는 절대 안 바뀌어서 더 안전해.

---

## /etc/fstab — 자동 마운트 설정

부팅할 때 자동으로 마운트되게 하려면 `/etc/fstab` 파일에 설정을 추가해:

```
# 장치              마운트위치    파일시스템  옵션       dump  pass
/dev/sda1           /            ext4       defaults   0     1
UUID=xxxx-xxxx      /mnt/data    ext4       defaults   0     2
```

UUID를 쓰는 이유: 장치명은 꽂는 순서에 따라 바뀔 수 있지만, UUID는 파티션 고유 ID라서 항상 같은 장치를 가리켜.

---

## macOS와 비교

맥 사용자라면 이 비교가 도움될 거야:

| 개념 | 리눅스 | macOS |
|------|--------|-------|
| 물리 디스크 | `/dev/sda` | `/dev/disk0` |
| 파티션 | `/dev/sda1` | `/dev/disk0s1` |
| 디스크 목록 | `lsblk` | `diskutil list` |
| 마운트 상태 | `df -h` | `df -h` |
| 파티션 관리 | `fdisk` | `diskutil` |
| 포맷 | `mkfs.ext4` | `diskutil eraseVolume` |

### APFS 특이점 (macOS)

macOS의 APFS는 하나의 물리 파티션 안에서 여러 **볼륨**을 만들어. `synthesized`로 표시되는 disk가 논리적 컨테이너야.

---

## 파일시스템 종류

| 이름 | 용도 |
|------|------|
| ext4 | 리눅스 표준 |
| FAT32/vfat | USB, 크로스플랫폼 (4GB 파일 제한 있음) |
| exFAT | FAT32의 대용량 개선판 |
| NTFS | 윈도우 표준 |
| APFS | macOS 표준 |
| XFS | 대용량 서버용 |

---

## 정리

오늘 배운 핵심:
- **마운트** = 저장장치를 파일 트리에 붙이는 것
- `lsblk`로 장치 확인, `mount`/`umount`로 마운트/해제 (n 없음 주의!)
- `mkfs.ext4`로 포맷 (데이터 날아감!)
- **UUID** = 파티션 고유 ID → 장치명보다 안전
- `/etc/fstab`에 설정하면 부팅 시 자동 마운트
