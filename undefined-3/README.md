# 대시보드

운영 중인 전체 데이터베이스와 인스턴스의 상태에 대한 요약 정보와 알림을 제공합니다. **OwlDB 콘솔 화면 > OwlDB 로고**를 클릭하여 대시보드를 확인할 수 있습니다.

> 📷 **\[이미지\]** 이미지

# 전체 상태 요약 정보 확인

운영 중인 전체 데이터베이스/인스턴스에 대해 최상위 상태 값(**Status**)과 세부 상태 값(**Health**)을 확인합니다. 각 상태를 클릭하면 '[데이터베이스/인스턴스 목록 확인](#%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%EB%AA%A9%EB%A1%9D-%ED%99%95%EC%9D%B8)'에서 해당 상태 값을 가진 데이터베이스/인스턴스 목록을 확인할 수 있습니다.

{% tabs %}
{% tab title="Status" %}
최상위 상태인 Status는 데이터베이스 단위로 표시합니다.
* 인스턴스 노드 별로 상태가 변경될 수 있기 때문에 상태 옆에 개수를 (n/m) 형태로 표시합니다. n: 사용할 수 있는 인스턴스 노드 개수 m: 전체 인스턴스 노드 개수
* **Bold체 **\* 는 대시보드에서 필터링 가능한 타입입니다.

| 타입  | 설명  |
|-----|-----|
| Provisioning | 신규 리소스를 생성 중인 상태입니다. (Cloud 환경에서 DB 서비스 생성 시에 해당합니다.) |
| **Running** \* | 데이터베이스를 정상적으로 사용할 수 있는 상태입니다. |
| **Updating** \* | 데이터베이스에 계획된 작업이나 변경 사항을 적용 중인 상태입니다. (예: 전체 재시작, 역할 전환, 스펙 변경, 마이그레이션, 복구 등) |
| **Degraded** \* | 일부 데이터베이스 혹은 구성요소 사용이 불가능한 상태입니다. (예: 개별 인스턴스 재시작 등) |
| **Failover** \* | 시스템이 데이터베이스 장애를 감지하여 Auto Failover를 수행 중인 상태입니다. (Primary DB 클러스터 전체가 Unavailable 상태가 되었을 때 발생합니다.) |
| **Down** \* | 전체 데이터베이스 사용이 불가능한 상태입니다. (Primary와 Standby 모두 사용 불가 상태입니다.) |
| Starting | Stopped 상태에서 Running으로 전환 중인 상태입니다. |
| Stopping | Running 상태에서 Stopped로 전환 중인 상태입니다. (Cloud 환경에서는 트랜잭션 롤백 및 프로세스 정지 후 VM 중지 전환을, On-Premise 환경에서는 DB 전체 종료를 수행합니다.) |
| Stopped | 모든 리소스를 일시적으로 사용하지 않는 상태입니다. (리소스가 비활성화됩니다.) |
| Terminating | 모든 리소스와 데이터를 영구적으로 삭제 중인 상태입니다. (완료 시, 접근 및 복구가 불가능합니다.) |
|     |     |
|
{% endtab %}
|     |
|     |     |
|     |     |
|
{% tab title="Health" %}
|     |
|     |     |
| 세부 상태인 Health는 인스턴스 노드 별로 표시합니다. |     |

* DB Boot mode, Instance, 클러스터 관리 도구(이하 CMT), Agent 연결 상태를 조합하여 표시합니다.
* 메시지에 따라 제한되는 기능은 각 메뉴 진입 시 배너로 확인할 수 있습니다.

### Tibero Health

Tibero 인스턴스 노드의 Health는 DB Boot Mode, 클러스터 관리 도구(CMT), Agent 상태를 조합하여 판정합니다.

