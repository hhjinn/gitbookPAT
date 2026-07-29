# 세션 모니터링

세션 모니터링은 데이터베이스에 연결된 세션의 현재 상태를 실시간으로 확인하는 기능입니다. 접속 사용자, 실행 중인 SQL, 대기 이벤트, 경과 시간 등 세션 주요 지표를 테이블 형태로 조회하여 이상 세션을 빠르게 파악합니다.

Tibero와 OpenSQL 두 엔진을 모두 지원하며, GNB의 DB Type 토글로 전환하면 해당 엔진에 맞는 지표 목록이 표시됩니다. Elapsed Time 알림 기능을 활성화하면 설정한 임계값을 초과한 세션 행을 색상으로 강조하여 장시간 실행 세션을 시각적으로 식별합니다. 테이블에서 특정 세션 행을 클릭하면 상세 정보와 SQL 전문을 드로어에서 확인할 수 있습니다.


## 세션 모니터링

**모니터링 > 세션 모니터링** 메뉴에서 현재 DB 인스턴스에 연결된 세션 목록을 실시간 테이블로 조회합니다. GNB의 DB Type 토글로 Tibero 또는 OpenSQL을 전환하면 해당 엔진의 세션 지표가 표시됩니다.

선택한 DB Type에 인스턴스가 없거나 인스턴스를 선택하지 않으면 데이터 없음 화면이 표시됩니다. 인스턴스 상태가 비정상이면 DB 선택 트리에서 해당 인스턴스에 ⚠️가 표시되며, 비정상 인스턴스만 선택한 경우에는 세션 데이터가 표시되지 않습니다.


1. **모니터링 > 세션 모니터링** 메뉴를 클릭합니다.
2. GNB의 DB Type 토글로 **Tibero** 또는 **OpenSQL**을 선택합니다.
3. 검색 조건을 선택하고 검색어를 입력해 원하는 세션을 필터링합니다.
4. Elapsed Time 알림 설정(⚙️) 아이콘을 클릭해 임계값을 설정하고 저장합니다.
5. 세션 행을 클릭해 상세 정보 드로어를 열고, 📋 아이콘으로 SQL 전문을 복사합니다.

테이블 상단에서 다음 기능을 이용합니다.

* **수동 새로고침(🔃)**: 클릭하면 세션 데이터를 즉시 갱신합니다.
* **자동 새로고침(⚙️)**: 자동 갱신 여부와 주기를 설정합니다.
* **검색**: 조건을 선택하고 검색어를 입력해 원하는 세션을 필터링합니다. 일치하는 세션이 없으면 "확인 가능한 데이터가 없습니다." 메시지가 표시됩니다.
* **테이블 설정(⚙️)**: 테이블에 표시할 컬럼을 추가하거나 숨깁니다.

검색 조건은 선택한 DB Type에 따라 다릅니다.

{% tabs %}
{% tab title="Tibero" %}
전체 / Instance Alias / SID / Username / Status
{% endtab %}
{% tab title="OpenSQL" %}
전체 / Instance Alias / Database Name / PID / Username / State
{% endtab %}
{% endtabs %}

### 세션 컬럼 구성

테이블 컬럼은 표시 정책에 따라 세 가지로 구분됩니다.

* **항상 표시**: 숨길 수 없는 필수 컬럼
* **기본 표시**: 기본으로 표시되며 사용자가 숨길 수 있는 컬럼
* **선택 표시**: 기본으로 숨겨져 있으며 테이블 설정에서 추가할 수 있는 컬럼

{% tabs %}
{% tab title="Tibero" %}

| 컬럼  | 설명  | 표시 정책 |
|-----|-----|-------|
| Service Name | 서비스 식별자 | 항상 표시 |
| Instance Alias | 인스턴스 식별자 | 항상 표시 |
| SID | 세션 ID (Tibero 고유 식별자) | 항상 표시 |
| Serial# | 세션 재사용 구분용 일련번호 | 선택 표시 |
| PID | 서버 백엔드 프로세스 ID | 항상 표시 |
| Username | DB 접속 사용자 | 항상 표시 |
| Schema | 현재 스키마 이름 | 기본 표시 |
| Session Type | 세션 타입 | 기본 표시 |
| Status | 세션 상태 (`RUNNING` · `READY` · `TX_RECOVERING` · `SESS_CLEANUP` · `ASSIGNED` · `CLOSING` · `ROLLING_BACK`) | 항상 표시 |
| State | 세션의 세부 실행 단계 | 항상 표시 |
| LOGON TIME | 세션 생성 시각 (데이터베이스 타임존 기준) | 기본 표시 |
| Elapsed Time | 현재 SQL 또는 세션 상태의 경과 시간 | 항상 표시 |
| Wait Event | 현재 세션이 대기 중인 이벤트 | 기본 표시 |
| Wait Time | Wait Event 대기 시간 | 기본 표시 |
| WLock Wait | 세션이 대기 중인 lock 유형 | 기본 표시 |
| SQL ID | 실행 중 SQL 식별자 | 기본 표시 |
| SQL Text | 실행 중 SQL 전문 | 기본 표시 |
| Prev SQL ID | 이전에 실행된 SQL 식별자 | 선택 표시 |
| Command | SQL 명령 유형 (SELECT, INSERT 등) | 기본 표시 |
| Program | 접속 애플리케이션 이름 | 기본 표시 |
| Module | 사용자 정의 모듈 이름 | 선택 표시 |
| Action | 모듈의 액션 이름 | 선택 표시 |
| IP Address | 클라이언트 접속 IP | 항상 표시 |
| Client PID | 클라이언트 프로세스 ID | 선택 표시 |
| CONSUMED_CPU_TIME | 세션이 사용한 누적 CPU 시간 | 기본 표시 |
| PGA Used Memory | 세션이 사용 중인 PGA 메모리 | 기본 표시 |
| Machine | 연결된 세션의 호스트 이름 | 선택 표시 |
| OS USER | 연결된 세션의 OS 계정 이름 | 선택 표시 |

