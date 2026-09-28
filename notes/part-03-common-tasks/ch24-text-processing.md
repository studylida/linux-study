# Ch.24 Text Processing — 텍스트 가공 도구 총정리

텍스트 처리 6대 도구를 3개 파트로 학습한다.
- **파트 A**: cut, paste, join (자르기 & 합치기)
- **파트 B**: comm, diff (비교하기)
- **파트 C**: sed, awk (변환 & 가공)

---

## 파트 A: cut, paste, join (자르기 & 합치기)

### cut — 열(column) 잘라내기

파일에서 특정 열만 뽑아내는 도구. 엑셀에서 열 선택하는 것과 같다.

| 옵션 | 의미 | 예시 |
|------|------|------|
| `-d` | delimiter (구분자) | `-d:` 콜론 기준, `-d,` 쉼표 기준 |
| `-f` | field (필드 번호) | `-f1` 첫째, `-f1,3` 첫째+셋째, `-f2-4` 2~4번째 |
| `-c` | character (문자 위치) | `-c1-5` 1~5번째 글자 |

#### 실습: CSV에서 원하는 열 추출

```bash
# 테스트 파일 만들기
echo -e "id,name,dept,salary\n1,Alice,Engineering,90000\n2,Bob,Marketing,75000\n3,Charlie,Engineering,85000" > /tmp/employees.csv

# 이름과 부서만 추출
cut -d, -f2,3 /tmp/employees.csv
# → name,dept / Alice,Engineering / Bob,Marketing / Charlie,Engineering

# 헤더 제외하고 이름만
tail -n +2 /tmp/employees.csv | cut -d, -f2
# → Alice / Bob / Charlie
```

#### 주의사항
- 기본 구분자는 **탭(Tab)**
- 구분자는 **한 글자만** 가능 (`-d"::"` 안 됨)
- 공백 여러 개면 cut으로는 힘듦 → `awk` 사용

---

### paste — 파일을 옆으로 합치기

두 파일을 나란히 붙이는 도구. cat이 세로로 합친다면, paste는 가로로 합친다.

```bash
# 테스트 파일 만들기
echo -e "Alice\nBob\nCharlie" > /tmp/names.txt
echo -e "90\n75\n95" > /tmp/scores.txt

# 옆으로 합치기 (탭 구분)
paste /tmp/names.txt /tmp/scores.txt
# → Alice   90 / Bob     75 / Charlie  95

# 구분자 지정
paste -d, /tmp/names.txt /tmp/scores.txt
# → Alice,90 / Bob,75 / Charlie,95

# 한 파일의 줄을 한 줄로 합침
paste -s -d, /tmp/scores.txt
# → 90,75,95
```

`paste - - -` 형태로 stdin을 N개씩 묶을 수도 있다.

---

### join — 공통 필드로 합치기

SQL의 JOIN과 같은 개념. 두 파일에서 공통 키를 기준으로 합친다.

```bash
# 테스트 파일 (첫 번째 필드가 키)
echo -e "1 Alice\n2 Bob\n3 Charlie" > /tmp/ids.txt
echo -e "1 Engineering\n2 Marketing\n3 Engineering" > /tmp/depts.txt

# join으로 합치기
join /tmp/ids.txt /tmp/depts.txt
# → 1 Alice Engineering / 2 Bob Marketing / 3 Charlie Engineering
```

#### 주의사항
- 두 파일 모두 **정렬 필수** (sort 선행)
- 기본: 첫 번째 필드를 키로 사용
- `-1 N -2 M`으로 키 필드 지정 가능

---

## 파트 B: comm, diff (비교하기)

### comm — 두 파일의 집합 비교

정렬된 두 파일을 비교해서 **3개 열**로 보여준다.

```
열1: 파일1에만 있는 줄     (들여쓰기 없음)
  열2: 파일2에만 있는 줄   (탭 1개)
    열3: 양쪽 다 있는 줄   (탭 2개)
```

#### 실습: 두 팀의 멤버 비교

```bash
echo -e "alice\nbanana\ncharlie\ndate" > /tmp/fruits1.txt
echo -e "banana\ncherry\nfig\ngrape" > /tmp/fruits2.txt

# 전체 비교
comm /tmp/fruits1.txt /tmp/fruits2.txt
# apple          ← 파일1에만
#         banana ← 양쪽 다 (탭 2개)
#         cherry ← 양쪽 다
# date           ← 파일1에만
#     fig        ← 파일2에만 (탭 1개)
#     grape      ← 파일2에만
```

#### 열 숨기기 옵션 — 숫자가 "숨기는 열"을 의미

