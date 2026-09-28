# Ch.22: Regular Expressions (정규식)

## 핵심 개념

### 정규식이란?

텍스트 패턴을 표현하는 언어. grep, sed, awk, vim, Python, JavaScript 등 거의 모든 곳에서 사용.

> **와일드카드(glob) ≠ 정규식!**
> - 와일드카드 `*` = 아무거나 다 (find, ls에서 사용)
> - 정규식 `*` = 앞 문자를 0번 이상 반복
> - 정규식에서 "아무거나 다"는 `.*`

### 리터럴 vs 메타문자

- **리터럴**: 글자 그대로 매칭. `grep "hello"` → hello 찾기
- **메타문자**: 특별한 의미. `. ^ $ * + ? [] {} () | \`

---

## 메타문자 정리

### BRE (Basic Regular Expression) — `grep` 기본

| 메타문자 | 의미 | 예시 | 매칭 |
|---------|------|------|------|
| `.` | 아무 문자 1개 | `h.t` | hat, hot, hit |
| `^` | 줄의 시작 | `^Hello` | Hello로 시작하는 줄 |
| `$` | 줄의 끝 | `end$` | end로 끝나는 줄 |
| `^$` | 빈 줄 | `grep -v "^$"` | 빈 줄 제거 |
| `[abc]` | 이 중 하나 | `[aeiou]` | 모음 1개 |
| `[^abc]` | 이것 빼고 | `[^0-9]` | 숫자 아닌 문자 |
| `*` | 앞 문자 0번 이상 | `ab*c` | ac, abc, abbc |
| `.*` | 아무거나 다 | `h.*o` | ho, hello, h123o |
| `\` | 이스케이프 | `\.` | 진짜 마침표 |

### `^`의 이중 의미 — 위치에 따라 다르다!

```
^Hello    → ^ = 줄의 시작 (앵커)
[^abc]    → ^ = 제외 (부정 문자 클래스)
```

실전 예시:
```bash
grep "^[^#]" config.txt    # 줄 시작이 #이 아닌 줄 = 주석 아닌 줄
```

### 범위 표현

```
[0-9]      숫자 0~9
[a-z]      소문자 a~z
[A-Z]      대문자 A~Z
[a-zA-Z]   모든 영문자
[0-9a-f]   16진수 문자
```

---

## ERE (Extended Regular Expression) — `grep -E`

BRE의 모든 메타문자 + 아래 추가:

| 메타문자 | 의미 | 예시 | 매칭 |
|---------|------|------|------|
| `+` | 1번 이상 반복 | `ab+c` | abc, abbc (ac는 ❌) |
| `?` | 0번 또는 1번 | `colou?r` | color, colour (colouur는 ❌) |
| `{n,m}` | n~m번 반복 | `[0-9]{3}` | 숫자 3자리 |
| `{n,}` | n번 이상 | `[0-9]{3,}` | 숫자 3자리 이상 |
| `\|` | OR | `cat\|dog` | cat 또는 dog |
| `()` | 그룹화 | `(ab)+` | ab, abab, ababab |

### BRE vs ERE 비교

| | BRE (`grep`) | ERE (`grep -E`) |
|---|---|---|
| `? + {} () \|` | `\` 이스케이프 필요 | 그냥 사용 |
| 예시 | `grep "ab\{2,4\}"` | `grep -E "ab{2,4}"` |
| 실무 | 거의 안 씀 | **이걸 쓰자!** |

---

## grep 주요 옵션

| 옵션 | 의미 | 예시 |
|------|------|------|
| `-i` | 대소문자 무시 | `grep -i "error"` |
| `-n` | 줄 번호 표시 | `grep -n "error"` |
| `-c` | 매칭 줄 개수 | `grep -c "error"` |
| `-v` | 매칭 안 되는 줄 | `grep -v "^$"` (빈 줄 제거) |
| `-w` | 단어 단위 매칭 | `grep -w "cat"` (category 안 잡힘) |
| `-E` | ERE 사용 | `grep -E "cat\|dog"` |

---

## 실전 패턴 모음

```bash
# 빈 줄 제거
grep -v "^$" file

# 주석(#) 아닌 줄만
grep "^[^#]" config.txt

# ERROR 또는 WARNING으로 시작하는 줄
grep -E "^(ERROR|WARNING)" log.txt

# 숫자 3자리 이상
grep -E "[0-9]{3}" file

# color/colour 둘 다 매칭
grep -E "colou?r" file
```

---

## 주의사항 & 흔한 실수

1. **`|` 양쪽에 공백 넣지 마!** — 정규식에서 공백도 문자다
   - ❌ `grep -E "ERROR | WARNING"` → "ERROR " 또는 " WARNING" 검색
   - ✅ `grep -E "ERROR|WARNING"`

2. **와일드카드 `*`과 정규식 `*` 혼동 주의**
   - `find -name "*.log"` → glob (아무거나)
   - `grep ".*log"` → 정규식 (앞 문자 반복)

3. **`^`의 의미 — 위치가 중요**
   - 패턴 맨 앞: 줄의 시작
   - `[]` 안 첫 글자: 부정(제외)

4. **BRE에서 `{}`는 이스케이프 필요** — `grep -E`를 쓰면 신경 안 써도 됨
