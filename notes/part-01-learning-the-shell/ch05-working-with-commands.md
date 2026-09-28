# Ch.5: Working with Commands

## 오늘 배울 것

지금까지 `ls`, `cd`, `cp` 같은 명령어를 잔뜩 배웠지? 오늘은 **"명령어 자체"에 대해** 배울 거야. 명령어가 뭔지, 어떤 종류가 있는지, 그리고 모르는 명령어를 어떻게 찾아보는지!

---

## 명령어의 4가지 종류

리눅스 명령어는 사실 4가지 종류가 있어:

| 종류 | 설명 | 예시 |
|------|------|------|
| 실행 파일 | 디스크에 있는 컴파일된 프로그램 | `/usr/bin/ls` |
| 셸 내장 명령어 | 셸 안에 내장되어 있음 | `cd`, `exit` |
| 셸 함수 | 스크립트로 정의된 명령어 | 사용자 정의 |
| 별칭 (alias) | 다른 명령어에 붙인 별명 | `ll` → `ls -l` |

### 실행 파일 vs 내장 명령어

| | 실행 파일 | 내장 명령어 |
|---|----------|-------------|
| 위치 | `/bin`, `/usr/bin` 등에 파일로 존재 | 셸 프로그램 안에 포함 |
| 확인 방법 | `which`, `man` | `help` |
| 속도 | 상대적으로 느림 | 빠름 |
| 예시 | `cp`, `ls`, `grep` | `cd`, `exit`, `alias` |

### 왜 내장 명령어가 필요할까?

`cd`가 대표적인 예야. `cd`는 **현재 셸의 디렉토리를 바꿔야** 하잖아. 만약 `cd`가 외부 실행 파일이면, 새로운 프로세스가 생기고 그 프로세스의 디렉토리만 바뀌고 끝나버려. 원래 셸은 그대로야. 그래서 `cd`는 셸 안에 내장되어 있어야만 해.

---

## type — "이 명령어가 뭐야?"

가장 먼저 쓸 수 있는 도구야. 명령어의 종류를 알려줘:

```bash
type cd        # cd is a shell builtin
type cp        # cp is /bin/cp
type ll        # ll is an alias for ls -l
```

| 옵션 | 의미 |
|------|------|
| `-a` | 같은 이름의 모든 명령어를 보여줌 |
| `-t` | 종류만 짧게 출력 (alias, builtin, file 등) |

---

## which — "실행 파일이 어디 있어?"

```bash
which cp       # /bin/cp
which cd       # (출력 없음 — 내장 명령어라서 파일이 없어!)
```

`-a` 옵션을 쓰면 같은 이름의 모든 실행 파일을 보여줘. 셸 내장 명령어는 파일이 아니니까 `which`로 못 찾아.

---

## 도움말 보는 방법

모르는 명령어가 있을 때 도움을 받는 방법이 여러 가지야:

### help — 셸 내장 명령어용

```bash
help cd
help exit
```

셸 내장 명령어 전용이야. `help cp` 이런 건 안 돼.

### man — 매뉴얼 페이지

```bash
man cp         # cp의 매뉴얼 (q로 나가기)
man 5 passwd   # 섹션 5의 passwd 문서
```

`man`은 `less`와 같은 화면으로 열리니까 `q`로 나가면 돼.

매뉴얼은 **섹션**으로 나뉘어 있어:

| 번호 | 내용 |
|------|------|
| 1 | 사용자 명령어 |
| 2 | 시스템 콜 |
| 3 | 라이브러리 함수 |
| 4 | 특수 파일 |
| 5 | 파일 형식 |
| 6 | 게임 |
| 7 | 기타 |
| 8 | 시스템 관리 명령어 |

### whatis — 한 줄 설명

이름은 아는데 뭐하는 건지 빠르게 확인하고 싶을 때:

```bash
whatis ls      # ls (1) - list directory contents
whatis cp      # cp (1) - copy files and directories
```

### apropos — "이런 기능 하는 명령어가 뭐야?"

명령어 이름이 기억 안 날 때 키워드로 찾을 수 있어:

```bash
apropos copy     # 복사 관련 명령어 검색
# cp (1)       - copy files and directories
# dd (1)       - convert and copy a file
# install (1)  - copy files and set attributes

apropos delete   # 삭제 관련 명령어 검색
apropos network  # 네트워크 관련 명령어 검색
```

`man -k copy`도 `apropos copy`와 완전히 같은 거야. `-k`는 keyword의 약자.

---

## alias — 별칭 만들기

자주 쓰는 긴 명령어에 짧은 별명을 붙일 수 있어:

```bash
alias                        # 현재 별칭 전체 보기
alias ll='ls -l'             # ll이라고 치면 ls -l이 실행됨
alias la='ls -la'
alias ..='cd ..'
alias ...='cd ../..'
unalias ll                   # 별칭 삭제
```

**주의**: 터미널을 끄면 사라져! 영구적으로 저장하려면 Ch.11(환경 설정)에서 배울 `.bashrc`에 넣어야 해.

---

## 정리

오늘 배운 핵심:
- 명령어는 **실행 파일, 내장 명령어, 셸 함수, 별칭** 4가지 종류
- `type`으로 명령어 종류 확인, `which`로 실행 파일 경로 찾기
- 도움말: `help`(내장), `man`(매뉴얼), `whatis`(한 줄), `apropos`(키워드 검색)
- `alias`로 자주 쓰는 명령어에 별명 붙이기
