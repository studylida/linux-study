# Ch.27: Compiling Programs

## 핵심 개념

### 컴파일이란?

소스코드(사람용)를 실행파일(기계용)로 번역하는 과정.

### 컴파일 4단계

```
소스코드(.c)
    │ ① 전처리 (Preprocessing) — #include, #define 처리
    ▼
전처리된 소스(.i)
    │ ② 컴파일 (Compilation) — C → 어셈블리어
    ▼
어셈블리 코드(.s)
    │ ③ 어셈블 (Assembly) — 어셈블리어 → 기계어
    ▼
오브젝트 파일(.o)
    │ ④ 링킹 (Linking) — 라이브러리 연결, 실행파일 생성
    ▼
실행파일
```

보통 `gcc`가 4단계를 한 번에 처리해줌.

---

## gcc 명령어 정리

### 기본 사용법

```bash
gcc hello.c -o hello    # 소스 → 실행파일 (이름 지정)
gcc hello.c             # 이름 생략하면 a.out
./hello                 # 실행 (./ 필요 — 현재 디렉토리는 PATH에 없으므로)
```

### 주요 옵션

| 옵션 | 의미 | 비고 |
|------|------|------|
| `-o 파일명` | 출력 파일 이름 지정 | |
| `-Wall` | 모든 경고 표시 | **항상 쓰는 게 좋음** |
| `-g` | 디버그 정보 포함 | gdb 디버깅용 |
| `-c` | 컴파일만 (링킹 안 함) | `.o` 파일 생성 |
| `-O2` | 최적화 레벨 2 | 실행 속도 향상 |

### 여러 파일 컴파일

```bash
# 방법 1: 한 번에
gcc main.c utils.c -o myapp

# 방법 2: 따로 컴파일 후 링킹 (큰 프로젝트용)
gcc -c main.c          # → main.o
gcc -c utils.c         # → utils.o
gcc main.o utils.o -o myapp
```

방법 2의 장점: 수정된 파일만 다시 컴파일 → **이걸 자동화하는 게 make**

---

## .h와 .c의 관계

```
.h = 메뉴판 (선언만)     .c = 주방 (실제 구현)
```

```
┌─ math_utils.h ─────────┐
│ int add(int, int);      │  ← 선언만
│ int multiply(int, int); │
└─────────────────────────┘
        ▲           ▲
        │include     │include
┌───────┴────┐  ┌────┴─────┐
│math_utils.c│  │  main.c  │
│ 구현(주방)  │  │ 사용(손님)│
└────────────┘  └──────────┘
```

- `.c`가 `.h`를 include함 (반대 아님!)
- `.h`에 `.c`를 include하면 → 함수 중복 정의 에러

---

## make와 Makefile

### make의 핵심 아이디어

바뀐 파일만 추적해서 **변경된 것만 다시 컴파일**.

### Makefile 기본 구조

```makefile
타겟: 의존성
	명령어          ← 반드시 Tab! 스페이스 쓰면 에러!
```

### 예시 Makefile

```makefile
CC = gcc
CFLAGS = -Wall -g

calculator: main.o math_utils.o
	$(CC) $(CFLAGS) main.o math_utils.o -o calculator

main.o: main.c math_utils.h
	$(CC) $(CFLAGS) -c main.c

math_utils.o: math_utils.c math_utils.h
	$(CC) $(CFLAGS) -c math_utils.c

clean:
	rm -f *.o calculator
```

### Makefile 관례 변수

- `CC`: 컴파일러
- `CFLAGS`: 컴파일 옵션
- `$(변수명)`으로 참조

### make 실행

```bash
make          # 첫 번째 타겟 빌드
make clean    # clean 타겟 실행
make utils.o  # 특정 타겟만
```

---

## 오픈소스 소스 설치 3단계

```bash
./configure          # ① 환경 체크 → Makefile 자동 생성
make                 # ② Makefile대로 컴파일
sudo make install    # ③ 완성품을 시스템 경로에 설치
```

| 단계 | 하는 일 | 비유 |
|------|---------|------|
| `./configure` | 컴파일러/라이브러리 있는지 체크, Makefile 생성 | 재료 확인 |
| `make` | 컴파일 | 요리 |
| `make install` | 바이너리를 `/usr/local/bin` 등에 복사 | 접시에 담기 |

### configure 에러 대처

```
configure: error: OpenSSL not found
→ apt install libssl-dev 후 다시 ./configure
```

### configure 옵션

```bash
./configure --prefix=/opt/myapp       # 설치 경로 변경
./configure --without-ssl             # 기능 끄기
```

---

## 주의사항 & 흔한 실수

- **Makefile 들여쓰기는 반드시 Tab**: 스페이스 → `missing separator` 에러
- **`gcc -c`는 실행파일 안 만듦**: `.o` (오브젝트 파일)만 생성
- **`./` 붙여야 실행**: 현재 디렉토리는 PATH에 없으므로 `./hello`
- **파일 만들 때 `cat > file`**: `cat file`은 읽기, `cat > file`은 쓰기!
