# Ch.21: Archiving and Backup

## 핵심 개념

### 묶기(Archiving) vs 압축(Compressing)
- **묶기**: 여러 파일을 하나로 합치는 것 (용량 안 줄어듦) → `tar`
- **압축**: 파일 크기를 줄이는 것 → `gzip`, `bzip2`, `xz`
- 리눅스는 이 두 단계가 분리되어 있음 (윈도우 zip은 동시 처리)
- 보통 `tar로 묶고 → gzip으로 압축` = `.tar.gz`

---

## 명령어 정리

### tar (Tape Archive)

#### 핵심 옵션

| 옵션 | 의미 | 설명 |
|------|------|------|
| `c` | create | 새 아카이브 생성 |
| `x` | extract | 아카이브 풀기 |
| `t` | list | 내용물 목록 보기 (풀지 않고) |
| `v` | verbose | 처리 중인 파일 이름 표시 |
| `f` | file | 아카이브 파일명 지정 |

#### 압축 옵션

| 옵션 | 프로그램 | 확장자 | 특징 |
|------|---------|--------|------|
| `z` | gzip | `.tar.gz` / `.tgz` | 가장 보편적, 빠름 |
| `j` | bzip2 | `.tar.bz2` | 더 잘 압축, 좀 더 느림 |
| `J` | xz | `.tar.xz` | 가장 잘 압축, 가장 느림 |

#### 자주 쓰는 패턴 3개

```bash
# 묶기 + 압축 (Create)
tar czf archive.tar.gz folder/

# 내용물 확인 (lisT)
tar tzf archive.tar.gz

# 풀기 (eXtract)
tar xzf archive.tar.gz

# 특정 디렉토리에 풀기
tar xzf archive.tar.gz -C /tmp/output/
```

외우는 법: **c**zf, **t**zf, **x**zf — 첫 글자만 바뀜!

### gzip / gunzip

```bash
gzip file.txt          # file.txt → file.txt.gz (원본 사라짐!)
gunzip file.txt.gz     # 압축 풀기
gzip -k file.txt       # -k = keep. 원본 유지
gzip -l file.txt.gz    # 압축률 확인
```

⚠️ gzip은 원본 파일을 삭제함! `-k`로 원본 유지.

### 압축률 비교

```
속도:    gzip(빠름) > bzip2 > xz(느림)
압축률:  xz(작음)   > bzip2 > gzip(큼)
보편성:  gzip(최고) > xz    > bzip2
```

### zip / unzip

```bash
zip -r archive.zip folder/    # -r 필수! (폴더 압축시)
unzip archive.zip              # 풀기
unzip -l archive.zip           # 목록만 보기
unzip archive.zip -d /tmp/     # 특정 디렉토리에 풀기
```

#### tar.gz vs zip 선택 기준

| 상황 | 선택 |
|------|------|
| 리눅스끼리 | `tar.gz` (권한, 심볼릭 링크 보존) |
| 윈도우에 보낼 때 | `zip` (호환성) |

### rsync (Remote Sync)

변경된 파일만 복사하는 똑똑한 백업 도구.

```bash
# 로컬 백업
rsync -av source/ destination/

# 원격 백업 (SSH 기반)
rsync -av source/ user@server:/backup/
```

#### 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-a` | archive. 권한, 타임스탬프 등 모두 보존 |
| `-v` | verbose. 진행 상황 표시 |
| `--delete` | 원본에 없는 파일을 대상에서도 삭제 (미러링) |
| `--dry-run` / `-n` | 실제 복사 안 하고 뭘 할지만 보여줌 |

#### 슬래시(/) 규칙 — 주의!

```bash
rsync -av source/ dest/     # 내용물을 dest에 복사
rsync -av source  dest/     # source 폴더 자체를 dest 안에 복사
```

#### --dry-run 습관화

```bash
rsync -avn --delete source/ dest/    # 먼저 확인
rsync -av --delete source/ dest/     # 확인 후 실행
```

`--delete` 쓸 때는 반드시 dry-run 먼저!

---

## 주의사항 & 흔한 실수

- `gzip` 원본 삭제됨 → `-k` 잊지 말기
- `zip -r` 폴더 압축 시 `-r` 빠뜨리면 빈 폴더만 들어감
- `rsync` 슬래시 유무로 결과가 완전히 달라짐
- `--delete`는 위험 → 항상 `--dry-run` 먼저
- `-C`는 Change directory (풀 위치 지정)