| 옵션 | 의미 | 용도 |
|------|------|------|
| `-12` | 열1,2 숨기기 | **공통 항목만** 보기 |
| `-23` | 열2,3 숨기기 | **파일1에만** 있는 것 |
| `-13` | 열1,3 숨기기 | **파일2에만** 있는 것 |

```bash
# 공통 항목만 보기
comm -12 /tmp/fruits1.txt /tmp/fruits2.txt

# 파일1에만 있는 것
comm -23 /tmp/fruits1.txt /tmp/fruits2.txt
```

> **comm은 정렬된 파일만 비교 가능!** 정렬 안 되어 있으면 `sort` 먼저.

---

### diff — 줄 단위 차이 비교

두 파일 간 **뭐가 바뀌었는지** 보여주는 도구. git diff가 바로 이 diff 기반이다.

#### 실습: 설정 파일 변경 비교

```bash
echo -e "host=localhost\nport=3306\nuser=admin\npass=1234" > /tmp/old.conf
echo -e "host=192.168.1.10\nport=3306\nuser=admin\npass=secure123\ntimeout=30" > /tmp/new.conf

# unified 형식 (git diff 스타일) — 가장 많이 쓰는 형식
diff -u /tmp/old.conf /tmp/new.conf
```

출력:
```
--- /tmp/old.conf
+++ /tmp/new.conf
@@ -1,4 +1,5 @@
-host=localhost
+host=192.168.1.10
 port=3306
 user=admin
-pass=1234
+pass=secure123
+timeout=30
```

#### unified 형식 읽는 법

- `---` = 원본, `+++` = 수정본
- `-` = 삭제된 줄, `+` = 추가된 줄, 공백 = 변경 없음
- `@@ -1,4 +1,5 @@` = 원본 1번째부터 4줄 구간, 수정본 1번째부터 5줄 구간 (위치 표시)

#### 출력 형식 3가지

| 형식 | 옵션 | 특징 |
|------|------|------|
| 기본(normal) | (없음) | `2c2`, `<`, `>` 형식 |
| unified | `-u` | `+`, `-` 형식. **git diff가 이것!** |
| side-by-side | `-y` | 나란히 비교 |

#### 유용한 옵션

| 옵션 | 의미 |
|------|------|
| `-u` | unified 형식 (가장 많이 씀) |
| `-y` | side-by-side 나란히 비교 |
| `-r` | 디렉토리 재귀 비교 |
| `-q` | 다른지만 알려줌 (내용은 안 보여줌) |
| `-rq` | 두 디렉토리에서 **다른 파일 목록만** 빠르게 확인 |
| `-i` | 대소문자 무시 |
| `-w` | 공백 차이 무시 |

---

## 파트 C: sed, awk (변환 & 가공)

### sed — Stream Editor (찾아 바꾸기 끝판왕)

파일을 한 줄씩 읽으면서 변환해서 출력하는 도구. vim의 `:%s` 문법과 거의 같다.

#### 가장 많이 쓰는 형태: 찾아 바꾸기

```bash
echo -e "Hello World\nHello Linux\nHello Hello Shell" > /tmp/test.txt

# 기본: 각 줄의 첫 번째 매치만 바꿈
sed 's/Hello/Hi/' /tmp/test.txt
# → Hi World / Hi Linux / Hi Hello Shell (두 번째 Hello 안 바뀜!)

# g 플래그: 줄의 모든 매치 바꾸기
sed 's/Hello/Hi/g' /tmp/test.txt
# → Hi World / Hi Linux / Hi Hi Shell (둘 다 바뀜!)
```

> **핵심**: `g` 없으면 줄당 첫 번째만, `g` 있으면 줄의 전부!

#### sed 주요 명령 정리

| 사용법 | 의미 |
|--------|------|
| `s/A/B/` | 각 줄의 **첫 매치만** 바꾸기 |
| `s/A/B/g` | 줄의 **전부** 바꾸기 |
| `s/A/B/gi` | 대소문자 무시하고 전부 바꾸기 |
| `-i` | **파일 직접 수정** (되돌릴 수 없음!) |
| `Nd` | N번째 줄 삭제 |
| `/패턴/d` | 패턴 매치 줄 삭제 |
| `-n` + `p` | 매치된 줄만 출력 |

#### 실습: 주석과 빈 줄 제거

```bash
echo -e "# comment\nhost=localhost\n\n# another\nport=3306" > /tmp/config.txt

# 세미콜론(;)으로 여러 명령 연결
sed '/^#/d; /^$/d' /tmp/config.txt
# → host=localhost / port=3306
```

#### -i 사용 시 안전 습관

