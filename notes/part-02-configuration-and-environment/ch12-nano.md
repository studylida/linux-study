# Ch.12: Nano 에디터

## 오늘 배울 것

오늘은 터미널에서 파일을 편집하는 방법을 배울 거야. **nano**는 터미널 기반 텍스트 에디터 중에서 가장 쉬운 거야. 서버에 접속해서 설정 파일을 수정해야 할 때 반드시 필요해!

---

## 기본 사용법

```bash
nano filename         # 파일 열기 (없으면 새로 만들기)
nano +10 filename     # 10번째 줄에서 열기
nano -l filename      # 줄 번호 표시하며 열기
sudo nano /etc/hosts  # 관리자 권한으로 시스템 파일 수정
```

nano를 열면 아래쪽에 `^G Help`, `^O Write Out` 같은 게 보이는데, `^`는 **Ctrl** 키를 뜻해.

---

## 핵심 단축키

### 저장 & 종료 — 이것만 알면 살 수 있어

- `Ctrl+O` → Enter — **저장** (Write Out)
- `Ctrl+X` — **나가기** (Exit)
- `Ctrl+X` → `Y` → Enter — **저장하고 나가기** (가장 많이 쓰는 조합)

### 편집

- `Ctrl+K` — 현재 줄 잘라내기 (Kill)
- `Ctrl+U` — 붙여넣기 (Uncut)
- `Ctrl+6` — 선택 시작 → 커서 이동 → `Ctrl+K`로 잘라내기
- `Alt+6` — 선택 영역 복사

### 이동

- `Ctrl+A` / `Ctrl+E` — 줄 맨 앞 / 맨 끝
- `Ctrl+Y` / `Ctrl+V` — 페이지 위 / 아래
- `Ctrl+_` — 특정 줄 번호로 이동

### 찾기 & 바꾸기

- `Ctrl+W` — 찾기 (Where is)
- `Alt+W` — 다음 검색 결과
- `Ctrl+\` — 찾아 바꾸기 (Y = 하나씩, A = 전부 다)

### 기타

- `Ctrl+G` — 도움말
- `Ctrl+C` — 현재 커서 위치 확인 (줄/열)

---

## bash Kill/Yank과 헷갈리지 말기!

nano의 단축키가 bash와 좀 다른 부분이 있어:

| 동작 | nano | bash |
|------|------|------|
| 잘라내기 | `Ctrl+K` | `Ctrl+K` |
| 붙여넣기 | **`Ctrl+U`** | **`Ctrl+Y`** |

붙여넣기 키가 다르니까 주의! nano에서는 `Ctrl+U`, bash에서는 `Ctrl+Y`야.

---

## 정리

오늘 배운 핵심:
- `nano filename`으로 파일 열기
- **저장하고 나가기**: `Ctrl+X` → `Y` → Enter (이거 하나만 기억해!)
- `Ctrl+K` = 잘라내기, `Ctrl+U` = 붙여넣기
- `Ctrl+W` = 찾기, `Ctrl+\` = 찾아 바꾸기
- bash와 붙여넣기 키가 다르다는 것만 주의!
