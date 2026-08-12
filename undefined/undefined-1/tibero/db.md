OwlDB 데이터베이스 설치를 위한 시스템·네트워크 요구사항을 확인하고 배포 파일과 인프라를 준비합니다.

{% hint style="info" %}
**참고**

본 가이드는 고객사 인프라 담당자를 대상으로 합니다.
{% endhint %}

---

## 시스템 요구사항

### 1. 하드웨어 요구사항

| 항목 | 최소 사양 | 비고 |
| --- | --- | --- |
| CPU | 2 Core 이상 | - |
| Memory | 4GB 이상 | - |
| Disk | - | 3. 디스크(볼륨) 요구사항 확인 |

### 2. 운영체제 요구사항

| 항목 | 요구사항 |
| --- | --- |
| OS | Rocky Linux 9.5 이상 |

### 3. 디스크(볼륨) 요구사항

TAC 구성의 경우 아래의 요구 사항을 따라야 합니다.

- TAC 구성에서 사용되는 모든 디스크는 **모든 DB 노드에서 접근 가능한 공유 볼륨**이어야 합니다.

| 용도 | 요구 사항 |
| --- | --- |
| Data / Archive / Redo | - 공유 볼륨, 로우 디바이스 또는 파티셔닝 경로 형태로 준비<br>- 파일 시스템 생성 금지 |
| Backup | 공유 볼륨, 파일 시스템 경로로 준비 |

{% hint style="warning" %}
**주의**

OwlDB가 Data/Archive/Redo 디스크에 Tibero 전용 파일 시스템(TAS)을 자동으로 구성하므로, 파티셔닝 또는 파일시스템을 미리 생성하면 안 됩니다.
{% endhint %}

### 4. 커널 파라미터 설정

Tibero 데이터베이스 구동을 위해 아래 커널 파라미터를 설정해야 합니다. Tibero **설치가이드** 내 [커널 파라미터 설정](https://docs.tibero.com/tibero/topics/installation/installaion-guide/installation-prerequisites#undefined-4)을 통해 설정합니다.

## 네트워크 요구사항

### 데이터베이스 서버 필수 포트 구성

신규 데이터베이스 설치를 위해 다음 포트들이 필요합니다.

| 포트 유형 | 포트 번호 | 용도 | 비고 |
| --- | --- | --- | --- |
| **DB Listener 포트** | 예시) 8629/tcp | 데이터베이스 접속 포트 | OwlDB 서버로부터의 8629 인바운드 허용 |
| **노드 간 내부 연결 포트** | 예시) 8630~8679/tcp | - TAC 구성 시 노드 간 내부 통신 포트<br>- DB Listener 포트 기준 +50 범위 | 노드 간 인바운드/아웃바운드 모두 허용 (TAC, DR 구성 시에만 필요) |

{% hint style="warning" %}
**주의**

데이터베이스는 지정된 DB Listener 포트를 기준으로 **+50 범위의 포트**를 노드 간 내부 연결용으로 사용합니다.

**예시**: DB Listener 포트가 8629인 경우

- DB Listener 포트: 8629/tcp
- 노드 간 내부 연결 포트: 8630/tcp ~ 8679/tcp (총 50개 포트)

따라서 DB Listener 포트를 설정할 때 해당 포트 +50 범위가 모두 비어 있어야 합니다. 해당 포트 대역에 다른 애플리케이션이나 서비스가 사용 중이면 안 되며, 포트 충돌 시 데이터베이스 설치 또는 TAC 구성이 실패할 수 있습니다.
{% endhint %}

### 방화벽 설정

OwlDB 서버와 데이터베이스 서버 간 통신을 위해 방화벽 설정이 필요합니다.

---

## 배포 파일 준비 및 배치

### 1. 필요 파일 목록

- owldb dp 바이너리 (`owldb-dp-installer-*.tar.gz`)
- tibero 바이너리 (`tibero.tar.gz`)
- 라이선스 파일 (`license.xml`)

### 2. 설치 디렉터리 생성

데이터베이스를 설치할 경로(이하 `설치 디렉터리`)를 생성합니다. (예시: `/home/rocky/owldb`)

```bash
mkdir -p {설치 디렉터리}/tibero
chmod 755 {설치 디렉터리}/tibero
```

`{설치 디렉터리}/tibero` 경로가 이후 절차에서 `$TB_HOME`으로 사용됩니다.

### 3. 파일 배치

DP 바이너리를 `$TB_HOME`에 압축 해제하고, tibero 바이너리와 라이선스 파일을 배치합니다.

```bash
# DP 바이너리 압축 해제
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $TB_HOME --strip-components=2

# tibero 바이너리 배치
mv {tibero 바이너리 파일} $TB_HOME/tibero.tar.gz

# 라이선스 파일 배치
mv {라이선스 파일} $TB_HOME/license.xml
```

준비 완료 후 `$TB_HOME` 구조는 다음과 같습니다.

```
$TB_HOME/
 ├── tibero.tar.gz               # tibero 바이너리
 ├── license.xml                 # 라이선스 파일
 ├── install/                    # Tibero 설치 스크립트
 ├── validate_infra.sh           # 인프라 검증 스크립트
 ├── install_pkg.sh              # Tibero 패키지 설치 스크립트
 └── tbagent_dist_latest.tar.gz  # tbagent 바이너리
```

## 인프라 검증 스크립트 실행

설치 DB 환경의 인프라 설정이 올바르게 구성되어 있는지 검증합니다.

```bash
cd $TB_HOME
sh validate_infra.sh --mode DP
```

검증 항목은 다음과 같습니다.

- CPU, Memory 사이즈
- 디스크 사이즈
- Tibero 패키지 설치 여부
- 네트워크 연결 상태
- 방화벽 포트 설정
- 필수 파일 리스트

{% hint style="warning" %}
**주의**

통과하지 못한 항목이 있는 경우, 조치 완료 후 반드시 재검증을 진행합니다. 모든 항목을 통과한 후에 패키지 설치 및 Agent 설치를 진행합니다.
{% endhint %}

## 패키지 설치

인프라 검증을 모두 통과한 후 패키지를 설치합니다.

{% tabs %}
{% tab title="외부 인터넷 연결이 가능한 경우" %}
```bash
cd $TB_HOME
sudo bash install_pkg.sh
```
{% endtab %}
{% tab title="인터넷 연결이 불가한 경우" %}
아래 패키지를 사전에 수동 설치합니다.

- Tibero 패키지: Tibero 패키지 설치 가이드 참고
- OwlDB 패키지: `openssh-server`, `openssh-clients`, `sshpass`, `udev`, `xfsprogs`, `iproute`
{% endtab %}
{% endtabs %}

이후 [데이터베이스 서버 Agent 설치 문서](#ppMhStCgDdusZJ3FQt8y)로 이동하여 진행합니다.
