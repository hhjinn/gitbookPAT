# 인스턴스

**OwlDB**에서 운영 중인 인스턴스의 상태와 자원 사용량을 모니터링하고, 필요에 따라 인스턴스를 수정하거나 재시작할 수 있습니다. 인스턴스 상태가 `Available`이 아닌 경우, 일부 정보가 누락될 수 있습니다.

{% hint style="info" %}
**참고**

* 대시보드에서 다음 경로를 통해 인스턴스 관리 페이지로 이동할 수 있습니다. **\[리스트뷰]** 데이터베이스 별칭 옆 화살표 아이콘 > 인스턴스 별칭 클릭 **\[카드뷰]** 데이터베이스 카드의 인스턴스 별칭 클릭
* AWS 환경에서는 Tibero 엔진만 지원합니다.
{% endhint %}

### 인스턴스 목록 조회

1. **관리 > Overview**로 이동합니다.
2. **인스턴스** 탭을 클릭합니다.
3. 목록에서 확인할 인스턴스의 상태와 자원 사용량을 확인합니다.

#### Primary(Leader) DB 표시 항목

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th width="221">항목</th><th>설명</th></tr></thead><tbody><tr><td>별칭</td><td>인스턴스 별칭 표시(클릭 시 상세 정보 페이지로 이동)</td></tr><tr><td>생성일</td><td>인스턴스 생성 일시(<code>yyyy-mm-dd HH:mm:ss</code>)</td></tr><tr><td>Health</td><td>인스턴스의 현재 상태(Available / In Progress / Limited / Unavailable)</td></tr><tr><td>Open Mode</td><td>DB의 운영 모드(READ WRITE)</td></tr><tr><td>Replication Mode</td><td>Primary DB의 동작 모드(PERFORMANCE)</td></tr><tr><td>CPU</td><td>프로비저닝된 vCPU 대비 사용량 바 차트</td></tr><tr><td>Memory</td><td>프로비저닝된 메모리 대비 사용량 바 차트</td></tr><tr><td>활성 세션</td><td>활성화된 세션 수 바 차트</td></tr><tr><td>Data Volume</td><td>data 볼륨 사용량(90% 임계값 표시 포함)</td></tr><tr><td>Redo log Volume</td><td>redo log 볼륨 사용량</td></tr><tr><td>Archive log Volume</td><td>archive log 볼륨 사용량</td></tr><tr><td>Root Volume</td><td>root 볼륨 사용량</td></tr><tr><td>Current Log</td><td>가장 최근 Redo log 식별값(Standby 구성 시에만 표시)</td></tr></tbody></table>
{% endtab %}

{% tab title="OpenSQL" %}
<table><thead><tr><th width="221">항목</th><th>설명</th></tr></thead><tbody><tr><td>별칭</td><td>인스턴스 별칭 표시(클릭 시 상세 정보 페이지로 이동)</td></tr><tr><td>생성일</td><td>인스턴스 생성 일시(<code>yyyy-mm-dd HH:mm:ss</code>)</td></tr><tr><td>Health</td><td>인스턴스의 현재 상태(Available / In progress / Limited / Unavailable)</td></tr><tr><td>Open Mode</td><td>DB의 운영 모드(READ WRITE)</td></tr><tr><td>CPU</td><td>프로비저닝된 vCPU 대비 사용량 바 차트</td></tr><tr><td>Memory</td><td>프로비저닝된 메모리 대비 사용량 바 차트</td></tr><tr><td>활성 세션</td><td>활성화된 세션 수 바 차트</td></tr><tr><td>Volume</td><td>OpenSQL의 볼륨의 총합과 사용량</td></tr><tr><td>Root Volume</td><td>root 볼륨 사용량</td></tr><tr><td>Current Log</td><td>가장 최근 WAL log 식별값(LSN, HA 구성 시에만 표시)</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

#### Standby(Replica) DB 표시 항목

