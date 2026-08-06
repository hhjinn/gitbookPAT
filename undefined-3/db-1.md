OwlDB에서 데이터베이스를 운영하기 위해 데이터베이스를 설치합니다. 데이터베이스 설치가 완료되면 OwlDB에서 제공하는 모든 기능을 사용할 수 있습니다.

**설치 전 아래 사항을 확인합니다.**

- 설치 기능은 관리자 계정만 사용할 수 있습니다.

# 표준 아키텍처

OwlDB는 표준 아키텍처를 기반으로 데이터베이스 신규 설치 기능을 제공합니다. OwlDB On-premise에서 제공하는 표준 아키텍처는 아래와 같습니다.

| 구성 | 설명 |
| --- | --- |
| Single | 단일 노드 구성 |
| TAC (Tibero) | 최대 4노드 클러스터 구성 |
| Single + DR (Tibero) | Single 구성에 Standby 1노드 고정 |
| TAC + DR (Tibero) | TAC 구성에 Standby 1노드 고정 |
| HA + DR (OpenSQL) | HA 구성에 Replica 1노드 고정 |

TAC 및 DR 구성의 경우, 모든 노드는 동일한 스펙으로 구성되며, DR 구성 시 Standby 노드는 1개로 고정됩니다.

{% hint style="info" %}
**참고**

**안내**

Multi-node Cluster를 Standby로 구성할 수 없으며, Standby 노드 개수를 최대 1개까지 지원합니다.
{% endhint %}

{% hint style="warning" %}
**주의**

OwlDB를 통하지 않고 외부에서 직접 데이터베이스 구성을 변경한 경우, OwlDB의 일부 기능이 정상적으로 동작하지 않을 수 있습니다. 구성 변경이 필요한 경우 반드시 OwlDB를 통해 수행해 주세요.
{% endhint %}

# OwlDB 표준 아키텍처

> 📷 **[이미지]** 이미지

---

# **설치 프로세스**

