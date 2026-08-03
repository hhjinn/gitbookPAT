# 인스턴스 모니터링

인스턴스 모니터링 페이지에서는 데이터베이스 인스턴스의 주요 성능 지표를 실시간 라인 차트로 확인할 수 있습니다.

CPU 사용률, 메모리 사용률, 활성 세션 수 등 핵심 지표는 화면 상단에 항상 표시됩니다. Quick Insight·Load·I/O·Writes·All 탭을 선택해 상황별 지표를 집중적으로 조회하며, GNB의 DB Type 토글로 Tibero와 OpenSQL을 전환하면 해당 엔진에 맞는 지표 구성이 적용됩니다.

{% hint style="info" %}
**참고**\
DB Type에 따라 모니터링 대상 리소스 계층이 다릅니다. Tibero는 Service → Instance 단위로 지표를 표시하며, OpenSQL은 Service → Instance → Database 단위까지 선택합니다.
{% endhint %}

## 인스턴스 모니터링 조회

인스턴스 모니터링 페이지는 GNB 상단에서 DB Type과 조회 대상 리소스를 선택하는 것으로 시작합니다. DB Type에 따라 선택 가능한 리소스 계층과 표시되는 지표가 달라집니다.


1. 좌측 메뉴에서 **모니터링 > 인스턴스 모니터링**을 클릭합니다.
2. GNB의 DB Type 토글에서 **Tibero** 또는 **OpenSQL**을 선택합니다.
3. DB Select 트리에서 조회할 Service와 Instance를 선택합니다. OpenSQL은 Database 단위까지 선택할 수 있습니다.
4. **(OpenSQL만 해당)** 필요에 따라 **Instance View** 또는 **Database View** 버튼을 클릭합니다.
5. 탭(**Quick Insight** / **Load** / **I/O** / **Writes** / **All**)을 클릭하여 원하는 지표를 조회합니다.
6. 필요한 경우 ⚙️ 아이콘을 클릭하여 자동 새로고침을 설정하거나, 🔃 아이콘을 클릭하여 데이터를 수동으로 갱신합니다.

### DB Type 및 리소스 선택

GNB의 DB Type 토글에서 **Tibero** 또는 **OpenSQL**을 선택하면, 해당 DB Type에 맞는 DB Select 트리와 지표가 표시됩니다. DB Type을 전환하면 DB Select는 전체 선택으로 초기화됩니다.

{% tabs %}
{% tab title="Tibero" %}
DB Select 트리는 **Service → Instance** 2단계 구조입니다. 전체 선택, Primary DB, Standby DB 단위로 선택할 수 있습니다.

* Instance Alias는 최대 30자까지 표시되며, 초과 시 말줄임표(…)로 줄여서 표시합니다.
* 정렬 순서: Service 생성일 최신순 → Instance Role (Primary → Standby(Read Only) → Standby(Recovery)) → 같은 역할 내 별칭 오름차순
{% endtab %}
{% tab title="OpenSQL" %}
DB Select 트리는 **Service → Instance → Database** 3단계 구조입니다. Database 단위까지 선택할 수 있으며, Database를 선택하면 상위 Instance가 자동으로 포함됩니다.
* Database를 일부만 선택하면 Instance View에서도 선택된 Database를 기준으로만 지표를 집계합니다.
* Instance Alias와 Database 이름은 각각 최대 10자까지 표시되며, 초과 시 말줄임표(…)로 줄여서 표시합니다.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**참고**\
선택한 DB Type에 리소스가 없거나 수집된 데이터가 없으면 No Data 화면이 표시됩니다. 인스턴스 Health가 비정상인 경우 DB Select 트리에 ⚠️ 아이콘이 표시되며, 해당 리소스만 선택하면 No Data 화면으로 전환됩니다.
{% endhint %}

### 상단 고정 지표

DB Type에 관계없이 페이지 상단에 항상 표시되는 핵심 지표입니다. 현재 DB Type의 인스턴스 Health 상태(`Health(n)` `Available(n)` `Limited(n)` `Unavailable(n)` `In Progress(n)`)도 함께 표시됩니다.

| 지표명 | 설명  |
|-----|-----|
| CPU Usage (%) | CPU 사용률 |
| Memory Usage (%) | 메모리 사용률 |
| Active Session Count (CNT) | 현재 활성 중인 세션 수 |
| Replication Lag (sec) | Primary-Standby 간 복제 지연 시간 (Standby가 있는 경우에만 표시) |

### 탭별 지표 조회

페이지 진입 시 기본 탭은 **Quick Insight**입니다. 탭을 전환하면 해당 상황에 맞는 지표만 조회합니다.

| 탭   | 확인 목적 |
|-----|-------|
| Quick Insight | 전반적인 이상 징후를 빠르게 파악 |
| Load | 세션·쿼리 부하 및 CPU/Memory 상승 원인 확인 |
| I/O | 디스크 읽기·쓰기 활동량 및 응답 지연 원인 확인 |
| Writes | 대량 쓰기 작업 및 트랜잭션 롤백 급증 확인 |
| All | 전체 지표를 한 번에 조회하여 복합 원인 진단 |

자주 사용하는 탭은 고정할 수 있습니다. 탭에 마우스를 올리면 고정 아이콘이 나타나고, 클릭하면 해당 탭이 기본 진입 탭으로 설정됩니다. 최대 1개만 고정할 수 있으며, 다른 탭을 고정하면 기존 고정이 자동 해제됩니다.

탭별로 표시되는 지표는 DB 엔진에 따라 다릅니다.

