OwlDB 온프레미스를 사용하기 위해서는 고객 환경에 몇 가지 사전 준비가 필요합니다. 이 페이지에서는 OwlDB의 구성을 먼저 설명하고, 각 구성 요소별로 어떤 준비가 필요한지 안내합니다.

## 시스템 구성

OwlDB는 두 종류의 서버로 구성됩니다.

| 서버 | 역할 | 설명 |
| --- | --- | --- |
| OwlDB 서버 | 관제 서버 | - OwlDB 애플리케이션이 구동되는 서버<br>- 웹 UI·백엔드 서비스 제공, Agent와 통신해 설치/관제 수행 |
| 데이터베이스 서버 | 관제 대상 서버 | - Tibero 데이터베이스와 OwlDB Agent가 설치되는 서버<br>- OwlDB 서버 명령으로 DB 설치/운영 |

{% hint style="info" %}
**참고**

OwlDB 서버와 데이터베이스 서버는 반드시 별도의 독립된 환경에 구성되어야 합니다.
{% endhint %}

### 통신 구조

OwlDB 서버와 데이터베이스 서버는 각 서버에 설치된 Agent를 통해 통신합니다.

```
[ 사용자 브라우저 ]
        |  HTTP (UI_PORT)
        v
[ OwlDB 서버 ]
   - OwlDB 백엔드
   - OwlDB 프론트엔드
        |  SERVER_PORT (아웃바운드)
        v
[ 데이터베이스 서버 ]
   - OwlDB Agent (tbagent)
   - Tibero DB
```

Agent는 데이터베이스 서버에 설치되어 OwlDB 서버로부터 연결을 수신하고 Tibero 설치/구동/관제에 필요한 작업을 로컬에서 실행합니다.

## 데이터베이스 구성 방식

OwlDB 온프레미스는 데이터베이스를 사용하는 방식에 따라 두 가지 구성을 지원합니다.

- **설치 DB** : OwlDB를 통해 새 데이터베이스를 설치하고 관리하는 방식입니다.
- **등록 DB**: 이미 운영 중인 데이터베이스를 OwlDB에 등록하여 관리하는 방식입니다.

| 구분 | 설치 DB | 등록 DB |
| --- | --- | --- |
| 대상 DB | Tibero 7.2.5, OpenSQL 3.16.14.7, 3.17.10.7 | Tibero 7 이상 버전 |
| 지원 OS | Rocky Linux 9.5 이상 | CentOS 7, Rocky Linux 8/9, RHEL 계열 7/8/9 |

{% hint style="info" %}
**참고**

설치 DB는 OwlDB에서 제공하는 **최신 Tibero 바이너리 버전을 기준으로 설치**됩니다.

구성 방식별 상세 지원 조건과 사전 준비 사항은 [설치 DB 환경 준비 가이드](#W2TxdEHwoC3mStsQfJdo) 또는 [등록 DB 환경 준비 가이드](#pvI81bWJlNtie4nv4stI)를 확인해 주세요.
{% endhint %}

### 필수 패치 목록

OwlDB에서 데이터베이스를 사용하기 위해서는 아래와 같은 패치가 필요합니다.

| 패치명 | 패치 내용 |
| --- | --- |
| FS02PS_318447b | tbboot 시 stdout 닫히지 않는 문제 개선 |
| FS02PS_339919c | tbdown 시 switchover immediate 옵션 추가 |

{% hint style="warning" %}
**주의**

- FS02PS_318447b 미적용 시 **DB 기동** 기능이 미동작합니다.
- FS02PS_339919c 미적용 시 **failover**/**switchover** 기능이 미동작합니다.
{% endhint %}

## 설치 흐름

본 가이드는 아래 순서로 진행됩니다. OwlDB 서버와 데이터베이스 서버 준비는 병렬로 진행할 수 있으며, 모두 준비된 후 연결 확인을 수행합니다.

> 📷 **[이미지]** 이미지

## 배포 파일 구성

설치에 앞서 아래 두 종류의 배포 파일을 준비합니다.

| 배포 파일 | 설치 대상 서버 | 구성 요소 |
| --- | --- | --- |
| `owldb-cp-installer-*.tar.gz` | OwlDB 서버 | - OwlDB 백엔드·프론트엔드 Docker 이미지<br>- 설치 스크립트 |
| `owldb-dp-installer-*.tar.gz` | 데이터베이스 서버 | - Tibero 설치 스크립트<br>- tbagent 바이너리<br>- 인프라 검증 스크립트 |
