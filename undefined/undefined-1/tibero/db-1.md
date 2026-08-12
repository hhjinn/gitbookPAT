{% hint style="info" %}
**참고**

- 본 가이드는 고객사 인프라 담당자를 대상으로 합니다.
- 기존 운영 중인 Tibero를 OwlDB에 등록하여 관리하려면 **Tibero 7 이상 버전**이어야 합니다
{% endhint %}

---

이 페이지에서는 기존 운영 중인 Tibero를 OwlDB에 등록하기 전에 준비해야 할 시스템·네트워크·데이터베이스 설정과 배포 파일 배치 방법을 설명합니다.

## 시스템 요구사항

### 디스크(볼륨) 요구사항

기존 데이터베이스가 이미 구성되어 있으므로 Data/Archive/Redo 디스크에 대한 별도 준비는 필요하지 않습니다. Backup 디스크만 확인합니다.

Backup 디스크는 xfs 파일시스템 생성 및 마운트가 완료되어 있어야 합니다.

{% hint style="info" %}
**참고**

TAC 구성의 경우 Backup 디스크는 모든 노드에서 접근 가능한 공유 볼륨이어야 합니다.
{% endhint %}

## 네트워크 요구사항

### **데이터베이스 서버의 필수 포트 구성**

기존 데이터베이스를 OwlDB에 등록하기 위해 아래 포트들이 필요합니다.

| 포트 유형 | 포트 번호 | 용도 | 비고 |
| --- | --- | --- | --- |
| **DB Listener 포트** | 기존 DB 설정값 | - 데이터베이스 접속 포트<br>- Tibero: 예) 8629/tcp | - OwlDB 서버로부터의 DB Listener 포트 인바운드 허용 |

{% hint style="warning" %}
**주의**

기존 데이터베이스가 사용 중인 포트는 변경하지 않습니다. Agent 통신을 위한 40001 포트와 DB 연결을 위한 Listener 포트를 추가로 개방합니다.
{% endhint %}

### 방화벽 설정

OwlDB 서버와 데이터베이스 서버 간 통신을 위해 방화벽 설정이 필요합니다.

---

## 데이터베이스 설정 요구사항

### TAC, DR 구성 시 필수 설정

TAC(Tibero Active Cluster) 또는 DR 구성으로 운영 중인 데이터베이스는 아래 파라미터를 사전에 설정해 주시기 바랍니다.

**DB 파라미터**

| DB 파라미터 | 값 | 설명 |
| --- | --- | --- |
| LOG_ARCHIVE_FORMAT | - | 모든 노드에서 동일한 값으로 설정 |
| STANDBY_USE_OBSERVER | Y | DR 구성 시 필수 |

**CM 파라미터**

| CM 파라미터 | 값 | 설명 |
| --- | --- | --- |
| _CM_REDIRECT_STDOUT_TO_OUTFILE | Y | FS07PS_341175b 패치 필요 |

---

## 배포 파일 준비 및 배치

### 1. 필요 파일 목록

- owldb dp 바이너리 (`owldb-dp-installer-*.tar.gz`)
- 라이선스 파일 (`license.xml`)

{% hint style="info" %}
**참고**

등록 DB는 Tibero가 이미 설치되어 있으므로 Tibero 바이너리가 필요하지 않습니다.
{% endhint %}

### 2. 파일 배치

기존 데이터베이스가 설치된 경로(이하 `설치 디렉터리`)로 이동합니다. (예시: `/home/rocky/owldb`)

기존 데이터베이스의 `TB_HOME`과 같은 준위에 DP 바이너리를 압축 해제합니다.

```bash
# DP 바이너리 압축 해제
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# 라이선스 파일 배치
mv {라이선스 파일} $TB_HOME/license.xml
```

준비 완료 후 `$TB_HOME` 구조는 다음과 같습니다.

```
$TB_HOME/
 ├── license.xml                      # 라이선스 파일
 ├── install/                    # Tibero 설치 스크립트
 ├── validate_infra.sh           # 인프라 검증 스크립트
 ├── install_pkg.sh              # Tibero 패키지 설치 스크립트
 └── tbagent_dist_latest.tar.gz  # tbagent 바이너리
```

이후 데이터베이스 서버 Agent 설치 문서로 이동하여 진행합니다.
