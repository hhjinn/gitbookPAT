이 페이지에서는 배포 파일을 배치하고 owldb.env를 설정한 뒤 설치 스크립트를 실행해 OwlDB를 설치합니다.

## 1. 배포 파일 준비 및 배치

- OwlDB cp 바이너리 (`owldb-cp-installer-%Y%m%d-%H.tar.gz`)
- OwlDB 라이선스 파일 (`license.xml`)

### 1-1. 배포 파일 압축 해제

OwlDB 서버 내 원하는 경로에 CP 배포 파일을 압축 해제합니다.

```bash
tar -zxvf owldb-cp-installer-%Y%m%d-%H.tar.gz
```

### 1-2. OwlDB 라이선스 배치

OwlDB는 기동 시점에 OwlDB 라이선스를 검증합니다. 유효한 라이선스가 없으면 Backend 컨테이너가 정상 기동되지 않고 설치 스크립트의 Health Check 단계에서 실패하므로, 설치 스크립트 실행 전에 발급 받은 라이선스 파일을 반드시 배치해야 합니다.

발급 받은 라이선스 파일을 압축 해제한 배포 파일 내 `/license/license.xml` 경로에 배치합니다.

준비 완료 후 디렉토리 구조는 다음과 같습니다.

```
├── owldb-cp-installer-%Y%m%d-%H.tar.gz
└── owldb-cp-installer
    ├── docker-compose.yaml.template  # Docker Compose 설정 파일 템플릿
    ├── license
    │   └── license.xml         # OwlDB 라이선스
    ├── owldb-be
    │   ├── owldb_be.tar        # Backend Docker 이미지
    │   └── owldb_h2.tar        # H2 Docker 이미지
    ├── owldb-fe                
    │   └── owldb_fe.tar        # Frontend Docker 이미지
    ├── owldb.env               # OwlDB env 파일
    ├── owldb_install.sh        # OwlDB 설정 스크립트
    └── validate_infra.sh       # 인프라 검증 스크립트
```

{% hint style="warning" %}
**주의**

- on-premise 환경은 ops, fleet `edition` 라이선스만 허용됩니다.
- 설치 날짜가 `start_date` 이전 / `end_date`+`grace_period` 이후인 라이선스는 기동에 실패합니다. (`grace_period` 기간 중에는 경고 로그와 함께 기동됩니다.)
{% endhint %}

## 2. owldb.env 설정

설치 전 `owldb.env` 파일에 아래 항목을 입력합니다.

```bash
############################################################
#                     OWLDB ENV FILE                       #
#----------------------------------------------------------#
# This file contains environment variables for OwlDB      #
# Do NOT add spaces around '='                            #
# Lines starting with '#' are comments                    #
############################################################

# ------------------------------
# UI Configuration
# ------------------------------
UI_PORT=

# ------------------------------
# Server Configuration
# ------------------------------
SERVER_PORT=

# ------------------------------
# Database Credentials
# ------------------------------
DB_USERNAME=
DB_PASSWORD=

############################################################
# End of owldb.env
############################################################
```

| 항목 | 설명 | 예시 | 필수 여부 |
| --- | --- | --- | --- |
| UI_PORT | OwlDB 웹 UI 접속 포트 | `80` | 필수 |
| SERVER_PORT | OwlDB 서버 포트 | `8080` | 필수 |
| DB_USERNAME | OwlDB 메타 DB 계정 ID | `db_user` | 필수 |
| DB_PASSWORD | OwlDB 메타 DB 비밀번호 | `db_password` | 필수 |

{% hint style="warning" %}
**주의**

DB_USERNAME / DB_PASSWORD는 최초 설정 이후 변경이 불가능합니다.
{% endhint %}

## 3. 설치 스크립트 실행

`owldb.env` 파일 설정 완료 후 아래 명령어를 실행합니다.

```bash
sudo bash ./owldb_install.sh
```

{% hint style="info" %}
**참고**

스크립트 실행 전 [환경 준비 사항](#XOyYOS20WfPqABW42fpm)을 완료했는지 확인해주세요.
{% endhint %}

스크립트는 다음 순서로 동작합니다.

1. 하드웨어 및 패키지 사전 검증
2. Docker 이미지 로드
3. `docker-compose.yml` 생성 및 컨테이너 기동
4. Health Check 수행

**실행 결과 예시**

```bash
$ sudo bash owldb_install.sh 
[INFO] owldb.env 파일에서 설정을 읽습니다: ./owldb.env
======================================
 Pre-Check Script
 MODE : CP
======================================

== Hardware Check ==
[PASS] CPU : 16 cores (minimum 2 cores)
[PASS] Memory : Total 15 GB, Available 14 GB (CP requires Total ≥ 8GB)

== Package Check ==
[PASS] Package : docker (Docker version 29.1.3, build f52814d)
[PASS] Package : docker-compose (Docker Compose version v2.40.3-desktop.1)

======================================
 Result Summary
======================================
 PASS : 4
 FAIL : 0
 Overall Result : PASS
[INFO] H2 이미지 로드 중...
Loaded image: owldb_h2:latest
[INFO] BE 이미지 로드 중...
Loaded image: owldb_be:20260303-19
[INFO] FE 이미지 로드 중...
Loaded image: owldb_fe:20260303-19
[INFO] 이미지 로드 및 파일 준비 완료.
[INFO] docker-compose.yaml 치환 완료.
[+] Running 4/4
 ✔ Network owldb-net   Created                                                                                                                                                                         0.0s 
 ✔ Container owldb_h2  Started                                                                                                                                                                         0.5s 
 ✔ Container owldb_be  Started                                                                                                                                                                         0.6s 
 ✔ Container owldb_fe  Started                                                                                                                                                                         0.7s 
[INFO] 🔍 Health Check
[INFO] Health Check Failed (Attempt 1): Retrying in 10 seconds...
[INFO] Health Check Failed (Attempt 2): Retrying in 10 seconds...
[INFO] Health Check Success: OK
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    24    0     0  100    24      0    132 --:--:-- --:--:-- --:--:--   132
Response: 
[INFO] OwlDB 설치 완료. UI 접속: http://[OwlDB 호스트 IP]:80/owldb/#/auth/login
```

## 4. 설치 결과 확인

브라우저에서 아래 URL로 접속하여 로그인 화면이 표시되는지 확인합니다.

```
http://[OwlDB 서버 IP]:[UI_PORT]/owldb/#/auth/login
```

{% hint style="info" %}
**참고**

초기 root 계정 정보: ID `admin` / Password `admin` 입니다.
{% endhint %}

---

### 포트 변경이 필요한 경우

`owldb.env` 에서 포트 값을 수정 후 스크립트를 재실행합니다. 아래 프롬프트가 표시되면 **1번**을 선택합니다.

- UI_PORT
- SERVER_PORT

```
$sudo bash owldb_install.sh 
...
⚠️  경고: docker-compose.yaml 파일이 이미 존재합니다.

  1) 기존 docker-compose.yaml을 덮어쓰고 계속 진행
  2) 기존 docker-compose.yaml을 그대로 사용하고 계속 진행
  3) 취소

선택 [1-3]: 
```
