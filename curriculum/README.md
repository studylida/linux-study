# Linux 학습 커리큘럼

## 진행 현황

- **전체**: 42개 챕터
- **완료**: 28개
- **진행률**: 66.7%
- **다음 챕터**: Ch.28 Writing Your First Script

현재는 새 챕터를 시작하기 전에 [복구 과정](../practice/recovery.md)을 진행하고 있다.

## Part 1: Learning the Shell

- [x] [Ch.1: What Is the Shell?](../notes/part-01-learning-the-shell/ch01-what-is-the-shell.md) — 셸, 터미널, bash, 기본 명령어
- [x] [Ch.2: Navigation](../notes/part-01-learning-the-shell/ch02-navigation.md) — `pwd`, `cd`, `ls`, 절대 경로와 상대 경로
- [x] [Ch.3: Exploring the System](../notes/part-01-learning-the-shell/ch03-exploring-the-system.md) — `ls` 옵션, `file`, `less`, 디렉터리 구조
- [x] [Ch.4: Manipulating Files and Directories](../notes/part-01-learning-the-shell/ch04-manipulating-files-and-directories.md) — `mkdir`, `cp`, `mv`, `rm`, `ln`, 와일드카드
- [x] [Ch.4.5: 파일명 작명법](../notes/part-01-learning-the-shell/ch04-5-file-naming-conventions.md) — 공백과 특수문자를 포함한 파일명 처리
- [x] [Ch.5: Working with Commands](../notes/part-01-learning-the-shell/ch05-working-with-commands.md) — `type`, `which`, `help`, `man`, `apropos`, `alias`
- [x] [Ch.6: Redirection](../notes/part-01-learning-the-shell/ch06-redirection.md) — 표준 스트림, 리다이렉션, 파이프
- [x] [Ch.7: Seeing the World as the Shell Sees It](../notes/part-01-learning-the-shell/ch07-seeing-the-world-as-the-shell-sees-it.md) — 확장과 따옴표
- [x] [Ch.8: Advanced Keyboard Tricks](../notes/part-01-learning-the-shell/ch08-advanced-keyboard-tricks.md) — 편집, 히스토리, 자동 완성
- [x] [Ch.9: Permissions](../notes/part-01-learning-the-shell/ch09-permissions.md) — 권한, `chmod`, `chown`, `sudo`, `su`
- [x] [Ch.10: Processes](../notes/part-01-learning-the-shell/ch10-processes.md) — `ps`, `top`, 작업 제어, `kill`

## Part 2: Configuration and the Environment

- [x] [Ch.11: The Environment](../notes/part-02-configuration-and-environment/ch11-the-environment.md) — 환경변수, `PATH`, 시작 파일
- [x] [Ch.12: Nano 에디터](../notes/part-02-configuration-and-environment/ch12-nano.md) — 편집, 저장, 찾기와 바꾸기
- [x] [Ch.13: A Gentle Introduction to vi](../notes/part-02-configuration-and-environment/ch13-vi.md) — 모드, 이동, 편집, 저장
- [x] [Ch.14: Customizing the Prompt](../notes/part-02-configuration-and-environment/ch14-customizing-the-prompt.md) — `PS1`, 이스케이프 코드, 색상

## Part 3: Common Tasks and Essential Tools

- [x] [Ch.15: Reading Files](../notes/part-03-common-tasks/ch15-reading-files.md) — `cat`, `less`, `head`, `tail`, `wc`, `sort`
- [x] [Ch.16: Piping과 tr](../notes/part-03-common-tasks/ch16-piping-tr.md) — `tr`, `tee`, `uniq`, 파이프 조합
- [x] [Ch.17: Package Management](../notes/part-03-common-tasks/ch17-package-management.md) — `apt`, `yum`, `dnf`
- [x] [Ch.18: Storage Media](../notes/part-03-common-tasks/ch18-storage-media.md) — 마운트, 파티션, 파일시스템
- [x] [Ch.19: Networking](../notes/part-03-common-tasks/ch19-networking.md) — 네트워크 진단, SSH, 파일 전송
- [x] [Ch.20: Searching for Files](../notes/part-03-common-tasks/ch20-searching-for-files.md) — `locate`, `find`, `xargs`
- [x] [Ch.21: Archiving and Backup](../notes/part-03-common-tasks/ch21-archiving-and-backup.md) — `tar`, 압축, `zip`, `rsync`
- [x] [Ch.22: Regular Expressions](../notes/part-03-common-tasks/ch22-regular-expressions.md) — BRE, ERE, 메타문자
- [x] [Ch.23: grep 완전 정복](../notes/part-03-common-tasks/ch23-grep-mastery.md) — 재귀 검색, 추출, 고정 문자열
- [x] [Ch.24: Text Processing](../notes/part-03-common-tasks/ch24-text-processing.md) — `cut`, `paste`, `join`, `comm`, `diff`, `sed`, `awk`
- [x] [Ch.25: Formatting Output](../notes/part-03-common-tasks/ch25-formatting-output.md) — `nl`, `fold`, `fmt`, `pr`, `printf`
- [x] [Ch.26: Printing](../notes/part-03-common-tasks/ch26-printing.md) — CUPS, `lpr`, `lp`, 인쇄 대기열
- [x] [Ch.27: Compiling Programs](../notes/part-03-common-tasks/ch27-compiling-programs.md) — `gcc`, `make`, Makefile

## Part 4: Writing Shell Scripts

- [ ] **Ch.28: Writing Your First Script** — shebang, 실행 권한, `PATH`, 스크립트 실행
- [ ] **Ch.29: Starting a Project** — 스크립트 구조, 주석, 변수
- [ ] **Ch.30: Top-Down Design** — 함수, 지역변수, 모듈화
- [ ] **Ch.31: Flow Control: Branching with if** — `if`, `elif`, `else`, 조건식
- [ ] **Ch.32: Reading Keyboard Input** — `read`, 입력 처리, 유효성 검사
- [ ] **Ch.33: Flow Control: Looping with while/until** — 반복, `break`, `continue`
- [ ] **Ch.34: Troubleshooting** — 디버깅, `set -x`, 오류 처리
- [ ] **Ch.35: Flow Control: Branching with case** — `case`, 패턴 매칭
- [ ] **Ch.36: Positional Parameters** — `$0`, `$1`, `$#`, `$@`, `$*`, `shift`
- [ ] **Ch.37: Flow Control: Looping with for** — `for`, 범위, 배열 순회
- [ ] **Ch.38: Strings and Numbers** — 문자열 조작, 산술 연산, `bc`
- [ ] **Ch.39: Arrays** — 배열 선언, 접근, 순회
- [ ] **Ch.40: Exotica** — 그룹 명령, 서브셸, 프로세스 치환
- [ ] **Ch.41: Cron과 자동화** — `crontab`, cron 문법, 자동 백업
