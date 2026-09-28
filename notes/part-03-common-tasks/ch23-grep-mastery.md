# Ch.23: grep 완전 정복

## 핵심 개념

### grep 기본 구조

```
grep [옵션] '패턴' [파일...]
```

- 패턴에 매치되는 **줄 전체**를 출력
- 파일 안 주면 stdin(파이프)에서 읽음

---

## 옵션 총정리 (4가지 카테고리)

### 1. 매칭 방식

| 옵션 | 의미 | 예시 |
|------|------|------|
| `-i` | 대소문자 무시 (ignore case) | `grep -i 'error' log.txt` |
| `-w` | 단어 단위 매칭 (word) | `grep -w 'is' file` → "this" 안 잡힘 |
| `-x` | 줄 전체 일치 | `grep -x 'hello' file` |
| `-v` | 반전 (invert) | `grep -v '#' config` → 주석 아닌 줄 |
| `-F` | 정규식 끄기 (Fixed string) | `grep -F '192.168.1.1' log` |
| `-E` | ERE 사용 (= egrep) | `grep -E 'error|warning' log` |

### 2. 출력 제어

| 옵션 | 의미 | 예시 |
|------|------|------|
| `-n` | 줄 번호 표시 | `grep -n 'TODO' *.py` |
| `-c` | 매치된 줄 수만 | `grep -c 'error' log` → `42` |
| `-l` | 매치된 파일명만 | `grep -l 'TODO' *.py` |
| `-L` | 매치 안 된 파일명만 | `grep -L 'TODO' *.py` |
| `-o` | 매치 부분만 추출 | `grep -oE '[0-9]+' file` |
| `-h` | 파일명 숨기기 | `grep -h 'error' *.log` |

### 3. 컨텍스트 (주변 줄 보기)

| 옵션 | 의미 | 예시 |
|------|------|------|
| `-A N` | 매치 후 N줄 (After) | `grep -A 3 'error' log` |
| `-B N` | 매치 전 N줄 (Before) | `grep -B 2 'error' log` |
| `-C N` | 앞뒤 N줄 (Context) | `grep -C 2 'error' log` |

→ 로그 분석 시 에러 전후 맥락 파악에 필수

### 4. 재귀 검색

| 옵션 | 의미 | 예시 |
|------|------|------|
| `-r` | 디렉토리 재귀 검색 | `grep -r 'TODO' ./src/` |
| `-R` | `-r` + 심볼릭 링크 따라감 | `grep -R 'TODO' ./` |
| `--include` | 특정 파일만 | `grep -r --include='*.py' 'import' .` |
| `--exclude` | 특정 파일 제외 | `grep -r --exclude='*.log' 'error' .` |
| `--exclude-dir` | 특정 디렉토리 제외 | `grep -r --exclude-dir='.git' 'TODO' .` |

---

## 실전 필수 조합 베스트 5

```bash
# 1. 프로젝트 전체 TODO 검색 (줄 번호 포함)
grep -rn 'TODO' .

# 2. 주석과 빈 줄 제외하고 설정만 보기
grep -vE '^#|^$' config.conf

# 3. 파일별 에러 횟수
grep -c 'ERROR' *.log

# 4. 특정 함수 쓰는 파일 목록
grep -rlw 'function_name' ./src/

# 5. 여러 키워드 한번에
grep -E 'error|warning|fatal' log
```

---

## `-w` 단어 매칭

```bash
grep 'is' file      # → "this", "island" 다 잡힘
grep -w 'is' file    # → "is"만 잡힘 (단어 경계 기준)
```

단어 경계 = 공백, 줄 시작/끝, 구두점. 정규식 `\b`와 같은 효과.

## `-o`로 매치 부분만 추출

```bash
# IP 주소만 뽑기
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' access.log

# 숫자 덩어리만 뽑기
grep -oE '[0-9]+' file
```

## `-F`가 필요한 이유

```bash
grep '192.168.1.1' log      # . 이 "아무 문자"로 해석됨! 위험
grep -F '192.168.1.1' log   # 문자 그대로 검색. 안전
```

## grep vs egrep vs fgrep

```
grep     = BRE (기본)
egrep    = grep -E (ERE) — deprecated
fgrep    = grep -F (고정 문자열) — deprecated
```

새 스크립트에서는 `grep -E`, `grep -F` 사용.

---

## 주의사항 & 흔한 실수

- **`-rn`은 세트**: 재귀 검색할 때 `-r`만 쓰면 줄 번호 없어서 불편. 항상 `-rn`
- **파이프로 받을 때 파일명 빼기**: `grep 'A' file | grep -oE 'B' file` ← 두 번째 grep이 파이프 무시하고 파일 읽음!
- **`[0-9]` vs `[0-9]+`**: `+` 없으면 한 자리씩 나옴. 숫자 덩어리는 `+` 필수
- **`^$` = 빈 줄**: 빈 줄 제외할 때 `grep -v '^$'`
