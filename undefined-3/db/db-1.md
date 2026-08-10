이미 운영 중인 데이터베이스를 OwlDB에 등록하여 관리할 수 있습니다. 표준 아키텍처 기반으로 구축된 데이터베이스뿐만 아니라, 고객이 직접 구성한 비표준 아키텍처의 데이터베이스도 등록을 지원합니다.

등록이 완료되면 OwlDB에서 제공하는 모니터링, 파라미터 관리 등의 기능을 사용할 수 있습니다.

**등록 전 아래 사항을 확인합니다.**

- 등록 기능은 관리자 계정만 사용할 수 있습니다.
- 등록할 DB 노드의 부트모드는 `Normal`, `Recovery`, `Readonly` 여야 합니다. 이외의 부트모드로 기동된 노드는 비정상 노드로 처리됩니다.
- Standby 멀티 노드 구성은 등록을 지원하지 않습니다.

# 등록

## 등록 절차

1. OwlDB 콘솔 화면 > 대시보드 > **[탐색]** 버튼을 클릭합니다.
2. 탐색 결과에서 **등록 가능한 DB** 목록을 확인하고, 해당 데이터베이스의 **[등록]** 버튼을 클릭하여 등록 페이지로 이동합니다.
3. 등록 옵션을 단계별로 입력합니다. 등록 옵션 입력은 총 5단계로 이루어지며, 각 단계에 대한 상세 내용은 아래 '**등록 옵션**'을 참고해 주세요.
4. 입력한 정보를 확인하고 **[등록]** 버튼을 클릭합니다.
5. 대시보드 목록에서 해당 데이터베이스의 상태가 **Running**으로 표시되면 등록이 정상적으로 완료된 것입니다.

**[등록]** 버튼을 클릭하면 라이선스 조건을 검증한 후 등록 요청이 시작되며, 대시보드로 이동합니다. 등록 시작, 완료, 실패 여부는 시스템 알림으로 확인할 수 있습니다.

![탐색 결과 화면에서 등록 가능한 데이터베이스 목록과 등록 버튼](탐색 결과 목록 및 등록 버튼 화면)

{% hint style="warning" %}
**주의**

등록 진행 중 아래와 같은 경우 등록이 실패할 수 있습니다.

- 대상 데이터베이스와의 연결에 실패한 경우
- 등록 진행 중 데이터베이스 상태가 변경된 경우
- Agent와의 연결에 실패한 경우
- 대상 노드에 라이선스 파일이 없거나 라이선스 코어 수가 부족한 경우

등록이 실패하면 오류 메시지가 표시되며 대시보드로 이동하지 않고 처리가 종료됩니다.
{% endhint %}

---

## 등록 옵션

등록 페이지로 이동하면 탐색을 통해 수집된 데이터베이스 정보가 자동으로 입력되어 있습니다. 자동으로 입력된 항목은 변경할 수 없으며, 수집할 수 없는 일부 항목만 입력이 필요합니다.

### 엔진 옵션

데이터베이스 이름과 엔진, 토폴로지 정보를 설정하는 단계입니다.

| 항목 | 설명 |
| --- | --- |
| Service Name* | 데이터베이스 서비스를 식별하기 위한 이름<br>- 6~30자의 영문 대소문자(a-z, A-Z), 숫자(0-9), 하이픈(-)만 사용 가능<br>- 미입력 시`owldb-001`과 같은 형태로 자동 생성 |
| Database Engine Type | 사용할 데이터베이스 엔진<br>-**Tibero**: 다중화 구성으로 안정적인 서비스 운영 및 DB 확장이 가능한 RDBMS<br>-**OpenSQL** : Open Source 기반 고객 맞춤형 DBMS 기술 플랫폼 |
| Topology | 데이터베이스 구조를 결정할 토폴로지 유형<br>-**Tibero**: Single, TAC<br>-**OpenSQL** : Single, HA |
| Node Count | 클러스터 구성 노드 수<br>-**Tibero**Single : 1 / TAC : 2~~8~~<br>~~-~~~~~~~~~~~~~~~~~~~~~~~~~~~~**OpenSQL**~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Single : 1 / HA : 2~~3 |
| PostgreSQL Version | OpenSQL 선택 시 노출되는 PostgreSQL 버전<br>현재 단일 버전만 제공 |

{% hint style="info" %}
**참고**

OpenSQL의 HA는 2node 또는 3node로 탐색될 수 있습니다.

- 2node HA : Leader 1대 + Replica 1대 + Quorum Node(etcd) 1대로 구성
- 3node HA : Leader 1대 + Replica 2대로 구성
{% endhint %}

### DR 구성