{% tabs %}
{% tab title="Tibero" %}

**Quick Insight**

| 지표명 | 설명  |
|-----|-----|
| Total Session Count (CNT) | 현재 연결된 전체 세션 수 |
| Execute Count (CNT) | 단위 시간 내 실행된 쿼리 횟수 |
| Buffer Cache Hit (%) | Logical Reads와 Physical Reads의 비율 |
| Physical Reads (CNT) | 디스크에서 직접 읽은 페이지 수 |
| Redo Log Rate (MB/s) | 단위 시간 당 Redo Log에 기록된 데이터 양 |
| User Rollbacks (CNT) | 단위 시간 내 롤백된 트랜잭션 수 |

**Load**

| 지표명 | 설명  |
|-----|-----|
| Total Session Count (CNT) | 현재 연결된 전체 세션 수 |
| Execute Count (CNT) | 단위 시간 내 실행된 쿼리 횟수 |
| Logical Reads (CNT) | 버퍼 캐시에서 데이터를 읽은 수 |
| Physical Reads (CNT) | 디스크에서 직접 읽은 페이지 수 |
| Buffer Cache Hit (%) | Logical Reads와 Physical Reads의 비율 |
| Hard Parse Count (CNT) | 캐시를 사용하지 않고 전체 실행 과정을 거친 SQL 횟수 |

**I/O**

| 지표명 | 설명  |
|-----|-----|
| Physical Reads (CNT) | 디스크에서 데이터를 읽은 수 |
| Disk Read Rate (MB/s) | 단위 시간 당 디스크에서 읽기 요청을 처리한 데이터 양 |
| Disk Write Rate (MB/s) | 단위 시간 당 디스크에서 쓰기 요청을 처리한 데이터 양 |
| Multi Block Disk Read (block/s) | 단위 시간 내 멀티 블록 디스크 읽기 블록 수(초당) |
| Physical Write (block/s) | 단위 시간 내 DBWR 기록 블록 수(초당) |

**Writes**

| 지표명 | 설명  |
|-----|-----|
| Execute Count (CNT) | 단위 시간 내 실행된 쿼리 횟수 |
| Redo Entries (CNT) | 로그 버퍼에 기록된 Redo 레코드 수 |
| Redo Log Rate (MB/s) | 단위 시간 당 Redo Log에 기록된 데이터 양 |
| User Rollbacks (CNT) | 단위 시간 내 롤백된 트랜잭션 수 |

{% endtab %}
{% tab title="OpenSQL" %}

**Quick Insight**

| 지표명 | 설명  |
|-----|-----|
| Total Session Count (CNT) | 현재 연결된 전체 세션 수 |
| Execute Count (CNT) | 단위 시간 내 실행된 쿼리 횟수 |
| Buffer Cache Hit (%) | Logical Reads와 Physical Reads의 비율 |
| Physical Reads (CNT) | 디스크에서 직접 읽은 페이지 수 |
| WAL Rate (MB/s) | 단위 시간 당 WAL Log에 기록된 데이터 양 |
| Rollbacks (CNT) | 단위 시간 내 롤백된 트랜잭션 수 |

**Load**

| 지표명 | 설명  |
|-----|-----|
| Total Session Count (CNT) | 현재 연결된 전체 세션 수 |
| Execute Count (CNT) | 단위 시간 내 실행된 쿼리 횟수 |
| Logical Reads (CNT) | 버퍼 캐시에서 데이터를 읽은 수 |
| Physical Reads (CNT) | 디스크에서 직접 읽은 페이지 수 |
| Buffer Cache Hit (%) | Logical Reads와 Physical Reads의 비율 |
| Transactions (CNT) | 단위 시간 내 완료된 트랜잭션 수 |

**I/O**

| 지표명 | 설명  |
|-----|-----|
| Physical Reads (CNT) | 디스크에서 데이터를 읽은 수 |
| Disk Read Rate (MB/s) | 단위 시간 당 디스크에서 읽기 요청을 처리한 데이터 양 |
| Disk Write Rate (MB/s) | 단위 시간 당 디스크에서 쓰기 요청을 처리한 데이터 양 |
| Block Read Latency (ms/block) | 데이터 블록 1개를 읽는 데 소요된 평균 시간 |
| Block Write Latency (ms/block) | 데이터 블록 1개를 기록하는 데 소요된 평균 시간 |

**Writes**

| 지표명 | 설명  |
|-----|-----|
| Execute Count (CNT) | 단위 시간 내 실행된 쿼리 횟수 |
| Transactions (CNT) | 단위 시간 내 완료된 트랜잭션 수 |
| WAL Rate (MB/s) | 단위 시간 당 WAL Log에 기록된 데이터 양 |
| Rollbacks (CNT) | 단위 시간 내 롤백된 트랜잭션 수 |

{% endtab %}
{% endtabs %}

### OpenSQL View 전환

OpenSQL을 선택한 경우, 화면에 **Instance View**와 **Database View** 전환 버튼이 추가로 표시됩니다.

| View | 설명  |
|------|-----|
| Instance View (기본값) | 선택한 Database의 지표를 합산 또는 평균하여 Instance 단위로 표시. DB Select 트리는 2단계로 표시 |
| Database View | 선택한 Database별로 지표를 분리하여 표시. DB Select 트리는 3단계로 전환 |

{% hint style="info" %}
**참고**\
Database View로 전환해도 Instance 레벨에서만 제공되는 지표는 Instance 기준으로 그대로 표시됩니다.
{% endhint %}