{% tabs %}
{% tab title="Tibero" %}
<table><thead><tr><th width="218">항목</th><th>설명</th></tr></thead><tbody><tr><td>별칭</td><td>인스턴스 별칭 표시(클릭 시 상세 정보 페이지로 이동)</td></tr><tr><td>생성일</td><td>인스턴스 생성 일시(<code>yyyy-mm-dd HH:mm:ss</code>)</td></tr><tr><td>Health</td><td>인스턴스의 현재 상태(Available / In progress / Limited / Unavailable / Retired)</td></tr><tr><td>Standby Status</td><td><code>v$standby</code> 조회 시 나오는 <code>status</code> 값 표시</td></tr><tr><td>Open Mode</td><td>DB의 운영 모드(MOUNTED / RECOVERY / READ WRITE / READ ONLY / READ ONLY WITH APPLY)</td></tr><tr><td>Log Replication Type</td><td>Standby의 복제 방식(LGWR ASYNC / ARCH ASYNC)</td></tr><tr><td>CPU</td><td>프로비저닝된 vCPU 대비 사용량 바 차트</td></tr><tr><td>Memory</td><td>프로비저닝된 메모리 대비 사용량 바 차트</td></tr><tr><td>활성 세션</td><td>활성화된 세션 수 바 차트(Read Only 상태일 때만 표시)</td></tr><tr><td>log last received</td><td>Primary로부터 최근 수신한 Redo log 식별값(TSN 값)</td></tr><tr><td>log last applied</td><td>Standby에 최근 적용된 Redo log 식별값(TSN 값)</td></tr><tr><td>Replication Lag(초)</td><td>Primary DB와의 복제 지연 시간</td></tr></tbody></table>
{% endtab %}

{% tab title="OpenSQL" %}
<table><thead><tr><th width="240">항목</th><th>설명</th></tr></thead><tbody><tr><td>별칭</td><td>인스턴스 별칭 표시(클릭 시 상세 정보 페이지로 이동)</td></tr><tr><td>생성일</td><td>인스턴스 생성 일시(<code>yyyy-mm-dd HH:mm:ss</code>)</td></tr><tr><td>Health</td><td>인스턴스의 현재 상태(Available / In Progress / Limited / Unavailable)</td></tr><tr><td>Open Mode</td><td>DB의 운영 모드(READ ONLY)</td></tr><tr><td>Log Replication Type</td><td>Replica의 복제 방식(ASYNC / SYNC)</td></tr><tr><td>CPU</td><td>프로비저닝된 vCPU 대비 사용량 바 차트</td></tr><tr><td>Memory</td><td>프로비저닝된 메모리 대비 사용량 바 차트</td></tr><tr><td>활성 세션</td><td>활성화된 세션 수 바 차트(항상 표시)</td></tr><tr><td>log last received</td><td>Leader로부터 최근 수신한 WAL log 식별값(LSN 값)</td></tr><tr><td>log last applied</td><td>Replica에 최근 적용된 WAL log 식별값(LSN 값)</td></tr><tr><td>Replication Lag(초)</td><td>Leader DB와의 복제 지연 시간</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**주의**

인스턴스 상태가 `Available`이 아닌 경우, 화면 상단에 배너가 나타나 현재 상태에 대한 안내를 제공합니다.
{% endhint %}

### 인스턴스 상세 정보 조회

인스턴스 목록에서 별칭을 클릭하면 해당 인스턴스의 상세 정보 페이지로 이동합니다. 상세 페이지는 상단 요약 정보, 가용성 및 복제 정보, 자원 사용 현황, 네트워크 정보, 데이터베이스 정보 순으로 구성됩니다.

1. Overview 페이지에서 **인스턴스** 탭을 클릭합니다.
2. 인스턴스 목록에서 상세 정보를 확인할 인스턴스의 별칭을 클릭합니다.
3. 상세 정보 페이지에서 상단 정보, 가용성 및 복제 정보, 자원 사용 정보, 네트워크 정보, 데이터베이스 정보를 확인합니다.

#### 상단 정보

| 항목          | 설명                              | Primary/Leader                                  | Standby/Replica                                                                        |
| ----------- | ------------------------------- | ----------------------------------------------- | -------------------------------------------------------------------------------------- |
| Health      | 인스턴스 상태                         | Available / In progress / Limited / Unavailable | 공통(Tibero는 Retired 포함)                                                                 |
| 역할          | 인스턴스 역할                         | Tibero: Primary / OpenSQL: Leader               | Tibero: Standby(Recovery/Read Only) / OpenSQL: Replica                                 |
| 인스턴스 생성일    | 생성 일시(`yyyy-mm-dd HH:mm:ss`)    | 공통                                              | 공통                                                                                     |
| Open Mode   | DB 운영 모드                        | READ WRITE                                      | Tibero: MOUNTED/RECOVERY/READ WRITE/READ ONLY/READ ONLY WITH APPLY, OpenSQL: READ ONLY |
| 마지막 업데이트 일자 | 설정 변경 일시(`yyyy-mm-dd HH:mm:ss`) | 공통                                              | 공통                                                                                     |