1. **OwlDB 콘솔 화면** > **대시보드**로 이동합니다.
2. 대시보드 상단의 **[설치]** 버튼을 눌러 설치 페이지로 이동합니다.
3. 설치 옵션을 단계별로 입력합니다. 설치 옵션 입력은 총 5단계로 이루어지며, 각 단계에 대한 상세 내용은 아래의 [**설치 옵션 단계** ](#undefined-2)섹션을 참고해 주세요.
4. 입력한 정보를 확인하고 설치 가능 여부 검증이 완료되면, **[설치]** 버튼을 클릭합니다.
5. 설치가 시작되면 대시보드 목록에서 진행 상태를 확인할 수 있습니다. 상태가 **Running**으로 변경되면 설치가 정상적으로 완료된 것입니다.

{% hint style="info" %}
**참고**

데이터베이스 설치 기능은 Root 계정에서만 사용 가능합니다.

아래 경로를 통해서도 데이터베이스 설치 페이지에 접근할 수 있습니다.

- OwlDB 콘솔 화면 > 대시보드 > 카드뷰 > + 아이콘
- GNB > DB Alias 드롭다운 > 데이터베이스 설치 버튼

설치 진행 중 **[취소]** 버튼을 클릭하면 확인 모달이 나타나며, 모달에서 확인을 클릭하면 입력한 정보가 초기화되고 대시보드로 이동합니다.

설치 요청이 접수되면 시스템 알림을 통해 설치 시작, 완료, 실패 여부를 확인할 수 있습니다.
{% endhint %}

{% hint style="warning" %}
**주의**

**[설치]** 버튼을 클릭하면 라이선스 파일 유무 및 코어 수 검증이 함께 진행됩니다.

- 설치 대상 호스트에 라이선스 파일이 없으면 설치가 진행되지 않으며, 라이선스 파일을 배치한 후 다시 시도해야 합니다.
- 요청한 코어 수가 보유 라이선스의 최대 코어 수를 초과하면 설치가 진행되지 않습니다.
- 동시에 많은 설치 요청이 접수되어 처리가 지연되는 경우, 잠시 후 다시 시도해야 합니다.
{% endhint %}

---

# **설치 옵션 단계**

## 엔진 옵션

데이터베이스 이름과 엔진, 토폴로지 정보를 설정하는 단계입니다.

| 항목 | 설명 |
| --- | --- |
| Service Name* | DB 서비스를 식별하기 위한 이름<br>- 6~30자의 영어 대소문자, 숫자, 하이픈(-)만 입력 가능<br>- OwlDB 계정 내에서 중복 생성 불가<br>- 기본값 :`owldb-001`부터 순차 부여 |
| DB Engine Type* | 사용할 데이터베이스 엔진<br>-**Tibero**: 다중화 구성으로 안정적인 서비스 운영 및 DB 확장이 가능한 RDBMS<br>-**OpenSQL** : Open Source 기반 DBMS |
| Topology* | - **Tibero**: Single, TAC<br>-**OpenSQL** : Single, HA |
| Node Count* | - **Tibero**: Single(1, 고정), TAC(2~4 중 선택)<br>-**OpenSQL** : Single, HA(1, 고정) |
| PostgreSQL Version | OpenSQL에서 사용할 PostgreSQL 버전(Tibero는 해당 없음)<br>- 기본값 :**17.9** |

---

## DR 구성

DR 사용 여부와 장애 조치 자동화 레벨을 설정하는 단계입니다.

{% tabs %}
{% tab title="Tibero" %}
| 항목 | 설명 |
| --- | --- |
| Enable DR* | DR 구성 사용 여부(직접 선택 가능) |
| Failover Automation Level* | [자동 장애 조치 단계](#undefined)<br>- 0단계 : 수동<br>- 1단계 : 자동 장애 조치<br>- 2단계 : 자동 구성 복구 (OwlDB v1.3 미지원)<br>- 3단계 : 완전 자동화<br>- Single : 0, 1, 3단계 지원<br>- TAC : 0, 1단계 지원 |
| Standby Count* | Standby DB 개수(표준 아키텍처 기준 최대 1개로 고정) |
| Standby Mode* | Standby Mode 옵션<br>-**Recovery**<br>-**Read Only** |
| Log Replication Type | Primary에서 Standby로의 로그 전송 방식<br>-**LGWR ASYNC**: 트랜잭션이 발생하면 실시간으로 생성되는 Redo log를 전송하는 복제 모드<br>-**ARCH ASYNC** : 로그 스위치 이후, 아카이브 로그 파일이 생성되면 해당 파일을 모아서 전송하는 복제 모드 |
{% endtab %}
{% tab title="Tab" %}
| | |
{% endtab %}
{% tab title="OpenSQL" %}
| |

| 항목 | 설명 |
| --- | --- |
| Enable DR* | Single은 DR 미사용, HA는 DR 사용으로 자동 결정되며 수정 불가 |
| Failover Automation Level* | [자동 장애 조치 단계](#undefined)<br>- 0단계 : 수동<br>- 2단계 : 자동 구성 복구 (OwlDB v1.3 미지원)<br>- 3단계 : 완전 자동화<br>- 0, 3단계 지원(기본값 3단계) |
| Standby Count* | Replica DB 개수(표준 아키텍처 기준 최대 1개로 고정) |
| Log Replication Type | ASYNC 방식으로 고정되어 수정 불가 |
{% endtab %}
{% tab title="Tab" %}
| | |
{% endtab %}
{% endtabs %}

| |

{% hint style="info" %}
**참고**

- Enable DR 항목에서 DR 사용을 선택한 경우 **Failover Automation Level, Standby Count, Standby Mode, Log Replication Type** 항목들이 노출됩니다.
- ARCH ASYNC 모드 사용 시, 아카이브 로그가 생성되는 주기(최대 약 10분)에 따라 Standby로의 동기화가 지연될 수 있습니다.
{% endhint %}

{% hint style="warning" %}
**주의**

Failover Automation Level을 3단계(완전 자동화)로 설정하면 장애 조치 이후 복구 및 리소스 정리까지 모든 과정이 자동으로 처리됩니다. 복구 속도를 최우선으로 처리하므로 장애 발생 시점 기준 최근 일부 데이터가 유실될 수 있습니다.
{% endhint %}

---

## 인스턴스 구성

토폴로지에 따라 노드별 입력 항목이 자동으로 구성됩니다. *표기는 필수 입력 항목을 의미합니다.

{% tabs %}
{% tab title="Tibero" %}
노드 역할은 **Primary/Standby**로 표시됩니다.

### Single

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | 선택한 토폴로지 구성에 맞게,<br>데이터베이스를 설치할 호스트 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우<br>외부 Port 입력 |
| Data Path* | Data Path 입력 | 파일 시스템 경로만 입력 가능 |
| Redo Path* | Redo Path 입력 | 파일 시스템 경로만 입력 가능 |
| Archive Path* | Archive Path 입력 | 파일 시스템 경로만 입력 가능 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능 |

**각 Path**는 파일 시스템 경로만 입력할 수 있습니다. 동일한 경로 또는 서로 다른 경로를 중복 입력하는 것도 허용됩니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

### Single+DR

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | 선택한 토폴로지 구성에 맞게,<br>데이터베이스를 설치할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Primary Destination IP*(Primary 인스턴스만 입력) | StandByDB에서 PrimaryDB로의 통신에서 사용할 Primary Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력<br>Primary 인스턴스만 입력 |
| Primary Destination Port*<br>(Primary 인스턴스만 입력) | StandBy DB에서 Primary DB로 통신에서 사용할 Primary Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| StandBy Destination IP*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로의 통신에서 사용할 StandBy Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력 |
| StandBy Destination Port*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로 통신에서 사용할 StandBy Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Data Path* | Data Path 입력 | 파일 시스템 경로만 입력 가능 |
| Redo Path* | Redo Path 입력 | 파일 시스템 경로만 입력 가능 |
| Archive Path* | Archive Path 입력 | 파일 시스템 경로만 입력 가능 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능 |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | 인스턴스 간 SSH 접속에 사용할 private key path 입력 | 인스턴스 간 SSH 접속 시 동일한 private key를 사용하도록 공통 private key path 입력 |

{% hint style="info" %}
**참고**

DR 구성을 사용하는 경우에는 SSH Key File을 지정된 경로에 미리 배치해야 합니다. 자세한 설정 방법은 [노드 간 SSH 공용키 설정](#4VCx1BdX0fpROq6CwGnH)에서 확인하세요.
{% endhint %}

**모든 Path**는 파일 시스템 경로만 입력할 수 있습니다. 동일한 경로 또는 서로 다른 경로를 중복 입력하는 것도 허용됩니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

### TAC

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | 선택한 토폴로지 구성에 맞게,<br>데이터베이스를 설치할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Interconnect IP* | Cluster내부 노드 간 통신에 사용할 Interconnect IP를 목록에서 선택 | Primary, StandBy 클러스터 둘 다 입력 |
| Data Path* | 데이터 Path 입력 | 로우 디바이스 또는 파티션 경로 입력, 공유 볼륨 사용 |
| Redo Path* | Redo Path 입력 | 로우 디바이스 또는 파티션 경로 입력, 공유 볼륨 사용 |
| Archive Path* | Archive Path 입력 | 로우 디바이스 또는 파티션 경로 입력, 공유 볼륨 사용 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능 |

**Data Path, Redo Path, Archive Path**는 로우 디바이스 경로(`/dev/sdb`) 또는 파티션 경로(`/dev/sdb1`)를 입력합니다. **Backup Path**의 경우 파일 시스템 경로만 입력할 수 있습니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 모든 Path는 **공유 볼륨**으로 구성되어 있어야 합니다.
- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

### TAC+DR

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | 선택한 토폴로지 구성에 맞게,<br>데이터베이스를 설치할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용한다면 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Interconnect IP* | Cluster내부 노드 간 통신에 사용할 Interconnect IP를 목록에서 선택 | Primary, StandBy 클러스터 둘 다 입력 |
| Primary Destination IP*(Primary 인스턴스만 입력) | StandByDB에서 PrimaryDB로의 통신에서 사용할 Primary Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력 |
| Primary Destination Port*<br>(Primary 인스턴스만 입력) | StandBy DB에서 Primary DB로 통신에서 사용할 Primary Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| StandBy Destination IP*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로의 통신에서 사용할 StandBy Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력 |
| StandBy Destination Port*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로 통신에서 사용할 StandBy Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Data Path* | 데이터 Path 입력 | 로우 디바이스 또는 파티션 경로 입력, 공유 볼륨 사용 |
| Redo Path* | Redo Path 입력 | 로우 디바이스 또는 파티션 경로 입력, 공유 볼륨 사용 |
| Archive Path* | Archive Path 입력 | 로우 디바이스 또는 파티션 경로 입력, 공유 볼륨 사용 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능 |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | 인스턴스 간 SSH 접속에 사용할 private key path 입력 | 인스턴스 간 SSH 접속 시 동일한 private key를 사용하도록 공통 private key path 입력 |

{% hint style="info" %}
**참고**

DR 구성을 사용하는 경우에는 SSH Key File을 지정된 경로에 미리 배치해야 합니다. 자세한 설정 방법은 [노드 간 SSH 공용키 설정](#4VCx1BdX0fpROq6CwGnH)에서 확인하세요.
{% endhint %}

**Data Path, Redo Path, Archive Path**는 로우 디바이스 경로(`/dev/sdb`) 또는 파티션 경로(`/dev/sdb1`)를 입력합니다. **Backup Path**의 경우 파일 시스템 경로만 입력할 수 있습니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 모든 Path는 **공유 볼륨**으로 구성되어 있어야 합니다.
- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

{% hint style="info" %}
**참고**

- 선택한 토폴로지에 따라 필요한 노드 수 만큼 위의 입력 항목이 자동으로 구성됩니다.
- 안정적인 운영 환경을 위해, 클러스터 내 모든 인스턴스는 동일한 스펙으로 자동 구성됩니다.
{% endhint %}
{% endtab %}
{% tab title="OpenSQL" %}
노드 역할은 **Leader/Replica**로 표시됩니다.

### Single

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | 선택한 토폴로지 구성에 맞게,<br>데이터베이스를 설치할 호스트 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우<br>외부 Port 입력 |
| Data Path* | Data Path 입력 | 파일 시스템 경로만 입력 가능 |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | 인스턴스 간 SSH 접속에 사용할 private key path 입력 | 인스턴스 간 SSH 접속 시 동일한 private key를 사용하도록 공통 private key path 입력 |

**각 Path**는 파일 시스템 경로만 입력할 수 있습니다. 동일한 경로 또는 서로 다른 경로를 중복 입력하는 것도 허용됩니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

### HA

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | 선택한 토폴로지 구성에 맞게,<br>데이터베이스를 설치할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Replication Connection IP* | 복제 연결에 사용할 IP을 입력 또는 선택 | - |
| 네트워크 인터페이스* | 네트워크 인터페이스를 선택 | - |
| Data Path* | Data Path 입력 | 파일 시스템 경로만 입력 가능 |
| SSH Port* | SSH port | - |
| SSH User* | SSH User | - |
| SSH Key File Path* | 인스턴스 간 SSH 접속에 사용할 private key path 입력 | 인스턴스 간 SSH 접속 시 동일한 private key를 사용하도록 공통 private key path 입력 |

{% hint style="info" %}
**참고**

DR 구성을 사용하는 경우에는 SSH Key File을 지정된 경로에 미리 배치해야 합니다. 자세한 설정 방법은 [노드 간 SSH 공용키 설정](#4VCx1BdX0fpROq6CwGnH)에서 확인하세요.
{% endhint %}

**모든 Path**는 파일 시스템 경로만 입력할 수 있습니다. 동일한 경로 또는 서로 다른 경로를 중복 입력하는 것도 허용됩니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.
{% endtab %}
{% endtabs %}

---

## 데이터베이스 구성

데이터베이스 구성 정보를 입력하는 단계입니다.

| 항목 | 설명 |
| --- | --- |
| Database Name* | 사용할 데이터베이스의 이름 |
| SYS User Password* | 데이터베이스 최고 권한 관리자 계정(SYS)의 비밀번호 |
| Target Memory Size* | 대상 메모리 사이즈 |
| Shared Memory Size* | 공유 메모리 사이즈 |
| Character Set* | 데이터베이스에 사용할 문자 인코딩 |
| Timezone* | 데이터베이스가 설치될 OS 시간대 |
| VIP* | VIP 사용 여부 선택 |
| Primary Node #N Vip | 데이터베이스 가상 IP<br>(VIP 사용 선택시 활성화) |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 |
| Redo Log File Size (MB) | Redo 로그 파일 크기 |
| System Data File Size (MB) | 시스템 테이블 및 주요 메타 데이터를 저장할 데이터 파일의 크기 |
| Syssub Data File Size (MB) | 시스템 운영 관련 데이터 저장을 위한 서브 데이터 파일 크기 |
| User Tablespace Data File Size (MB) | 사용자 데이터를 저장할 테이블 스페이스 데이터 파일 크기 |
| Temporary Tablespace Data File Size (MB) | 대용량 연산에 사용되는 임시 테이블스페이스 데이터 파일 크기 |
| Undo Tablespace Data File Size (MB) | Undo 테이블스페이스 크기 |

| | |

| |

| 항목 | 설명 |
| --- | --- |
| Database Name* | 사용할 데이터베이스의 이름 |
| User Id* | 데이터베이스 최고 권한 관리자 계정 ID |
| User Password* | 데이터베이스 최고 권한 관리자 계정의 비밀번호 |
| Character Set* | 데이터베이스에 사용할 문자 인코딩 |
| Timezone* | 데이터베이스가 설치될 OS 시간대 |
| VIP* | 데이터베이스 가상 IP |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 |
| Shared Buffers | 공유 메모리 크기 (수정 불가) |
| WAL File Size (MB) | WAL 파일 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| Connection Pooler Port | OpenSQL에서 커넥션 풀이 클라이언트 연결을 수신하는 포트<br>- 기본값 : 6432<br>- 입력 범위 : 1024~65535 |
| Extension | OpenSQL 데이터베이스 생성 시 함께 설치할 Extension 선택(다중 선택 가능) |

{% hint style="info" %}
**참고**

- Connection Pooler Port, Extension 항목은 OpenSQL 엔진 선택 시에만 노출됩니다.
- Extension 설치에 실패해도 데이터베이스 생성에는 영향을 주지 않으며, 설치에 실패한 Extension은 시스템 알림에서 확인할 수 있습니다.
{% endhint %}

---

## 구성 정보 확인

앞선 단계에서 입력한 모든 옵션들에 대하여 검증이 완료된 후, 이 단계로 진입할 수 있습니다.

입력한 구성 정보를 한눈에 확인하고, 이상이 없다면 **[설치]** 버튼을 클릭하여 설치를 시작합니다. 내용을 수정하려면 **[이전]** 버튼을 클릭해 주세요.

{% hint style="info" %}
**참고**

[4. 데이터베이스 구성] 단계에서 **[다음]** 버튼을 클릭하면 입력한 정보에 대한 유효성 검사가 자동으로 수행되며, 검사를 통과해야 **[5. 구성 정보 확인]** 단계로 진입할 수 있습니다.

**[Tibero]** 아래 항목을 추가로 확인합니다.

- 입력한 Data/Redo/Archive/Backup Path가 설치 대상 호스트에 실제로 존재하는지 여부
- 노드 간 SSH 연결 가능 여부
- 모든 노드의 OS Timezone 일치 여부
- 입력한 Data File 크기 합계가 스토리지 여유 공간을 초과하지 않는지 여부

**[OpenSQL]** 아래 항목을 추가로 확인합니다.

- 입력한 Data Path가 설치 대상 호스트에 실제로 존재하는지 여부
- 노드 간 SSH 연결 가능 여부
- SSH Key File Path가 실제로 존재하는지 여부 및 Private key 인지 여부
- 모든 노드의 OS Timezone 일치 여부
- 입력한 Data File 크기 합계가 스토리지 여유 공간을 초과하지 않는지 여부

검사에 실패하면 오류가 발생한 항목에 에러 메시지가 표시되며, 다음 단계로 이동할 수 없습니다.
{% endhint %}

---

# 설치 진행 상태

대시보드에서 데이터베이스의 설치 진행 상황을 실시간으로 확인할 수 있습니다. 설치는 아래 표와 같이 주요 단계와 세부 단계로 나뉘어 진행되며, 선택한 토폴로지에 따라 실제 수행되는 단계가 다를 수 있습니다.

{% tabs %}
{% tab title="Tibero" %}
| 주요 단계 | 세부 단계 |
| --- | --- |
| 설치 필수 조건 검증 | 1. sudo 권한 검증<br>2. 필수 파일 검증<br>3. parameter config 검증 |
| 인프라 설정 | 1. Kernel 환경 설정<br>2. 필수 package 설치<br>3. Disk udev rule 설정<br>4. Volume mount<br>5. Disk attach (instance) |
| Tibero 설정 | 1. Tibero instance Tip file & DSN file 생성<br>2. cm 순차설치로 인한 volume 설정 변경<br>3. cm 순차설치로 인한 volume 대기<br>4. Tip covert(primary <→ standby)<br>5. Standby 설치 완료 tag 설정 변경<br>6. Standby 설치 완료 tag 대기<br>7. RMGR 백업 및 전송<br>8. RMGR 백업 대기 |
| DB 설치 | 1. CM gen(resource 등록 및 실행)<br>2. DB create(TAS도 포함됨 토폴로지에 따라)<br>3. Disk snapshot 생성<br>4. wait Disk snapshot in Standby node<br>5. CM Fence on(reboot)<br>6. cm complete(service up)<br>7. wait cm service<br>8. recovery RMGR |
| Tibero 상태 확인 | 1. Tb probe |
{% endtab %}
{% tab title="Tab" %}
| |
{% endtab %}
{% tab title="OpenSQL" %}
입력해 주세요
{% endtab %}
{% endtabs %}

| 주요 단계 | 세부 단계 |
| --- | --- |
| 인프라 설정 |   |

1. Kernel 환경 설정
2. 필수 package 설치
3. PgAgent 설치
4. mount volume
5. 데이터 디렉터리 준비 | | OpenSQL 설정 |
6. 모듈 설정 | | OpenSQL 설치 |
7. 부트스트랩 이후 설정 | | OpenSQL 상태체크 |
8. 초기화 후 설정 적용 | | PGAgent 설치 |
9. PgAgent 설정 | |

| |