| 표시  | 상태  | 메시지 | 설명  |
|-----|-----|-----|-----|
| 🟢 Available | normal | -   | 노드 정상 상태입니다. |
| 🟡 Limited | normal | Issue: CMT Inactive | 클러스터 관리 도구(CMT)가 비정상 상태입니다. |
|     | normal | Issue: Mount Mode | 노드가 Mount 모드로 기동된 상태입니다. |
| 🔴 Unavailable | normal | Issue: Nomount Mode | 노드가 Nomount 모드로 기동된 상태입니다. |
|     | normal | Issue: DB Down | 노드의 DB가 Down된 상태입니다. |
|     | normal | Issue: Agent Disconnect | 노드가 위치한 VM과 Agent 연결이 끊어진 상태입니다. |
|     | normal | Issue: VM Down | 노드가 위치한 VM이 Down된 상태입니다. |
| ⚫ Retired | normal | Issue: Failovered Primary | Failover 수행 과정에서 (구)Primary가 비정상 종료된 상태입니다. |

### OpenSQL Health

OpenSQL 인스턴스 노드의 Health는 Patroni, OpenProxy, etcd, Agent 상태를 조합하여 판정합니다.

| 표시  | 상태  | 메시지 | 설명  |
|-----|-----|-----|-----|
| 🟢 Available | normal | -   | 노드 정상 상태입니다. |
| 🟡 Limited | normal | Issue: etcd Inactive | etcd가 비정상 상태입니다. |
|     | normal | Issue: OpenProxy Inactive | OpenProxy가 비정상 상태입니다. |
| 🔴 Unavailable | normal | Issue: DB Down | Patroni가 비정상 상태로 DB가 Down된 상태입니다. |
|     | normal | Issue: Agent Disconnect | 노드가 위치한 VM과 Agent 연결이 끊어진 상태입니다. |

`🔵 In Progress` 상태는 Tibero와 OpenSQL 공통으로 적용됩니다.

| 준위  | 메시지 |
|-----|-----|
| 개별 인스턴스 노드 | Reboot, Switchover, Failover, Rebuilding, Modify Spec (TAC Scale In/Out) |
| 전체 인스턴스 | Modify Spec (Scale Up/Down), Migration, Restoring, Patch/Upgrade, Starting, Stopping |

{% hint style="info" %}
**참고**\
클러스터 관리 도구란, 클러스터 구성의 데이터베이스를 운영하기 위해 필요한 구성 요소입니다. (예: Tibero Cluster Manager(CM), Tibero Active Storage(TAS) 등)
{% endhint %}
 {% endtab %}
{% endtabs %}

# 데이터베이스/인스턴스 목록 확인

전체 데이터베이스와 하위 인스턴스 목록을 확인합니다. '[전체 상태 요약 정보 확인](#%EC%A0%84%EC%B2%B4-%EC%83%81%ED%83%9C-%EC%9A%94%EC%95%BD-%EC%A0%95%EB%B3%B4-%ED%99%95%EC%9D%B8)'에서 선택한 상태값에 따라 보여지는 데이터베이스 및 인스턴스 목록이 달라집니다.

* 선택한 데이터베이스의 하위 인스턴스에 대해 재시작 작업을 수행할 수 있습니다.
* 선택한 데이터베이스에 대해 시작, 중지, 삭제, 역할 전환, 라이선스 갱신 관리 작업을 수행할 수 있습니다.
* 데이터베이스/인스턴스 별칭 검색 기능을 제공합니다. 
{% hint style="info" %}
**참고**\
DB 서비스에 대한 다양한 관리 작업을 수행할 수 있습니다. 자세한 내용은 작업 페이지에서 확인 할 수 있습니다.
{% endhint %}

### 목록 뷰 전환 (카드뷰/리스트 뷰)

데이터베이스/인스턴스 목록을 카드뷰와 리스트뷰 두 가지 방식으로 확인할 수 있습니다. 뷰 전환 시 정렬 기능은 초기화되지만, 필터링은 유지됩니다.

