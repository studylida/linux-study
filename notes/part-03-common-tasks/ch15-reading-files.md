# Ch.15: Reading Files

## 오늘 배울 것

오늘은 **파일 내용을 읽는 다양한 방법**을 배울 거야. 전체를 볼 수도 있고, 앞부분만, 뒷부분만, 페이지별로, 정렬해서, 통계까지 — 상황에 맞는 도구를 고르면 돼!

---

## 파일 전체 출력

### cat (concatenate)

가장 기본적인 파일 출력 명령어야:

```bash
cat filename          # 파일 내용 출력
cat -n filename       # 줄 번호 붙여서 출력
cat -A filename       # 숨겨진 문자(탭, 줄끝) 표시
cat file1 file2       # 여러 파일 이어서 출력
```

이름이 concatenate(이어붙이기)인 이유가 있어 — 원래는 여러 파일을 합치는 용도였거든.

### tac (cat 거꾸로)

```bash
tac filename          # 마지막 줄부터 첫 줄 순서로 출력
```

이름이 cat을 거꾸로 쓴 거야 ㅋㅋ **줄 순서**를 뒤집어.

### rev (줄 안의 글자 반전)

```bash
rev filename          # 각 줄의 문자 순서를 뒤집음
echo "hello" | rev    # olleh
```

tac은 **줄 순서** 뒤집기, rev는 **글자 순서** 뒤집기. 헷갈리지 마!

---

## 부분만 보기

### head — 앞부분

```bash
head filename         # 처음 10줄 (기본값)
head -n 5 filename    # 처음 5줄
head -n -3 filename   # 마지막 3줄을 제외한 전부
```

### tail — 뒷부분

```bash
tail filename         # 마지막 10줄 (기본값)
tail -n 5 filename    # 마지막 5줄
tail -n +3 filename   # 3번째 줄부터 끝까지
tail -f logfile       # 실시간 모니터링! (follow)
```

`tail -f`는 서버 로그를 실시간으로 볼 때 필수 명령어야! 파일에 새 줄이 추가되면 자동으로 화면에 나와.

---

## 페이지 단위로 읽기 — less

파일이 길 때 가장 편한 방법:

```bash
less filename
```

| 키 | 동작 |
|----|------|
| Space / f | 다음 페이지 |
| b | 이전 페이지 |
| /검색어 | 앞으로 검색 |
| ?검색어 | 뒤로 검색 |
| n / N | 다음/이전 검색 결과 |
| g / G | 파일 처음/끝 |
| q | 종료 |

---

## 파일 통계 — wc (Word Count)

```bash
wc filename           # 줄 수  단어 수  바이트 수  파일명
wc -l filename        # 줄 수만
wc -w filename        # 단어 수만
wc -c filename        # 바이트 수만
wc -m filename        # 문자 수 (멀티바이트 문자 지원)
```

---

## 정렬 — sort

```bash
sort filename         # 알파벳 순 정렬
sort -r filename      # 역순 정렬
sort -n filename      # 숫자 기준 정렬 (이거 안 쓰면 1, 10, 2 순서가 됨!)
sort -k 2 filename    # 2번째 필드 기준 정렬
sort -t ',' -k 3 f.csv  # 콤마로 구분된 파일에서 3번째 필드 기준
sort -u filename      # 정렬 + 중복 제거 한 번에
```

---

## 팁: Useless Use of Cat

이런 실수를 많이 해:

```bash
# 나쁜 예 (불필요한 프로세스가 생김)
cat file | head -3

# 좋은 예 (직접 파일명을 전달)
head -3 file
```

대부분의 명령어는 파일명을 직접 받을 수 있으니까, 굳이 `cat`으로 먼저 읽을 필요 없어.

---

## 정리

오늘 배운 핵심:
- `cat` = 전체 출력 / 파일 합치기, `tac` = 줄 역순, `rev` = 글자 역순
- `head` = 앞부분, `tail` = 뒷부분, **`tail -f` = 실시간 모니터링**
- `less` = 페이지 단위로 읽기 (`q`로 나가기)
- `wc -l` = 줄 수 세기
- `sort -n` = 숫자 기준 정렬 (중요!)
- `cat file | head` 말고 `head file`로 쓰기