```bash
# 1단계: 먼저 화면에서 확인 (원본 안 건드림)
sed 's/old/new/g' file.txt

# 2단계: 괜찮으면 그때 -i 추가 (원본 직접 수정)
sed -i 's/old/new/g' file.txt
```

#### 구분자 변경 — 경로에 `/`가 있을 때

```bash
# /가 겹쳐서 에러!
sed 's//usr/local//opt/local/g'    # ❌

# 구분자를 | 또는 #으로 바꾸면 해결
sed 's|/usr/local|/opt/local|g'    # ✅
sed 's#/usr/local#/opt/local#g'    # ✅
```

---

### awk — 필드 단위 처리 (간이 프로그래밍 언어)

각 줄을 공백/탭 기준으로 자동 분리하여 필드별로 처리하는 도구. cut보다 훨씬 강력하다.

#### 기본 구조

```bash
awk '{동작}' 파일
awk '조건 {동작}' 파일
awk -F구분자 '{동작}' 파일
```

#### 내장 변수

| 변수 | 의미 |
|------|------|
| `$0` | 줄 전체 |
| `$1, $2...` | N번째 필드 |
| `$NF` | 마지막 필드 (필드 수 몰라도 OK) |
| `NR` | 현재 줄 번호 |
| `NF` | 현재 줄의 필드 수 |

#### 실습: 기본 필드 추출

```bash
echo -e "Alice 90 A\nBob 75 B\nCharlie 95 A+" > /tmp/scores.txt

# 이름만 출력
awk '{print $1}' /tmp/scores.txt
# → Alice / Bob / Charlie

# 이름과 점수
awk '{print $1, $2}' /tmp/scores.txt
# → Alice 90 / Bob 75 / Charlie 95
```

#### cut vs awk 핵심 차이 — 연속 공백 처리

```bash
echo "Alice    90    A" > /tmp/test2.txt

cut -d' ' -f2 /tmp/test2.txt     # ❌ 빈 문자열 (공백 1개씩만 구분)
awk '{print $2}' /tmp/test2.txt   # ✅ 90 (연속 공백 자동 처리!)
```

> **이게 awk를 쓰는 가장 큰 이유!** `ps aux`, `ls -l` 같은 출력은 공백이 불규칙한데, cut으론 힘들고 awk는 깔끔하게 처리한다.

#### 실습: 구분자 지정과 조건부 출력

```bash
# /etc/passwd에서 유저명과 쉘만 (콜론 구분)
awk -F: '{print $1, $7}' /etc/passwd
# → root /bin/bash / daemon /usr/sbin/nologin / ...

# 점수 80 이상인 사람만
awk '$2 >= 80 {print $1, $2}' /tmp/scores.txt
# → Alice 90 / Charlie 95

# 합계 계산 (END = 모든 줄 처리 끝난 후 실행)
awk '{sum += $2} END {print "Total:", sum}' /tmp/scores.txt
# → Total: 260
```

#### 실전 자주 쓰는 패턴

```bash
# ps에서 프로세스 이름만 추출
ps aux | awk '{print $11}'

# 디스크 사용량에서 장치와 사용률만
df -h | awk '{print $1, $5}'
```

---

## 주의사항 & 흔한 실수

### 1. awk는 반드시 작은따옴표!

```bash
awk '{print $1}'    # ✅ $1이 awk에 전달
awk "{print $1}"    # ❌ 셸이 $1을 먼저 해석 → 줄 전체($0)가 출력됨
```

### 2. sed -i 는 되돌릴 수 없다

먼저 `-i` 없이 화면에서 확인하고, 괜찮으면 그때 `-i` 추가하는 습관!

### 3. g 플래그 까먹으면 첫 매치만 바뀜

```bash
sed 's/A/B/'    # 줄당 첫 번째만 바꿈
sed 's/A/B/g'   # 줄의 전부 바꿈 ← 대부분 이걸 원함
```

### 4. awk 구분자 확인

`/etc/passwd`는 콜론(`:`)인데 쉼표(`,`)로 쓰면 줄 전체가 나온다. 파일의 구분자를 먼저 확인!

---

## 도구 선택 가이드

| 상황 | 도구 |
|------|------|
| 특정 열 추출 (구분자 명확, 1글자) | `cut` |
| 특정 열 추출 (공백 불규칙) | `awk` |
| 파일 옆으로 합치기 | `paste` |
| 공통 키로 합치기 | `join` |
| 두 파일 집합 비교 (공통/차이) | `comm` |
| 두 파일 차이 비교 | `diff -u` |
| 찾아 바꾸기 | `sed` |
| 필드별 가공/계산 | `awk` |