{% tabs %}
{% tab title="카드뷰(Card View)" %}
카드뷰는 데이터베이스의 주요 정보를 시각적으로 파악하기 쉽도록 개별적인 카드 형태로 구성하여 보여줍니다. 한 줄에 최대 4개의 카드가 표시되며, 각 카드의 크기는 고정됩니다. 콘텐츠 내용이 고정된 크기를 초과하는 경우 내부 스크롤이 활성화됩니다. 현재 운영 중인 DB 서비스 카드 목록의 마지막 칸에는 `➕` 아이콘이 표시되며, 클릭 시 DB 서비스 생성 페이지로 이동합니다.
| 항목  | 설명  |
|-----|-----|
| DB 서비스 정보 | 데이터베이스의 주요 정보를 카드 상단에 요약하여 표시합니다.<br>표시 항목<br>• DB Type Logo<br>• DB Service Name<br>• Status<br>• DB Type<br>• DB version (OpenSQL의 경우, OpenSQL 버전과 PostgreSQL 버전을 동시에 표시합니다.)<br>• Topology<br>• Eventlog : I / W / E (각각 Info / Warning / Error를 의미합니다.) |
| 노드 정보 | 데이터베이스 하위에 연계된 인스턴스 정보를 표시합니다. 역할(Role)을 기준으로 아코디언 형태로 축소/확장할 수 있습니다.<br>정렬 규칙<br>• Tibero: Primary → Standby(Read Only) → Standby(Recovery) 순으로 표시합니다.<br>• OpenSQL: Leader → Replica 순으로 표시합니다.<br>• 동일 Role 내에서 AZ 기준으로 정렬합니다.<br>표시 항목<br>• 역할(Role) : 해당 역할의 인스턴스 개수를 표시합니다.<br>• Data volume (Tibero) / Volume (OpenSQL): 사용량(%)과 바 차트, Threshold를 표시합니다.<br>테이블 항목<br>• 인스턴스 별칭<br>• Health<br>• vCPU: 현재 CPU 사용량(%)<br>• Memory: 현재 메모리 사용량(%)<br>• 활성 세션: 현재 활성 세션 개수 / Max Session Count (Standby(Recovery)는 미표시됩니다.)<br>• 가용 영역: 인스턴스가 위치한 AZ 정보 |
| 클러스터 정보 | Connection Health를 클러스터 다이어그램으로 표시합니다.<br>• Single: Primary/Leader DB 정보만 표시합니다.<br>• Single +DR / HA: Primary/Leader DB와 Standby/Replica DB 정보를 표시합니다. P-S 연결 상태 모니터링 정보를 표시하며, Standby/Replica DB는 AZ별로 각각 생성합니다.<br>• Tibero TAC: Primary DB Box 안에 다중 노드를 표시합니다.<br>• Tibero TAC + DR: Primary DB와 Standby DB 정보를 표시합니다. P-S 연결 상태 모니터링 정보를 표시하며, Primary DB Box 안에 다중 노드를 표시하고 Standby DB는 AZ별로 각각 생성합니다.<br>• DR 구성 연결 상태 모니터링: Primary - Standby / Leader - Replica 간 연결 상태를 선의 색상과 표시 정보로 나타냅니다.<br>• 정상: 초록색,`Connected`<br>• 실패: 붉은색,`Disconnected` |
|     |     |
|
{% endtab %}
|     |
|     |     |
|     |     |
|
{% tab title="리스트뷰(List View)" %}
|     |
|     |     |
| 리스트뷰는 데이터베이스 및 하위 인스턴스 정보를 계층 구조로 파악하기 쉽도록 트리 형태의 표로 표시합니다. |     |

{% hint style="info" %}
**참고**\
'기본값'은 초기 화면에서 기본적으로 노출되는 항목을, '필수값'은 숨김 설정이 불가능한 항목을 의미합니다.
{% endhint %}

