# Ch.17: Package Management

## 오늘 배울 것

오늘은 리눅스에서 **프로그램을 설치하고 관리하는 방법**인 패키지 매니저를 배울 거야. 스마트폰의 앱스토어처럼, 리눅스에도 프로그램을 쉽게 설치/삭제/업데이트하는 시스템이 있어!

---

## 핵심 개념 3가지

### 패키지(Package)
프로그램을 설치 가능한 형태로 묶어놓은 파일이야. 실행 파일 + 설정 파일 + 문서 + 의존성 정보가 들어있어.

### 의존성(Dependency)
프로그램 A가 동작하려면 라이브러리 B가 필요한 관계. 패키지 매니저가 자동으로 파악해서 함께 설치해줘.

### 저장소(Repository)
패키지들이 모여있는 온라인 서버야. 앱스토어와 같은 개념이야. 공식 저장소, 커뮤니티 저장소, 서드파티 저장소 등이 있어.

---

## 저수준 vs 고수준 도구

- **저수준** (`dpkg`, `rpm`): 패키지 파일 하나를 직접 설치/삭제. 의존성 자동 해결 안 됨
- **고수준** (`apt`, `yum`/`dnf`): 저장소에서 검색, 의존성 자동 해결 — **보통 이걸 씀!**

---

## 두 가지 계열

리눅스 배포판에 따라 다른 패키지 시스템을 써:

| | 데비안(Debian) 계열 | 레드햇(Red Hat) 계열 |
|--|--|--|
| **배포판** | Ubuntu, Debian, Mint | RHEL, CentOS, Fedora, Rocky |
| **패키지 형식** | `.deb` | `.rpm` |
| **저수준 도구** | `dpkg` | `rpm` |
| **고수준 도구** | `apt` | `yum` / `dnf` |

---

## apt 주요 명령어 (데비안/우분투)

| 명령어 | 용도 | sudo? |
|--------|------|-------|
| `sudo apt update` | 저장소 목록 최신화 (설치 X, 카탈로그 갱신) | O |
| `sudo apt install 패키지` | 패키지 설치 | O |
| `sudo apt remove 패키지` | 삭제 (설정파일 남김) | O |
| `sudo apt purge 패키지` | 완전 삭제 (설정파일까지) | O |
| `sudo apt upgrade` | 설치된 패키지 전부 업그레이드 | O |
| `apt search 키워드` | 패키지 검색 | X |
| `apt show 패키지` | 패키지 정보 보기 | X |
| `apt list --installed` | 설치된 패키지 목록 | X |

---

## yum/dnf 주요 명령어 (레드햇/CentOS)

| 명령어 | 용도 |
|--------|------|
| `sudo yum install 패키지` | 설치 |
| `sudo yum remove 패키지` | 삭제 |
| `sudo yum update` | 전체 업데이트 (= apt의 upgrade) |
| `yum search 키워드` | 검색 |
| `yum info 패키지` | 정보 보기 |

`dnf`는 `yum`의 후속 버전이야. Fedora, RHEL 8+에서 사용하는데, 명령어 사용법은 동일해.

---

## 헷갈리기 쉬운 차이들

### update vs upgrade (apt)

이거 처음에 다들 헷갈려:
- **`apt update`** = 저장소 목록 갱신 (뭐가 새 버전인지 **확인만** 함. 설치 안 함!)
- **`apt upgrade`** = 실제로 패키지들을 새 버전으로 **업그레이드**

보통 세트로 씀: `sudo apt update && sudo apt upgrade`

### remove vs purge

- **`remove`** = 프로그램 삭제, 설정 파일은 남김 (재설치 시 설정 복원 가능)
- **`purge`** = 프로그램 + 설정 파일 모두 삭제 (완전히 깨끗하게)

### sudo가 필요한 것 vs 불필요한 것

- **시스템을 변경**하는 건 sudo 필요: install, remove, update, upgrade
- **정보를 조회**하는 건 sudo 불필요: search, show, list

---

## 정리

오늘 배운 핵심:
- **패키지** = 설치 가능한 프로그램 묶음, **저장소** = 패키지 앱스토어
- 데비안 계열은 `apt`, 레드햇 계열은 `yum`/`dnf`
- `apt update` = 목록 갱신, `apt upgrade` = 실제 업그레이드 (헷갈리지 말기!)
- `remove` = 설정 남김, `purge` = 완전 삭제
- 시스템 변경은 sudo, 정보 조회는 sudo 불필요