#### 가용성 및 복제 정보

Primary/Leader는 아래 항목을 표시하며, Standby/Replica는 동일 항목에 복제 관련 항목을 추가로 표시합니다.

{% tabs %}
{% tab title="Tibero" %}
| 항목                   | 설명                              | 표시 대상   |
| -------------------- | ------------------------------- | ------- |
| Replication Mode     | Primary DB의 동작 모드(PERFORMANCE)  | Primary |
| Current Log          | 최근 Redo log 식별값(TSN)            | 공통      |
| Standby Status       | Standby 복제 상태(노드별 상이 가능)        | Standby |
| Log Replication Type | 복제 방식(LGWR ASYNC / ARCH ASYNC)  | Standby |
| log last received    | Primary로부터 수신한 최근 Redo log(TSN) | Standby |
| log last applied     | Standby에 적용된 최근 Redo log(TSN)   | Standby |
| Replication Lag(초)   | Primary와의 복제 지연 시간              | Standby |
{% endtab %}

{% tab title="OpenSQL" %}
| 항목                   | 설명                            | 표시 대상   |
| -------------------- | ----------------------------- | ------- |
| Current Log          | 최근 WAL log 식별값(LSN)           | 공통      |
| Log Replication Type | 복제 방식(ASYNC / SYNC)           | Standby |
| log last received    | Leader로부터 수신한 최근 WAL log(LSN) | Replica |
| log last applied     | Replica에 적용된 최근 WAL log(LSN)  | Replica |
| Replication Lag(초)   | Primary와의 복제 지연 시간            | Standby |
{% endtab %}
{% endtabs %}

#### 자원 사용 정보

| 항목        | 설명                        | 비고                                                                       |
| --------- | ------------------------- | ------------------------------------------------------------------------ |
| CPU       | 프로비저닝된 vCPU 대비 사용량(파이 차트) | 5초 주기 갱신                                                                 |
| Memory    | 프로비저닝된 메모리 대비 사용량(파이 차트)  | 5초 주기 갱신                                                                 |
| 최대 접속 세션수 | 활성 세션 수(라인 차트, 5초 주기 갱신)  | <p>- Tibero Standby: Read Only 상태일 때만 표시<br>- OpenSQL Replica: 항상 표시</p> |

#### 네트워크 정보

<table><thead><tr><th width="226">항목</th><th>설명</th></tr></thead><tbody><tr><td>Host Name</td><td>데이터베이스 서버가 실행 중인 호스트 이름</td></tr><tr><td>End Point</td><td>클라이언트 접속 주소(Private IP)</td></tr><tr><td>Port</td><td>데이터베이스 통신 포트 번호</td></tr></tbody></table>

#### 데이터베이스 정보

{% hint style="info" %}
**참고**

데이터베이스 정보는 OpenSQL 엔진에서만 제공합니다.
{% endhint %}

<table><thead><tr><th width="222">항목</th><th>설명</th></tr></thead><tbody><tr><td>Auto Vacuum</td><td>Auto Vacuum 사용 여부(On / Off)</td></tr><tr><td>데이터베이스 목록</td><td>하위 데이터베이스를 이름, Data Size(GB), 활성 세션, Bloat Ratio(%), 생성일로 표시하며, 이름 클릭 시 상세 정보로 이동</td></tr></tbody></table>

활성 세션 값은 조회 중인 인스턴스가 Primary/Leader이면 Primary 노드 기준, Standby/Replica이면 Standby 노드 기준으로 표시됩니다.

{% hint style="warning" %}
**주의**

* Health가 `Available`이 아닌 경우 일부 정보가 누락되어 표시될 수 있습니다.
* Health가 `Retired`인 경우(Tibero의 Standby 인스턴스에서만 발생) Health를 제외한 모든 정보가 `-`로 표시되며, **재시작** 버튼 대신 **삭제** 버튼이 나타납니다.
{% endhint %}