| 컬럼  | 설명  | DB 서비스 준위 | 인스턴스 준위 | 기본값 | 필수값 |
|-----|-----|-----------|---------|-----|-----|
| **이름** | 이름을 표시합니다.<br>• DB 서비스 이름 클릭 시 '[서비스 메타 정보 조회](/doc/5d7daeb2-e4f4-4409-aa74-dcaf1d65ab46)' 페이지로 이동합니다.<br>• 인스턴스 별칭 클릭 시 '[인스턴스 관리](/doc/dF57s45IXBUgU7RX1UvL)' 페이지로 이동합니다. | DB 서비스 이름 | 인스턴스 별칭 | O   | O   |
| **생성일** | DB 서비스 생성일을 `yyyy.mm.dd HH:mm:ss` 형식으로 표시합니다. | DB 서비스 생성일 | X       | X   | X   |
| **상태** | Status 또는 Health를 표시합니다. | • Creating<br>• Running(n/m)<br>• Updating(n/m)<br>• Degraded(n/m)<br>• Down(n/m)<br>• Starting<br>• Stopping<br>• Stopped<br>• Terminating<br>• Deploying (on-premise)<br>• Registering (on-premise)<br>• Unregistering (on-premise) | • 🟢 (available)<br>• 🟡 (limited)<br>• 🔵 (in progress)<br>• 🔴 (unavailable) | O   | O   |
| **유형** | 데이터베이스 엔진 유형을 표시합니다. | • Tibero<br>• OpenSQL | -       | O   | X   |
| **구성** | 토폴로지 또는 역할을 표시합니다. | • Tibero: Single, TAC(+DR)<br>• OpenSQL: Single, HA | • Tibero: Primary, Standby(Read Only), Standby(Recovery)<br>• OpenSQL: Leader, Replica | O   | O   |
| **가용 영역 (Cloud)** | Cloud 환경인 경우, 해당 인스턴스가 위치한 가용 영역(AZ) 정보를 표시합니다. | -         | 가용 영역(AZ) 정보 | O   | X   |
| **vCPU** | CPU 수와 사용량을 바 차트로 표시합니다. | O         | O       | O   |     |
| **Memory** | Memory와 사용량을 바 차트로 표시 | O         | O       | O   | X   |
| **활성세션** | 현재 활성화된 세션의 개수를 바 차트로 표시\* Primary / Standby(RO) / Leader / Replica 에 한하여 표시<br>\* Standby(Recovery) :`-`으로 표시 | O         | O       | O   | X   |
| **Data Volume (Tibero)** | data volume 사용량을 바 차트로 표시 + Threshold 표시 | O         | X       | O   | X   |
| **Redo log Volume (Tibero)** | redo log volume 사용량을 바 차트로 표시 | O         | X       | X   | X   |
| **Archive log Volume (Tibero)** | archive log volume 사용량을 바 차트로 표시 | O         | X       | X   | X   |
| **Root Volume** | Root volume 사용량을 바 차트로 표시 | O         | O       | X   | X   |
|     |     |           |         |     |     |
|
{% endtab %}
|     |           |         |     |     |
|     |     |           |         |     |     |
|     |     |           |         |     |     |
|
{% endtabs %}
|     |           |         |     |     |
|     |     |           |         |     |     |

### DB Service List 사용자 설정

DB Service List 화면 우측 상단의 ⚙️ 설정 아이콘을 클릭하면 화면 표시 방식을 사용자 환경에 맞게 설정할 수 있습니다.

* Card View : 카드에 표시되는 정보 영역을 설정합니다.
* List View : 목록에 표시되는 컬럼을 설정합니다.

설정한 내용은 사용자별로 저장되며 이후에도 동일하게 적용됩니다.

{% tabs %}
{% tab title="Card View" %}
Card View에서는 카드에 표시할 정보 영역을 선택할 수 있습니다.
1. DB Service List 화면에서 ⚙️ 설정을 클릭합니다.
2. Configuration에서 표시할 항목을 ON/OFF 합니다.
3. 우측 Preview에서 변경 사항을 미리 확인합니다.
4. 저장을 클릭하여 적용합니다.