DR 사용 여부와 장애 조치 자동화 레벨을 설정하는 단계입니다.

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Enable DR | DR 구성 사용 여부 | - |
| Failover Automation Level* | 자동 장애 조치 단계<br>-**0단계 : 수동**<br>-**1단계 : 자동 장애 조치**<br>-**2단계 : 자동 구성 복구 (On-Premise 미지원)**<br>-**3단계 : 완전 자동화** | - Tibero Single : 0, 1, 3단계 지원<br>- Tibero TAC : 0, 1단계 지원<br>- OpenSQL : 0, 3단계 지원 |
| {Standby/Replica} Count | Standby/Replica DB 개수 | - |
| Standby Mode | Standby Mode 옵션<br>-**Recovery**<br>-**Read Only** | Tibero에서만 입력 |
| Log Replication Type | Primary(Leader)에서 Standby(Replica)로의 로그 전송 방식<br>-**LGWR ASYNC**(Tibero) : 트랜잭션이 발생하면 실시간으로 생성되는 Redo log를 전송하는 복제 모드<br>-**ARCH ASYNC**(Tibero) : 로그 스위치 이후, 아카이브 로그 파일이 생성되면 해당 파일을 모아서 전송하는 복제 모드<br>-**ASYNC**(OpenSQL) : 복제 연결을 통해 데이터를 비동기로 전송하는 복제 모드 | - |

{% hint style="info" %}
**참고**

- Enable DR 항목에서 DR 사용을 선택한 경우 **Failover Automation Level, {Standby/Replica} Count, Standby Mode, Log Replication Type** 항목이 노출됩니다.
- OpenSQL을 HA 구성으로 탐색한 경우 Enable DR 항목은 자동으로 **DR 사용**으로 표시되며 변경할 수 없습니다. Topology를 변경하려면 이전 단계로 이동해 주세요.
{% endhint %}

### 인스턴스 구성

토폴로지에 따라 노드별 입력 항목이 자동으로 구성됩니다. 각 노드 영역은 아코디언 형태로 펼치거나 접을 수 있습니다. 두 번째 노드부터는 **이전과 동일** 체크박스를 선택하면 이전 노드에 입력한 Port와 Backup Path 값을 그대로 적용할 수 있습니다.

{% tabs %}
{% tab title="Tibero" %}
**Single**

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | DB 노드에 매핑할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우<br>외부 Port 입력 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능 |

**Path**는 파일 시스템 경로만 입력할 수 있습니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 경로가 등록 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

**Single+DR**

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | DB 노드에 매핑할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Primary Destination IP*(Primary 인스턴스만 입력) | StandByDB에서 PrimaryDB로의 통신에서 사용할 Primary Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력<br>Primary 인스턴스만 입력 |
| Primary Destination Port*<br>(Primary 인스턴스만 입력) | StandBy DB에서 Primary DB로 통신에서 사용할 Primary Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| StandBy Destination IP*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로의 통신에서 사용할 StandBy Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력 |
| StandBy Destination Port*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로 통신에서 사용할 StandBy Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능 |

**Path**는 파일 시스템 경로만 입력할 수 있습니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 경로가 등록 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

**TAC**

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | DB 노드에 매핑할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능,<br>공유 볼륨 사용 |

**Backup Path**의 경우 파일 시스템 경로만 입력할 수 있습니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 모든 Path는 **공유 볼륨**으로 구성되어 있어야 합니다.
- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.

---

**TAC+DR**

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | DB 노드에 매핑할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용한다면 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Primary Destination IP*(Primary 인스턴스만 입력) | StandByDB에서 PrimaryDB로의 통신에서 사용할 Primary Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력 |
| Primary Destination Port*<br>(Primary 인스턴스만 입력) | StandBy DB에서 Primary DB로 통신에서 사용할 Primary Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| StandBy Destination IP*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로의 통신에서 사용할 StandBy Destination IP를 직접 입력 | NAT를 사용하는 경우 NAT IP를 입력 |
| StandBy Destination Port*<br>(StandBy 인스턴스만 입력) | Primary DB에서 StandBy DB로 통신에서 사용할 StandBy Destination Port를 직접 입력 | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Backup Path* | Backup Path 입력 | 파일 시스템 경로만 입력 가능,<br>공유 볼륨 사용 |

**Backup Path**의 경우 파일 시스템 경로만 입력할 수 있습니다.

단, 다음 조건을 반드시 충족해야 합니다.

- 입력한 모든 Path는 **공유 볼륨**으로 구성되어 있어야 합니다.
- 입력한 경로가 설치 대상 호스트에 **실제로 존재**해야 합니다.
- 파일 시스템 경로에 대해 DP Agent 실행 계정이 **읽기, 쓰기, 실행 권한**을 보유해야 합니다.
{% endtab %}
{% tab title="OpenSQL" %}
노드 섹션명은 **Leader Node**, **Replica Node #{n}**으로 표시되며, 2node HA로 탐색된 경우 **Quorum Node**가 함께 표시됩니다.

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Hostname* | DB 노드에 매핑할 호스트를 선택 | - |
| Service IP* | OwlDB와 노드간 통신에 사용할 IP를 직접 입력 또는 선택 | NAT를 사용하는 경우 NAT IP를 입력 |
| Service Port* | OwlDB와 데이터베이스 서버간 통신에 사용할 Port를 직접 입력<br>기본값 :`5432` | 포트 포워딩을 사용하는 경우 외부 Port를 입력 |
| Replication Connection IP* | HA 구성일 때, 복제 연결에 사용할 IP를 직접 입력 또는 선택 | HA 구성에서만 노출 |
{% endtab %}
{% tab title="Tab" %}
| | | |
{% endtab %}
{% endtabs %}

