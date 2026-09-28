# Ch.26: Printing

## 핵심 개념

### CUPS (Common Unix Printing System)

- 리눅스/macOS의 **표준 프린팅 시스템**
- Apple이 인수해서 macOS에도 내장
- 웹 관리: `http://localhost:631`

### 프린팅 명령어 두 계열

```
옛날: BSD 계열 vs System V 계열 (서로 호환 안 됨)
  ↓
지금: CUPS가 둘 다 통합 지원
```

| 기능 | BSD 계열 | System V 계열 |
|------|----------|---------------|
| 출력 | `lpr` | `lp` |
| 대기열 확인 | `lpq` | `lpstat` |
| 작업 취소 | `lprm` | `cancel` |

### 주요 명령어

```bash
# 파일 출력
lpr filename.txt
lp filename.txt

# 파이프로 출력
ls -la | lpr

# 프린터 확인
lpstat -p          # 프린터 목록
lpstat -d          # 기본 프린터
lpstat -a          # 전체 상태

# 작업 취소
lprm 작업번호
cancel 작업번호
```

### a2ps (Any to PostScript)

텍스트를 인쇄용으로 보기 좋게 변환:
- 줄 번호, 헤더, 페이지 번호 자동 추가
- 코드 출력할 때 유용 (요즘은 거의 안 씀)

```bash
a2ps filename.txt              # 프린터로 출력
a2ps filename.txt -o output.ps # PostScript 파일로 저장
```

## 한줄 요약

요즘 거의 안 쓰지만, CUPS + lpr/lp 정도는 알아두자.
