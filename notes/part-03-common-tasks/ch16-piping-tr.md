# Ch.16: Piping과 tr

## 오늘 배울 것

오늘은 파이프를 **실전에서 어떻게 조합하는지**, 그리고 문자를 변환하는 **tr**, 파이프 흐름을 나누는 **tee**, 중복을 제거하는 **uniq**를 배울 거야. 유닉스 철학인 "작은 도구를 파이프로 연결한다"를 직접 체험해볼 거야!

---

## tr — 문자 변환/삭제 도구

`tr`은 표준 입력을 받아서 **문자 단위로** 변환하거나 삭제해. 파일을 직접 읽지 못하니까 반드시 파이프(`|`)나 리다이렉션(`<`)으로 입력을 넘겨줘야 해.

### 기본 사용법 (변환)

```bash
echo "hello" | tr 'a-z' 'A-Z'       # → HELLO (소문자→대문자)
echo "hello world" | tr ' ' '_'      # → hello_world (공백→밑줄)
echo "abc" | tr 'abc' 'xyz'          # → xyz (문자 1:1 매핑)
```

### tr -d (삭제)

`-d`를 붙이면 지정한 문자를 **삭제**해:

```bash
echo "hello123world456" | tr -d '0-9'   # → helloworld (숫자 삭제)
echo "h e l l o" | tr -d ' '            # → hello (공백 삭제)
```

주의: `-d` 없이 쓰면 삭제가 아니라 변환이야!

### tr -s (squeeze, 압축)

연속된 중복 문자를 **하나로** 압축해:

```bash
echo "hello     world" | tr -s ' '    # → hello world (공백 여러 개를 하나로)
echo "aaabbbccc" | tr -s 'abc'         # → abc
```

### 문자 클래스

범위 대신 표준화된 클래스를 쓸 수도 있어:

| 클래스 | 의미 |
|--------|------|
| `[:lower:]` | 소문자 |
| `[:upper:]` | 대문자 |
| `[:digit:]` | 숫자 |
| `[:space:]` | 공백 문자 |
| `[:punct:]` | 구두점 |

```bash
echo "Hello World 123" | tr '[:lower:]' '[:upper:]'   # → HELLO WORLD 123
echo "Hello World 123" | tr -d '[:digit:]'             # → Hello World
```

---

## tee — T자 파이프 (이중 출력)

파이프 흐름을 유지하면서 **동시에 파일에도 저장**하고 싶을 때 써. 배관의 T자 연결부처럼 흐름을 두 갈래로 나눠:

```
         ┌──→ 파일 (file)
입력 → tee
         └──→ stdout (다음 파이프 또는 화면)
```

```bash
ls -l | tee output.txt                      # 화면에도 나오고 파일에도 저장
cat data.txt | sort | tee sorted.txt | head -5   # 파이프 중간에서 저장
```

### tee -a (append 모드)

기본적으로 tee는 **덮어쓰기**(`>`처럼)야. `-a`를 쓰면 **추가**(`>>`처럼):

```bash
echo "새 로그" | tee -a log.txt    # 기존 내용 유지하면서 추가
```

### tee vs `>` 차이

- `>` — 출력이 파일로만 가고 화면에는 안 나옴
- `tee` — 파일에도 가고 화면(stdout)에도 감 → 파이프 체인 중간에 끼울 수 있어

---

## uniq — 중복 제거

**중요**: uniq는 **인접한** 중복만 제거해. 그래서 **반드시 sort 후에** 써야 해!

```bash
sort list.txt | uniq        # 중복 제거
sort list.txt | uniq -c     # 중복 횟수 표시
sort list.txt | uniq -d     # 중복된 항목만 출력
sort list.txt | uniq -u     # 유일한 항목만 출력 (1번만 나온 것)
```

### 옵션 정리

| 옵션 | 의미 | 뭘 보여줘? |
|------|------|-----------|
| (없음) | 중복 제거 | 모든 항목 (중복은 1개로) |
| `-c` | count | 모든 항목 + 횟수 |
| `-d` | duplicated | 2번 이상 나온 것만 |
| `-u` | unique | 정확히 1번만 나온 것만 |

### sort -u vs sort | uniq

- `sort -u` — 단순 중복 제거만 필요할 때 한 번에
- `sort | uniq -c` — **빈도 분석**이 필요하면 이걸 써야 해 (sort -u로는 횟수를 못 셈)

---

## 파이프 조합 실전

### 기본 패턴들

```bash
# 에러 줄 수 세기
grep "ERROR" app.log | wc -l

# 파일명을 대문자로 출력
ls | tr 'a-z' 'A-Z'

# CSV에서 특정 열의 빈도 분석
cut -d',' -f2 data.csv | sort | uniq -c | sort -rn
```

### 단어 빈도 분석 — 파이프의 진수!

이 파이프라인이 각 단계에서 뭘 하는지 하나씩 보자:

```bash
cat book.txt | tr 'A-Z' 'a-z' | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -10
```

| 단계 | 명령어 | 역할 |
|------|--------|------|
| 1 | `cat book.txt` | 파일 읽기 |
| 2 | `tr 'A-Z' 'a-z'` | 대문자→소문자 통일 |
| 3 | `tr -s ' ' '\n'` | 공백→줄바꿈 (단어별 한 줄) |
| 4 | `sort` | 알파벳 정렬 (uniq 전 필수!) |
| 5 | `uniq -c` | 각 단어 등장 횟수 |
| 6 | `sort -rn` | 횟수 기준 내림차순 정렬 |
| 7 | `head -10` | 상위 10개만 |

### 가장 많이 쓴 명령어 top 5

```bash
history | tr -s ' ' | cut -d' ' -f3 | sort | uniq -c | sort -rn | head -5
```

---

## 유닉스 철학

> "한 가지 일을 잘하는 작은 프로그램들을, 파이프로 조합한다."

tr, sort, uniq, head... 각각은 단순하지만, 파이프로 연결하면 복잡한 데이터 분석도 가능해. 이게 리눅스의 힘이야!

---

## 정리

오늘 배운 핵심:
- **tr** = 문자 변환(`tr 'a-z' 'A-Z'`), 삭제(`-d`), 압축(`-s`)
- **tee** = 파이프 흐름을 나눠서 파일 + 화면 동시 출력
- **uniq** = 중복 제거 (반드시 `sort` 먼저!), `-c`로 빈도 분석
- 파이프 조합의 핵심은 **각 단계가 뭘 하는지** 이해하는 것
- 유닉스 철학: 작은 도구 + 파이프 = 강력한 조합