| | |

### 데이터베이스 구성

데이터베이스 구성 정보를 확인하고, 수집할 수 없는 일부 항목을 직접 입력하는 단계입니다. 대부분의 항목은 탐색 결과가 자동으로 입력되며 수정할 수 없습니다.

{% tabs %}
{% tab title="Tibero" %}
| 항목 | 설명 |
| --- | --- |
| Database Name | 사용할 데이터베이스의 이름 |
| SYS User Password* | 데이터베이스 최고 권한 관리자 계정(SYS)의 비밀번호 |
| Character Set | 데이터베이스에 사용할 문자 인코딩 |
| Timezone | 데이터베이스가 설치될 OS 시간대 |
| VIP | 데이터베이스 가상 IP<br>VIP를 사용하지 않는 경우`-`로 표시 |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 (수정 불가) |
| Target Memory Size | 대상 메모리 크기 (수정 불가) |
| Shared Memory Size | 공유 메모리 크기 (수정 불가) |
| Redo Log File Size (MB) | Redo 로그 파일 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| System Data File Size (MB) | 시스템 테이블 및 주요 메타 데이터를 저장할 데이터 파일의 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| Syssub Data File Size (MB) | 시스템 운영 관련 데이터 저장을 위한 서브 데이터 파일 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| User Tablespace Data File Size (MB) | 사용자 데이터를 저장할 테이블 스페이스 데이터 파일 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| Temporary Tablespace Data File Size (MB) | 대용량 연산에 사용되는 임시 테이블스페이스 데이터 파일 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| Undo Tablespace Data File Size (MB) | Undo 테이블스페이스 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
{% endtab %}
{% tab title="Tab" %}
| | |
{% endtab %}
{% tab title="OpenSQL" %}
| |

| 항목 | 설명 |
| --- | --- |
| Database Name | 사용할 데이터베이스의 이름 |
| User Id |   |
| User Password* | 데이터베이스 최고 권한 관리자 계정의 비밀번호 |
| Character Set | 데이터베이스에 사용할 문자 인코딩 |
| Timezone | 데이터베이스가 설치될 OS 시간대 |
| VIP | 데이터베이스 가상 IP<br>VIP를 사용하지 않는 경우`-`로 표시 |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 (수정 불가) |
| Target Memory Size | 대상 메모리 크기 (수정 불가) |
| Shared Buffers | 공유 메모리 크기 (수정 불가) |
| WAL File Size (MB) | WAL 파일 크기<br>탐색 과정에서 값을 확인할 수 없어 빈 값으로 표시되며 수정할 수 없음 |
| Connection Pooler Port* | 커넥션 풀이 클라이언트 연결을 수신하는 포트<br>범위 : 1024~65535 |
{% endtab %}
{% tab title="Tab" %}
| | |
{% endtab %}
{% endtabs %}

| |

### 구성 정보 확인

앞선 단계에서 입력한 모든 옵션이 정상적으로 입력되었는지 검증이 완료된 후 이 단계로 진입할 수 있습니다.

입력한 구성 정보를 한눈에 확인하고, 이상이 없다면 **등록** 버튼을 클릭하여 등록을 시작합니다. 내용을 수정하려면 **이전** 버튼을 클릭해 주세요.

화면 우측의 요약 영역에서 단계별로 입력한 정보를 확장하거나 축소하여 확인할 수 있으며, 값을 입력하지 않은 필수 항목은 `[미입력]`으로 표시됩니다.

****표기는 필수 입력 항목을 의미합니다. 이외의 항목들은 탐색을 통해 수집된 정보가 자동으로 입력되어 있습니다.***

{% hint style="warning" %}
**주의**

인스턴스 구성 및 데이터베이스 구성 단계에서 **다음** 버튼을 클릭하면 아래 항목에 대한 유효성 검사가 수행되며, 실패 시 등록을 진행할 수 없습니다.

- 노드 간 OS Timezone이 다른 경우 (Tibero) : 각 노드에 접속하여 Timezone 설정을 통일해야 합니다.
- SYS 계정 로그인에 실패한 경우 (Tibero) : 입력한 비밀번호를 확인해야 합니다.
- 입력한 Backup Path가 호스트에 존재하지 않는 경우
{% endhint %}

---

# 등록 해제

등록된 데이터베이스는 삭제가 아닌 등록 해제 방식으로 OwlDB 관리 대상에서 제외할 수 있습니다. 등록 해제 후에도 실제 데이터베이스는 운영 환경에 그대로 유지되며, 필요 시 재등록이 가능합니다.

등록 해제는 다음 경로에서 진행할 수 있습니다.

- **Overview > 작업 > 등록 해제** 클릭
- **대시보드 > DB 서비스 선택 > 등록 해제** 클릭

**등록 해제** 버튼을 클릭하면 확인 모달이 표시되며, DB Service Name를 입력하고 **확인** 버튼을 클릭하면 등록 해제가 완료됩니다.
