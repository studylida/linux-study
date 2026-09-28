# 새로 배운 용어/개념

## Ch.9 Permissions

- **권한 숫자 계산**: r=4, w=2, x=1을 더한다. rwx=7, rw-=6, r-x=5, r--=4, ---=0
- **chmod 숫자 방식**: `chmod 755 file` = owner/group/others 순서로 3자리
- **chmod 기호 방식**: `chmod u+x file`. 누구(u/g/o/a) + 동작(+/-/=) + 권한(r/w/x)
- **권한 체크 순서**: owner → group → others 순서로, 첫 매칭에서 끝. owner이면서 group이어도 owner 권한만 적용
- **sudo**: Super User Do. 명령어 하나만 관리자 권한으로 실행. 실무에서 주로 사용
- **su**: Switch User. 사용자 통째로 전환. 거의 안 씀
- **chown**: change owner. 소유자/그룹 변경. sudo 필요 (남의 파일 가로채기 방지)

## Ch.10 Processes

- **프로세스 vs 프로그램**: 프로그램 = 디스크의 파일, 프로세스 = 실행 중인 프로그램 (메모리에 올라간 상태)
- **ps aux**: 시스템 전체 프로세스 목록. USER, PID, %CPU, %MEM, COMMAND 등 표시
- **top**: 실시간 프로세스 모니터링. q로 나가기
- **& (백그라운드 실행)**: `명령어 &`로 처음부터 백그라운드에서 실행
- **Ctrl+Z / bg / fg**: Ctrl+Z = 일시 정지, bg %번호 = 백그라운드 재개, fg %번호 = 포그라운드로
- **fg/bg는 jobs 번호(%), kill은 PID**: `fg %1` vs `kill 12345`
- **kill vs kill -9**: kill = 종료 요청(정리 시간), kill -9 = 강제 즉시 종료(최후의 수단)
- **killall**: 프로세스 이름으로 종료. `killall firefox`

## Ch.11 The Environment

- **셸 변수 vs 환경변수**: 셸 변수 = 현재 셸에서만. 환경변수 = `export`하면 자식 프로세스에도 전달
- **printenv**: 환경변수 목록 확인. `printenv PATH`로 특정 변수 확인 가능
- **PATH**: 명령어 검색 경로. `:`으로 구분, 왼쪽부터 탐색. 추가: `export PATH="$PATH:/새경로"`
- **변수 할당 규칙**: `=` 앞뒤 공백 금지! `VAR="값"` ✅ / `VAR = "값"` ❌
- **.bashrc vs .bash_profile**: `.bashrc` = 비로그인 셸(매번), `.bash_profile` = 로그인 셸(SSH 등)
- **source**: 설정 파일 다시 불러오기. `source ~/.bashrc` = `. ~/.bashrc`
- **unset**: 변수 삭제. `unset VAR`

## Ch.12 Nano 에디터

- **nano 기본**: `nano filename`으로 열기. `^` = Ctrl 키
- **저장하고 나가기**: `Ctrl+X` → `Y` → `Enter`
- **Ctrl+K / Ctrl+U**: 줄 잘라내기(Kill) / 붙여넣기(Uncut). bash의 Kill/Yank과 비슷하지만 붙여넣기 키 다름!
- **Ctrl+W**: 찾기 (Where is)
- **Ctrl+\\**: 찾아 바꾸기 (Y=하나씩, A=전부)
- **Ctrl+_**: 특정 줄 번호로 이동

## Ch.13 vi/vim