### 설정 항목

| 항목  | 설명  |
|:----|:----|
| DB Service Info | DB 서비스 기본 정보를 표시합니다. |
| Node Info | 노드별 상태 및 리소스 정보를 표시합니다. |
| Cluster Info | Cluster 정보 및 Connection Health를 표시합니다. |

{% hint style="info" %}
**참고**\
우측 Preview 영역에서 변경 결과를 실시간으로 확인할 수 있습니다. 저장 전까지 실제 화면에는 적용되지 않습니다.
{% endhint %}
 {% endtab %}

{% tab title="List View" %}
List View에서는 목록에 표시할 컬럼을 선택하거나 컬럼 순서를 변경할 수 있습니다.
1. DB Service List 화면에서 ⚙️ 설정을 클릭합니다.
2. 표시할 컬럼을 ON/OFF 합니다.
3. Drag & Drop으로 컬럼 순서를 변경합니다.
4. 저장을 클릭하여 적용합니다.

### 주요 기능

| 기능  | 설명  |
|:----|:----|
| 컬럼 표시/숨김 | 스위치를 이용하여 컬럼 표시 여부를 설정합니다. |
| 컬럼 검색 | 컬럼명을 검색하여 원하는 항목을 빠르게 찾을 수 있습니다. |
| 컬럼 순서 변경 | 드래그하여 원하는 순서로 변경합니다. |
| 기본값으로 초기화 | 기본 컬럼 구성으로 복원합니다. |
|     |     |
|
{% endtab %}
|     |
|     |     |
|     |     |
|
{% endtabs %}
|     |
|     |     |

# Top N 차트 조회

대시보드 화면에서 조회 권한을 가진 DB 서비스를 대상으로 CPU Usage, Memory Usage, Session Load 기준 상위 인스턴스를 차트로 확인합니다. Top N 차트는 대시보드 우측 영역에 노출되며, 별도 설정 없이 로그인한 사용자의 조회 권한 범위에 맞춰 자동으로 데이터가 구성됩니다. 조회 권한을 가진 DB 서비스가 없는 경우 차트 영역에 데이터 없음 상태가 표시됩니다.

{% hint style="info" %}
**참고**\
**환경별 지원 엔진 차이**

* AWS 환경: Tibero 엔진 인스턴스만 조회 대상에 포함됩니다.
* Azure 환경: Tibero, OpenSQL 엔진 인스턴스가 모두 조회 대상에 포함됩니다.
{% endhint %}

### 표시 지표

| 지표  | 설명  |
|-----|-----|
| CPU Usage | CPU 사용량 기준 상위 5개 인스턴스 차트<br>- 엔진 종류와 관계없이 전체 인스턴스 대상<br>- X축: 인스턴스 Alias<br>- 데이터 레이블(%) 표시 |
| Memory Usage | Memory 사용량 기준 상위 5개 인스턴스 차트<br>- 엔진 종류와 관계없이 전체 인스턴스 대상<br>- X축: 인스턴스 Alias<br>- 데이터 레이블(%) 표시 |
| Session Load | Session Load(%) 기준 상위 5개 인스턴스 차트<br>- Active Session이 Max Session 대비 차지하는 비율을 나타내는 지표<br>- X축: 인스턴스 Alias<br>- 데이터 레이블(%) 표시<br>- 툴팁: (Active Session/Max Session)X100 |

### 차트 영역 확장/축소

Top N 차트 영역 상단의 버튼을 클릭하면 차트 영역이 접히거나 펼쳐집니다.

* 기본 상태는 확장 상태입니다.
* 사용자가 축소 상태로 변경한 경우, 같은 세션 내에서 다른 화면으로 이동했다가 다시 돌아와도 축소 상태가 유지됩니다.
* 로그아웃하거나 세션이 종료되어 새 세션으로 접속하면 확장 상태로 초기화됩니다.