{% hint style="info" %}
**참고**
SQL ID 및 Prev SQL ID는 음수 값도 정상 범위에 포함됩니다.
{% endhint %}

{% endtab %}
{% tab title="OpenSQL" %}

| 컬럼  | 설명  | 표시 정책 |
|-----|-----|-------|
| Service Name | 서비스 식별자 | 항상 표시 |
| Instance Alias | 인스턴스 식별자 | 항상 표시 |
| datname | 접속 데이터베이스 이름 | 항상 표시 |
| pid | 서버 백엔드 프로세스 ID | 항상 표시 |
| usename | DB 접속 사용자 | 항상 표시 |
| state | 세션 상태 | 항상 표시 |
| elapsed_time | 현재 쿼리 실행 경과 시간 | 항상 표시 |
| wait_event | 세션 대기 이벤트명 | 항상 표시 |
| wait_event_type | 세션 대기 이벤트 유형 | 항상 표시 |
| query | 현재 또는 마지막 실행 SQL 문 | 항상 표시 |
| backend_type | 백엔드 프로세스 유형 | 기본 표시 |
| backend_start | 세션 또는 백엔드 프로세스 시작 시각 | 기본 표시 |
| application_name | 접속 애플리케이션 이름 | 기본 표시 |
| client_addr | 클라이언트 IP 주소 | 기본 표시 |
| query_id | 쿼리 식별자 | 기본 표시 |
| leader_pid | 병렬 작업 리더 프로세스 ID | 선택 표시 |
| client_port | 클라이언트 통신 포트 번호 | 선택 표시 |
| client_hostname | 클라이언트 호스트 이름 | 선택 표시 |
| query_start | 현재 또는 마지막 쿼리 시작 시각 | 선택 표시 |
| xact_start | 현재 트랜잭션 시작 시각 | 선택 표시 |
| state_change | 세션 상태 마지막 변경 시각 | 선택 표시 |
| xact_elapsed_time | 현재 트랜잭션 경과 시간 | 선택 표시 |
| backend_xid | 최상위 트랜잭션 ID | 선택 표시 |
| backend_xmin | 현재 backend의 xmin horizon | 선택 표시 |

{% endtab %}
{% endtabs %}

### Elapsed Time 알림 설정

Elapsed Time 임계값을 설정하면 경과 시간이 기준을 초과한 세션 행이 색상으로 강조됩니다.

| 상태  | 조건  | 표시 색상 |
|-----|-----|-------|
| 정상  | Elapsed Time < 주의 임계값 | 기본 색상 |
| 주의  | 주의 임계값 ≤ Elapsed Time < 경고 임계값 | 주황색   |
| 경고  | Elapsed Time ≥ 경고 임계값 | 빨간색   |

테이블 상단의 Elapsed Time 알림 설정(⚙️) 아이콘을 클릭하면 설정 패널이 나타납니다. 알림 활성화 토글을 켠 후 경고와 주의 임계값을 입력하고 저장합니다.

| 항목  | 설명  | 입력 규칙 |
|-----|-----|-------|
| 알림 활성화\* | 알림 기능 켜기/끄기 | 토글    |
| 경고\* | 경고 수준 임계값 (초) | `0.1` \~ `1000.0`, 소수점 첫째 자리까지 허용 |
| 주의\* | 주의 수준 임계값 (초) | `0.1` \~ `999.9`, 소수점 첫째 자리까지 허용, 경고 수준보다 작아야 함 |

\* 필수 항목
{% hint style="info" %}
**참고**
알림 활성화 토글을 켜지 않은 상태로 패널을 닫으면 입력한 임계값은 유지됩니다. 단, 페이지를 이동하면 초기값으로 돌아갑니다.
{% endhint %}

### 세션 상세 정보 조회

세션 테이블에서 특정 행을 클릭하면 오른쪽에 상세 정보 드로어가 열립니다. 드로어 제목은 **Session Detail ({SID 또는 PID})** 형식으로 표시됩니다.

| 항목  | 설명  | Tibero | OpenSQL |
|-----|-----|:------:|:-------:|
| Service Name | 세션이 연결된 DB 서비스 이름 | ○      | ○       |
| Instance Alias | 세션이 연결된 인스턴스 별칭 | ○      | ○       |
| Database Name | 세션이 연결된 데이터베이스 이름 | X      | ○       |
| State | 세션의 세부 실행 단계 | ○      | ○       |
| Elapsed Time | SQL 또는 세션 상태의 경과 시간 (초) | ○      | ○       |
| SQL ID / Query ID | 실행 중 SQL 식별자 | ○      | ○       |
| SQL Text / Query | 실행 중 SQL 전문 | ○      | ○       |

SQL Text 영역 우측의 📋 아이콘을 클릭하면 SQL 전문이 클립보드에 복사됩니다. SQL Text가 없는 세션은 해당 영역에 데이터 없음 화면이 표시됩니다. 드로어를 닫으려면 » 아이콘을, 더 넓게 보려면 ⛶ 아이콘을 클릭합니다.