- **vi의 3가지 모드**: Normal(기본, 이동/삭제/복사), Insert(텍스트 입력), Command(저장/종료/찾기바꾸기)
- **모드 전환**: Insert 진입 = `i`/`a`/`o`, Normal 복귀 = `Esc`, Command 진입 = `:`(Normal에서)
- **저장/종료**: `:w` 저장, `:q` 종료, `:wq` 저장+종료, `:q!` 강제종료, `ZZ` 저장+종료(Normal)
- **커서 이동 hjkl**: h=왼, j=아래, k=위, l=오른. `0`=줄 처음, `$`=줄 끝, `w`=다음 단어, `b`=이전 단어
- **파일 이동**: `gg`=맨 처음, `G`=맨 끝, `숫자G`=해당 줄로
- **Insert 진입 6가지**: `i`(앞), `a`(뒤), `o`(아래 새줄), `I`(줄 맨앞), `A`(줄 맨뒤), `O`(위 새줄)
- **삭제**: `x`=글자 1개, `dd`=줄 삭제, `dw`=단어 삭제, `d$`=줄 끝까지, `3dd`=3줄 삭제
- **복사/붙여넣기**: `yy`=줄 복사(yank), `yw`=단어 복사, `p`=뒤에 붙여넣기, `P`=앞에 붙여넣기. `dd`+`p`=줄 이동(잘라내기)
- **실행 취소**: `u`=undo, `Ctrl-r`=redo
- **찾기**: `/단어`=아래로 검색, `?단어`=위로 검색, `n`=다음, `N`=이전
- **찾기/바꾸기**: `:%s/찾을것/바꿀것/g` = 전체 바꾸기, `gc` = 하나씩 확인하며 바꾸기(confirm)

## Ch.14 Customizing the Prompt

