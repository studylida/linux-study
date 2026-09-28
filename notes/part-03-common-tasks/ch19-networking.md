# Ch.19: Networking

## 오늘 배울 것

오늘은 **네트워크 관련 명령어들**을 배울 거야. 서버가 살아있는지 확인하고, 원격으로 접속하고, 파일을 전송하고, API를 호출하는 방법까지! 실무에서 정말 자주 쓰는 것들이야.

---

## 네트워크 진단

### ping — "서버야, 살아있니?"

상대 서버가 살아있는지 확인하는 명령어야. ICMP 패킷을 보내고 응답을 받아:

```bash
ping google.com        # Ctrl+C로 멈출 때까지 계속 보냄
ping -c 4 google.com   # 4번만 보내고 멈춤
```

리눅스/macOS에서는 `Ctrl+C`로 멈춰야 해. 윈도우는 기본 4번만 보내고 끝나.

**중요**: ping이 안 된다고 서버가 죽은 게 아닐 수 있어! 방화벽이 ICMP를 차단하는 경우가 많거든 (네이버 같은 곳도 그래).

주요 옵션:
- `-c N` — 횟수 제한 (Count)
- `-i N` — 간격 (Interval, 초)
- `-W N` — 타임아웃 (Wait, 초)
- `-s N` — 패킷 크기 지정 (Size) — 큰 데이터 전송 문제 진단용

### traceroute — 경로 추적

목적지까지 거쳐가는 **라우터(hop)**를 하나씩 보여줘:

```bash
traceroute google.com
```

`* * *`이 나오면 그 라우터가 응답을 안 하는 거야 (차단된 게 아니야).

주요 옵션:
- `-m N` — 최대 hop 수 (Max)
- `-q N` — hop당 보내는 패킷 수 (기본 3). `-q 1`로 빠르게 확인
- `-w N` — 응답 대기 시간 (초)

### ifconfig / ip — 내 네트워크 정보

```bash
# macOS
ifconfig                    # macOS에서는 이걸 씀 (ip 명령어 없음)
ifconfig | grep "inet "     # IP만 추출

# 리눅스
ip addr show               # 또는 줄여서 ip a (ifconfig은 구식)
```

`inet` 뒤의 숫자가 내 IP 주소야.

### netstat / ss — 네트워크 연결 상태

```bash
# macOS
netstat -an | grep LISTEN      # 열려있는 포트 확인

# 리눅스
ss -tlnp                       # netstat 대신 ss를 씀
```

LISTEN = 연결 대기 중, ESTABLISHED = 현재 연결됨

---

## 원격 접속 & 파일 전송

### ssh — 원격 서버 접속

**S**ecure **Sh**ell. 암호화된 원격 접속이야. 서버 작업의 기본 중의 기본!

```bash
ssh user@server        # 접속 (이후 모든 명령어는 서버에서 실행됨)
exit                   # 나가기 (또는 Ctrl+D)
```

주요 옵션:
- `-p N` — 포트 지정 (기본 22)
- `-i 파일` — 인증키 파일 지정 (AWS 같은 클라우드에서 필수)
- `-v` — 디버그 모드 (접속 안 될 때 원인 파악)
- `-L 로컬포트:localhost:서버포트` — 포트 포워딩 (터널링)

### 포트 포워딩 (ssh -L)

외부에서 직접 접속 안 되는 서비스를 **SSH 터널**로 우회 접속하는 방법이야:

```bash
ssh -L 3307:localhost:3306 user@server
# 내 컴퓨터 localhost:3307 → 터널 → 서버 localhost:3306
```

비유하자면: IP가 **건물 주소**라면, 포트는 **건물 안의 문 번호**야. 터널을 열고 나서 해당 서비스에 맞는 클라이언트로 접속하면 돼 (DB면 mysql 클라이언트, 웹이면 브라우저 등).

### scp — 파일 전송 (SSH 기반)

`cp`의 네트워크 버전이야. 로컬 ↔ 서버 간 파일을 복사해:

```bash
scp myfile.txt user@server:/home/user/     # 업로드
scp user@server:/home/user/file.txt ./     # 다운로드
```

**주의**: 목적지를 생략하면 안 돼! 반드시 `./` 등을 명시해야 해.

주요 옵션:
- `-r` — 디렉토리 전체 복사 (cp -r과 같은 이유)
- `-P N` — 포트 지정 (**대문자 P!** ssh는 소문자 `-p`인데 scp는 대문자!)
- `-i 파일` — 인증키 파일
- `-C` — 압축 전송

---

## 다운로드 & API

### wget — 파일 다운로드 전문

URL에서 파일을 받는 도구야. 기본적으로 **파일로 저장**해:

```bash
wget https://example.com/file.zip
```

주요 옵션:
- `-O 파일` — 파일명 지정 (Output)
- `-c` — 이어받기 (Continue) — 끊겨도 재개 가능!
- `-q` — 조용히 (Quiet)
- `-b` — 백그라운드 다운로드

### curl — 만능 네트워크 도구

URL과 데이터를 주고받는 다용도 도구야. 기본적으로 **화면에 출력**해:

```bash
curl https://example.com
```

주요 옵션:
- `-I` — 헤더만 보기 (Head)
- `-o 파일` — 파일명 지정, `-O` — 원래 이름으로 저장
- `-s` — 진행 표시 숨기기 (Silent) — stderr의 progress 출력 제거
- `-L` — 리다이렉트 따라가기 (Location) — 301 → 200 자동 추적
- `-X METHOD` — HTTP 메서드 지정 (POST, PUT 등)
- `-d 데이터` — 데이터 전송 (Data)
- `-H "헤더"` — 커스텀 헤더 (Header)
- `-v` — 상세 출력 (Verbose)

### wget vs curl — 언제 뭘 써?

- **파일 다운로드만** → wget (이어받기 `-c`가 편함)
- **API 테스트/디버깅** → curl (POST, 헤더 등 다양한 기능)

---

## 정리

오늘 배운 핵심:
- `ping` = 서버 살아있는지 확인 (안 되어도 죽은 게 아닐 수 있음!)
- `traceroute` = 목적지까지의 경로 추적
- **ssh** = 원격 서버 접속의 기본! `-L`로 포트 포워딩도 가능
- **scp** = SSH 기반 파일 전송 (대문자 `-P`로 포트 지정 주의!)
- **wget** = 파일 다운로드 전문, **curl** = 만능 네트워크 도구 (API 테스트)
