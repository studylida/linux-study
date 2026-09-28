# Ch.20: Searching for Files

## 핵심 개념

### locate — DB 기반 빠른 검색
- 데이터베이스를 검색해서 파일을 찾음 (디스크 직접 탐색 X)
- `locate 키워드` — 경로/파일명에 키워드 포함된 파일 출력
- `locate -i` — 대소문자 무시
- DB 갱신: `sudo updatedb` (보통 하루 1번 자동)
- 장점: 매우 빠름 / 단점: 최신 파일 못 찾을 수 있음

### find — 디스크 직접 정밀 검색
- 기본 구조: `find [어디서] [조건] [동작]`

#### 이름으로 찾기
- `-name "*.txt"` — 대소문자 구분 (따옴표 필수!)
- `-iname "*.txt"` — 대소문자 무시

#### 타입으로 찾기
- `-type f` — 일반 파일
- `-type d` — 디렉토리
- `-type l` — 심볼릭 링크

#### 탐색 깊이 제한하기
- `-maxdepth 1` — 시작 위치 바로 아래까지만 탐색
- `-mindepth 1` — 시작 위치 자기 자신(`.`)은 결과에서 제외
- `find . -mindepth 1 -maxdepth 1` — 현재 디렉토리의 바로 아래 항목만 대상

깊이 예시:

```text
depth 0: .
depth 1: ./study
depth 1: ./script-agent
depth 2: ./study/git-study
depth 3: ./study/git-study/README.md
```

`-mindepth`는 결과에 포함할 최소 깊이를 정하고, `-maxdepth`는 내려갈 최대 깊이를 정한다.

#### 크기로 찾기
- `-size +100M` — 100MB 초과 (`+`초과, `-`미만)
- 단위: `c`(바이트), `k`(KB), `M`(MB), `G`(GB)

#### 시간으로 찾기
- `-mtime -3` — 3일 이내 수정 (`-`이내, `+`초과)
- `-mmin -60` — 60분 이내 수정
- `-atime` / `-amin` — 접근 시간 기준

#### 권한/소유자
- `-user root` — 특정 사용자 소유
- `-perm 755` — 특정 권한
- `-perm -4000` — setuid 비트 설정된 파일

#### 조건 조합
- AND: 기본 (조건 나열)
- OR: `-o` (`-name "*.jpg" -o -name "*.png"`)
- NOT: `!` 또는 `-not`

### find 동작 (Actions)
- `-ls` — ls -l 형식으로 출력
- `-delete` — 삭제 (확인 없이! 조건 먼저 테스트할 것)
- `-exec 명령어 {} \;` — 파일 하나씩 명령어 실행
- `-exec 명령어 {} +` — 모아서 한 번에 실행 (더 빠름)
- `{} \;` 빼먹으면 에러!

#### `-exec rm -rf -- {} +` 해부

```bash
find . -mindepth 1 -maxdepth 1 -type d ! -name 'study' ! -name '.*' -exec rm -rf -- {} +
```

- `-exec` — 찾은 항목을 뒤 명령어에 넘겨 실행
- `rm -rf` — 디렉토리까지 재귀적으로, 묻지 않고 삭제
- `--` — 옵션 끝 표시. 뒤에 오는 값은 파일/디렉토리 이름으로 해석
- `{}` — `find`가 찾은 경로가 들어가는 자리
- `+` — 여러 경로를 모아서 한 번에 실행

대략 이렇게 실행되는 것과 비슷하다:

```bash
rm -rf -- ./beku-point ./lifestyle-planner ./pdf2txt-cli ./script-agent
```

반대로 `{} \;`는 항목마다 한 번씩 실행한다:

```bash
rm -rf -- ./beku-point
rm -rf -- ./lifestyle-planner
rm -rf -- ./pdf2txt-cli
```

일반적으로 `{} +`가 더 빠르고, `{} \;`는 동작을 이해하기 쉽다.

#### 안전한 삭제 순서

삭제 전에는 먼저 `-print`로 대상 확인:

```bash
find . -mindepth 1 -maxdepth 1 -type d ! -name 'study' ! -name '.*' -print
```

출력이 맞을 때만 삭제:

```bash
find . -mindepth 1 -maxdepth 1 -type d ! -name 'study' ! -name '.*' -exec rm -rf -- {} +
```

### xargs — 파이프로 인자 전달
- `find ... | xargs 명령어` — 검색 결과를 명령어 인자로 전달
- 공백 안전 처리: `find ... -print0 | xargs -0 명령어` (짝으로!)

## 실수하기 쉬운 것

- `find . -name *.log` → 따옴표 빼먹으면 셸이 `*` 먼저 확장
- `-exec rm {}` → `\;` 빼먹으면 에러
- `rm -rf`는 진행률을 출력하지 않음 → 오래 걸리면 멈춘 것처럼 보일 수 있음
- `/mnt/c` 같은 WSL의 Windows 파일시스템에서는 삭제가 더 느릴 수 있음
- `-print0 | xargs` → `-0` 빼먹으면 WARNING
- `xargs wc -l` = 파일 **내용** 줄 수 / `| wc -l` = **목록** 줄 수(개수)