- **PS1**: 프롬프트를 저장하는 환경변수. `echo $PS1`로 확인, `.bashrc`에 추가하면 영구 적용
- **이스케이프 코드**: `\u`(유저명), `\h`(호스트명), `\w`(전체 경로), `\W`(폴더명만), `\t`(24시간), `\@`(12시간 AM/PM), `\n`(줄바꿈), `\$`($또는#)
- **색상 코드**: `\[\033[코드m\]내용\[\033[00m\]`. 01;31=빨강, 01;32=초록, 01;33=노랑, 01;34=파랑, 00=리셋
- **`\[...\]` 규칙**: 색상 코드를 반드시 감싸야 함. bash에게 "화면에 안 보이는 문자"라고 알려줘서 프롬프트 길이 계산 오류 방지
- **PS2**: 줄이 이어질 때 나오는 보조 프롬프트. 기본값 `>`

## Ch.15 Reading Files

- **cat**: concatenate. 파일 전체 출력. `-n` 줄 번호, `-A` 숨겨진 문자 표시
- **tac**: cat 거꾸로. 마지막 줄부터 출력. 로그에서 최신 내용 먼저 볼 때
- **rev**: 각 줄의 글자 순서를 좌우 반전. tac(줄 순서)과 구분!
- **head -n N**: 파일 앞에서 N줄 출력 (기본 10줄). `-n -3` = 마지막 3줄 제외
- **tail -n N**: 파일 뒤에서 N줄 출력 (기본 10줄). `-n +3` = 3번째 줄부터 끝까지
- **tail -f**: follow. 파일에 추가되는 내용을 실시간 출력. 로그 모니터링 필수 명령어
- **wc**: Word Count. `-l` 줄 수, `-w` 단어 수, `-c` 바이트 수, `-m` 문자 수
- **sort**: 정렬. `-n` 숫자 기준(없으면 문자열 정렬로 1,10,2 됨!), `-r` 역순, `-k N` N번째 필드, `-u` 중복 제거
- **Useless Use of Cat**: `cat file | cmd` 대신 `cmd file`로 직접 전달이 효율적 (불필요한 프로세스 방지)

## Ch.16 Piping과 tr

- **tr**: Translate. 문자 단위 변환. 파일 직접 읽기 불가 — 반드시 파이프(`|`) 또는 리다이렉션(`<`)으로 입력
- **tr -d**: 지정한 문자 삭제. `tr -d '0-9'`로 숫자 전부 삭제. `-d` 없으면 변환이지 삭제가 아님!
- **tr -s**: squeeze. 연속된 중복 문자를 하나로 압축. `tr -s ' '`으로 다중 공백 정리
- **tr 문자 클래스**: `[:lower:]`, `[:upper:]`, `[:digit:]`, `[:space:]`, `[:punct:]` — 범위 대신 표준 클래스 사용 가능
- **tee**: T자 파이프. stdout과 파일에 동시 출력. 배관의 T자 연결부 비유
- **tee -a**: append 모드. 기존 파일 내용 유지하면서 추가 (`>>` 같은 효과)
- ~~**uniq**~~ ✅ 2026-03-02 practicing으로 승급
- **uniq -c**: count. 각 줄의 등장 횟수 표시
- **uniq -d**: duplicated. 2번 이상 나온 줄만 출력
- **uniq -u**: unique. 정확히 1번만 나온 줄만 출력
- **sort | uniq -c | sort -rn**: 빈도 분석 필수 패턴. sort -u로는 횟수를 셀 수 없음

## Ch.17 Package Management

- **패키지(Package)**: 프로그램을 설치 가능하게 묶은 파일. 실행파일 + 설정파일 + 의존성 정보 포함
- **의존성(Dependency)**: 프로그램 A가 동작하려면 라이브러리 B가 필요한 관계. 패키지 매니저가 자동 해결
- **저장소(Repository)**: 패키지들이 모여있는 온라인 서버. 앱스토어 같은 것
- **데비안 계열 vs 레드햇 계열**: 데비안(.deb, apt) = Ubuntu/Debian, 레드햇(.rpm, yum/dnf) = CentOS/RHEL/Fedora
- **apt update vs apt upgrade**: update = 저장소 목록 갱신(설치X), upgrade = 실제 패키지 업그레이드. 세트로 씀
- **apt remove vs apt purge**: remove = 설정파일 남김(재설치 시 복원), purge = 설정파일까지 완전 삭제
- **sudo 필요 여부**: 시스템 변경(install/remove/update/upgrade) = sudo 필요, 조회(search/show/list) = 불필요
- **dnf**: yum의 후속 버전. RHEL 8+, Fedora에서 사용. 명령어 사용법 동일

## Ch.18 Storage Media

- **마운트(mount)**: 저장장치를 파일 트리의 특정 디렉토리에 연결하는 것. `sudo mount /dev/sdb1 /mnt/usb`
- **umount**: 마운트 해제. `unmount`가 아니라 **`umount`** (n 없음!). `sudo umount /mnt/usb`
- **장치 이름 규칙**: `sda`=첫 번째 디스크, `sda1`=첫 번째 파티션, `sdb1`=두 번째 디스크의 첫 번째 파티션
- **lsblk**: 블록 장치 트리 구조로 보기. macOS 동등 명령어: `diskutil list`
- **fdisk -l**: 모든 디스크/파티션 목록 확인. `sudo fdisk -l /dev/sda`로 특정 디스크만
- **mkfs**: make filesystem (포맷). `mkfs.ext4`, `mkfs.vfat`, `mkfs.ntfs` — 포맷하면 데이터 삭제!
- **/etc/fstab**: 부팅 시 자동 마운트 설정 파일. 장치/마운트위치/파일시스템/옵션/dump/pass 6개 필드
- **UUID 사용 이유**: 장치명(`sda`, `sdb`)은 꽂는 순서에 따라 바뀔 수 있음. UUID는 고유 ID라 안전. `blkid`로 확인
- **Microsoft Basic Data**: diskutil에서 보이는 타입. exFAT 또는 NTFS로 포맷된 윈도우 호환 파티션
- **nodev/nosuid/noowners/noatime**: nodev=디바이스파일 차단, nosuid=setuid 무시, noowners=소유자 무시(ExFAT 등), noatime=접근시간 기록 안 함(SSD 수명+성능)
- **setuid 비트**: `chmod 4755`의 4. 실행 시 파일 소유자 권한으로 실행됨. `ls -l`에서 `s`로 표시. 예: passwd 명령어
- **sealed (macOS)**: OS 볼륨 봉인. 시스템 파일 변조 방지, read-only로 마운트됨

## Ch.19 Networking

- **ping**: 서버 연결 확인. `-c N` 횟수, `-i N` 간격, `-s N` 패킷 크기 지정. ping 안 됨 ≠ 서버 죽음 (방화벽 차단 가능)
- **traceroute**: 목적지까지 경로 추적. 각 hop = 거쳐가는 라우터. `-m N` 최대 hop, `-q N` hop당 패킷 수 (기본 3, 빠르게는 1)
- **ifconfig vs ip**: macOS는 `ifconfig`, 리눅스는 `ip a`. `ifconfig | grep "inet "`으로 IP 추출
- **netstat vs ss**: macOS는 `netstat`, 리눅스는 `ss`. macOS: `netstat -an | grep LISTEN`
- **ssh**: 원격 접속. `-p`(소문자) 포트, `-i` 키파일, `-v` 디버그, `-L` 포트포워딩
- **ssh -L (포트포워딩)**: `ssh -L 로컬포트:localhost:서버포트 user@server`. 외부 차단된 서비스를 SSH 터널로 우회 접속. 터널 열고 해당 서비스 클라이언트로 접속
- **포트**: 건물의 문 번호. IP = 주소, 포트 = 문. 서비스마다 고유 포트 사용 (22=SSH, 80=HTTP, 443=HTTPS, 3306=MySQL)
- **scp**: SSH 기반 파일 복사. `scp 출발지 목적지` — 목적지 생략 불가! `-r` 디렉토리, `-P`(**대문자**) 포트. ssh는 -p(소문자)
- **wget**: 파일 다운로드 전문. `-O` 파일명, `-c` 이어받기(끊겨도 재개), 기본 파일로 저장
- **curl**: 만능 네트워크 도구. `-I` 헤더만, `-s` 조용히, `-L` 리다이렉트 따라가기, `-X` 메서드, `-d` 데이터, `-H` 커스텀 헤더. 기본 화면 출력
- ~~**wget vs curl**~~ ✅ 2026-03-02 practicing으로 승급

## Ch.20 Searching for Files

- **locate**: DB 기반 파일 검색. 매우 빠르지만 실시간 아님. `sudo updatedb`로 DB 갱신
- **find 기본 구조**: `find [어디서] [조건] [동작]`. 디스크 직접 탐색이라 느리지만 정밀
- **-name vs -iname**: `-name` = 대소문자 구분, `-iname` = 대소문자 무시. 패턴은 반드시 따옴표!
- **-type f/d/l**: f=일반파일, d=디렉토리, l=심볼릭링크
- **-size +100M**: `+`초과, `-`미만. 단위: c(바이트), k(KB), M(MB), G(GB)
- **-mtime -3**: 3일 이내 수정. `-mmin`은 분 단위. `+`는 오래된 것, `-`는 최근 것
- **-exec {} \;**: 찾은 파일에 명령어 실행. `{}`=파일명 자리, `\;`=종료 표시. 둘 다 빠지면 에러!
- ~~**-exec {} + vs {} \;**~~ ✅ 2026-03-16 mastered로 승급
- **-delete**: 찾은 파일 삭제. 확인 없이 바로 삭제! 반드시 조건만 먼저 테스트
- **find 조건 조합**: AND=기본, OR=`-o`, NOT=`!` 또는 `-not`
- **-print0 | xargs -0**: 공백 있는 파일명 안전 처리. 반드시 짝으로 사용
- **| wc -l vs xargs wc -l**: `| wc -l` = find 결과 개수, `xargs wc -l` = 파일 내용의 줄 수

## Ch.21 Archiving and Backup

- **묶기 vs 압축**: tar = 묶기(용량 안 줄어듦), gzip/bzip2/xz = 압축(용량 줄어듦). 리눅스는 두 단계 분리
- ~~**tar czf/xzf/tzf**~~ ✅ 2026-03-04 practicing으로 승급
- **tar -C**: Change directory. 풀 위치 지정. `tar xzf file.tar.gz -C /tmp/`
- **gzip 원본 삭제**: `gzip file` 하면 원본 사라짐! `-k`(keep)로 원본 유지
- **압축 3종 비교**: 속도 gzip>bzip2>xz, 압축률 xz>bzip2>gzip. 실무 90%는 gzip
- **zip -r 필수**: 폴더 압축 시 `-r` 없으면 빈 폴더만 들어감. `unzip -l`로 목록 확인
- **tar.gz vs zip**: 리눅스끼리=tar.gz(권한 보존), 윈도우 호환=zip
- **rsync -av**: 변경된 파일만 복사. a=archive(권한 보존), v=verbose
- **rsync 슬래시 규칙**: `source/` = 내용물 복사, `source` = 폴더 자체를 dest 안에 복사
- **rsync --delete**: 원본에 없는 파일을 대상에서도 삭제. 반드시 `--dry-run`(-n) 먼저!
- **--dry-run**: 실제 실행 안 하고 뭘 할지만 보여줌. --delete 전 필수 습관

## Ch.22 Regular Expressions

- **정규식(Regex)**: 텍스트 패턴 표현 언어. grep, sed, awk, vim, Python 등 거의 모든 곳에서 사용
- **`.` (마침표)**: 아무 문자 1개. `h.t` → hat, hot, hit. 줄바꿈 제외
- **`^`와 `$` (앵커)**: `^` = 줄 시작, `$` = 줄 끝. `^$` = 빈 줄
- ~~**`^`의 이중 의미**~~ ✅ 2026-03-16 mastered로 승급
- **`[]` 문자 클래스**: 안의 문자 중 하나. `[0-9]` 숫자, `[a-z]` 소문자, `[A-Za-z]` 영문자
- **`*` (0번 이상)**: 앞 문자 0번 이상 반복. `ab*c` → ac, abc, abbc. `.*` = 아무거나 다
- **와일드카드 ≠ 정규식**: glob `*`=아무거나, 정규식 `*`=앞 문자 반복. 정규식의 "아무거나"는 `.*`
- ~~**BRE vs ERE**~~ ✅ 2026-03-04 practicing으로 승급
- **`+` (1번 이상)**: `ab+c` → abc, abbc ✅ / ac ❌. `*`와 달리 최소 1번 필요
- **`?` (0 또는 1번)**: `colou?r` → color, colour ✅ / colouur ❌
- **`{n,m}` (n~m번)**: `[0-9]{3}` = 숫자 3자리. `{3,}` = 3자리 이상
- **`|` (OR)**: `cat|dog`. 공백 넣지 마! 공백도 패턴의 일부
- **`()` (그룹화)**: `^(ERROR|WARNING)` = ERROR 또는 WARNING으로 시작

## Ch.23 grep 완전 정복

- **`-rn` 세트**: 재귀 검색 시 거의 항상 `-r`(재귀) + `-n`(줄 번호) 함께 사용
- **`-w` 단어 매칭**: `grep -w 'is'`는 "is"만 잡고 "this", "island"는 안 잡힘. `\b`와 같은 효과
- **`-o` 매치 부분만 추출**: `grep -oE '[0-9]+' file`로 숫자 덩어리만 뽑기. `-o` 없으면 줄 전체 출력
- **`-v` 반전**: 매치 안 되는 줄 출력. `grep -vE '^#|^$'` = 주석과 빈 줄 제외
- **`-F` 고정 문자열**: 정규식 끄기. `grep -F '192.168.1.1'`로 `.`을 문자 그대로 검색. 메타문자 이스케이프 불필요
- **`-A/-B/-C` 컨텍스트**: After/Before/Context. `grep -C 3 'ERROR' log`로 에러 전후 3줄 표시
- **`--include`/`--exclude-dir`**: `grep -rn --include='*.py' --exclude-dir='.git' 'TODO' .` 파일/디렉토리 필터링
- **`-c` 카운트, `-l` 파일명만**: `-c`=매치 줄 수, `-l`=매치된 파일명 목록, `-L`=매치 안 된 파일명
- **파이프 받을 때 파일명 금지**: `cmd | grep 'A' file` → 파이프 무시하고 file 읽음! 파일명 빼야 파이프 입력 사용
- **egrep/fgrep deprecated**: `egrep` = `grep -E`, `fgrep` = `grep -F`. 새 스크립트에서는 옵션 방식 사용

## Ch.24 Text Processing

- ~~**cut vs awk 연속 공백**~~ ✅ 2026-03-16 mastered로 승급
- **cut -d -f**: 구분자(`-d`)로 나눠서 필드(`-f`) 추출. 기본 구분자=탭. 구분자는 한 글자만 가능
- **paste**: 파일을 옆으로 합침. `-d` 구분자, `-s` 한 줄로 합침
- **join**: 공통 필드로 두 파일 합침 (SQL JOIN). 두 파일 모두 정렬 필수
- **comm**: 정렬된 두 파일 집합 비교. 3열(파일1만/파일2만/공통). `-12`=공통만, `-23`=파일1만, `-13`=파일2만
- ~~**diff -u**~~ ✅ 2026-03-13 practicing으로 승급
- **diff -rq**: 디렉토리 비교에서 다른 파일 목록만 빠르게 확인
- **sed 's/A/B/g'**: 찾아 바꾸기. `g` 없으면 줄당 첫 매치만, `g` 있으면 전부
- **sed -i**: 파일 직접 수정. 되돌릴 수 없으니 먼저 화면에서 확인하는 습관!
- **sed 구분자 변경**: 경로에 `/` 있으면 `s|A|B|g` 또는 `s#A#B#g` 사용
- **sed '/패턴/d'**: 패턴 매치 줄 삭제. `sed '/^#/d; /^$/d'` = 주석과 빈 줄 제거
- **awk '{print $N}'**: N번째 필드 출력. $0=전체, $NF=마지막. 연속 공백 자동 처리 (cut과 핵심 차이)
- **awk -F**: 필드 구분자 지정. `awk -F: '{print $1}' /etc/passwd`
- **awk 조건문**: `awk '$2 >= 80 {print $1}'` = 조건 만족하는 줄만 처리
- **awk END 블록**: 모든 줄 처리 후 실행. `awk '{sum+=$2} END {print sum}'` = 합계 계산
- **awk는 작은따옴표 필수**: 큰따옴표 쓰면 셸이 $1을 해석 → 줄 전체 출력됨

## Ch.25 Formatting Output

- **nl vs cat -n**: 둘 다 줄 번호 붙이지만, nl은 빈 줄 건너뜀(기본), cat -n은 빈 줄에도 번호
- **nl -n rz -w 3**: 0으로 채운 3자리 번호(001, 002...). `-n rz`는 분리해서 써야 함 (`-rz` 에러!)
- **fold -w N -s**: 긴 줄을 N자 폭에서 단어 단위로 자름. `-s` 없으면 단어 중간에서 잘림
- **fold vs fmt**: fold=가위(자르기만), fmt=편집자(짧은 줄 합쳐서 문단 재배치)
- ~~**printf 줄바꿈 없음**~~ ✅ 2026-03-16 mastered로 승급
- **printf %s/%d/%f**: %s=문자열, %d=정수, %f=소수. %x=16진수, %o=8진수, %%=%문자
- **printf 폭/정렬**: `%10s`=오른쪽 정렬, `%-10s`=왼쪽 정렬, `%05d`=0채움, `%.2f`=소수 2자리
- **printf 인자 반복**: 형식보다 인자가 많으면 자동으로 형식을 반복 적용

## Ch.26 Printing

- **CUPS**: Common Unix Printing System. 리눅스/macOS 표준 프린팅 시스템. `http://localhost:631`로 웹 관리
- **lpr vs lp**: 둘 다 파일 출력. lpr=BSD 계열, lp=System V 계열. CUPS가 둘 다 지원
- **lpq/lpstat**: 인쇄 대기열 확인. lpq=BSD, lpstat=System V. `lpstat -p`=프린터 목록, `-d`=기본 프린터
- **lprm/cancel**: 인쇄 작업 취소. lprm=BSD, cancel=System V
- **a2ps**: Any to PostScript. 텍스트를 줄번호/헤더 붙여서 인쇄용으로 변환

## Ch.27 Compiling Programs

- **컴파일 4단계**: 전처리(#include 처리) → 컴파일(C→어셈블리) → 어셈블(→기계어) → 링킹(라이브러리 연결, 실행파일 생성)
- **gcc -c**: 컴파일만 하고 링킹 안 함. `.o` (오브젝트 파일) 생성. 실행파일이 아님!
- **gcc -Wall**: 모든 경고 표시. 실수 잡아줌, 항상 쓰는 게 좋음
- **gcc -o**: output. 실행파일 이름 지정. 없으면 기본 `a.out`
- **./로 실행하는 이유**: 현재 디렉토리(.)는 보통 PATH에 없어서 경로를 명시해야 함
- **.h vs .c**: .h=선언(메뉴판), .c=구현(주방). .c가 .h를 include (반대 아님!)
- **Makefile 들여쓰기는 Tab 필수**: 스페이스 쓰면 `missing separator` 에러
- **Makefile 구조**: `타겟: 의존성` 줄 다음에 Tab+명령어. 변경된 파일만 다시 컴파일
- **Makefile 관례 변수**: CC=컴파일러, CFLAGS=옵션. `$(변수명)`으로 참조
- **make의 핵심**: 파일 변경 시간 추적 → 바뀐 것만 재컴파일 (대형 프로젝트에서 빌드 시간 절약)
- **./configure**: 시스템 환경 체크(컴파일러/라이브러리 확인) 후 Makefile을 자동 생성해주는 스크립트
- **오픈소스 설치 3단계**: `./configure`(Makefile 생성) → `make`(빌드) → `sudo make install`(시스템에 설치)
- **clean 타겟**: `make clean`으로 빌드 결과물(.o, 실행파일) 정리. 의존성 없이 명령어만 실행
