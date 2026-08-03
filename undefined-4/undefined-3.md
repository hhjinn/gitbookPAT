# 연결 정보 관리

연결 정보 관리는 DB 서비스의 접속 주소를 확인하고, 접근 제어 및 외부 연동 설정을 통합 관리하는 메뉴입니다.

메뉴는 DB 엔진에 따라 제공되는 탭이 다릅니다.

| 탭   | 설명  | Tibero | OpenSQL |
|-----|-----|:------:|:-------:|
| Endpoint | 외부 애플리케이션이 접속 가능한 엔드포인트 주소를 확인합니다. | ✓      | ✓       |
| Access Control | IP 기반 접근 허용·차단 규칙(pg_hba)을 조회하고 관리합니다. | —      | ✓       |
| OpenProxy | OpenProxy 파라미터와 Pool, User, Shard 구성을 조회하고 수정합니다. | —      | ✓       |
| Replication Slot | 외부 시스템과의 연동에 사용하는 Replication Slot을 조회하고 관리합니다. | —      | ✓       |


## 공통 상단 영역

연결 정보 관리 화면 상단에는 현재 선택된 DB 서비스의 식별 정보가 모든 탭에 걸쳐 고정으로 표시됩니다.

| 항목  | 설명  |
|-----|-----|
| Status | DB 서비스의 현재 상태 |
| DB Type | 데이터베이스 엔진 유형 |
| Topology | 데이터베이스 클러스터 구성 방식 |


---

## Endpoint 탭

Endpoint 탭은 **Service Endpoint**와 **Endpoint Details** 두 영역으로 구성되며, DB 서비스의 대표 접속 주소와 인스턴스별 상세 정보를 조회합니다.

**Service Endpoint**

| 항목  | 설명  |
|-----|-----|
| Endpoint | 서비스 대표 접속 주소. **Single 토폴로지**는 VIP가 없으므로 Private IP로 표시하고, **HA·TAC·DR 토폴로지**는 VIP로 표시 |
| Port | DB Listener 포트 번호 |

**Endpoint Details**

| 컬럼  | 설명  |
|-----|-----|
| 별칭  | 인스턴스 별칭 |
| 역할  | Primary / Standby(Recovery) / Standby(Read Only) |
| VIP | 인스턴스 접속용 VIP 주소 |
| Private IP | 인스턴스 내부 네트워크 주소 |
| Port | DB Listener 포트 번호 |
| Health | 인스턴스 상태 |

생성이 완료된 인스턴스만 목록에 나타나며, 생성 중인 인스턴스는 표시되지 않습니다. Failover 또는 Switchover가 발생하더라도 Service Endpoint는 항상 **현재 Primary 인스턴스를 기준**으로 표시됩니다.

{% hint style="info" %}
**참고**\
스펙 변경 작업이 진행 중인 경우 화면 상단에 진행 중 배너가 표시되며, 이 상태에서 상단 공통 영역의 **Topology** 항목은 변경 적용 이전의 토폴로지를 표시합니다.
{% endhint %}

### Endpoint 조회 방법


1. 상단 메뉴에서 **관리 > 연결 정보 관리**를 클릭합니다.
2. **Endpoint** 탭을 클릭합니다.
3. **Service Endpoint** 영역에서 DB 서비스의 대표 접속 주소와 포트를 확인합니다.
   * Single 토폴로지: Private IP 주소가 표시됩니다.
   * DR / TAC / HA 토폴로지: VIP 주소가 표시됩니다.
4. **Endpoint Details** 목록에서 인스턴스별 별칭, 역할, VIP, Private IP, 포트, Health 상태를 확인합니다.
5. Endpoint 주소를 복사하려면 해당 행의 📋 아이콘을 클릭합니다. 상단의 🔃 아이콘으로 목록을 수동 새로고침할 수 있습니다.


---

## Access Control 탭

현재 DB 서비스에 적용된 pg_hba 규칙 목록을 테이블 형식으로 표시합니다. 규칙은 Priority 오름차순으로 고정 정렬되며, 위에 위치한 규칙일수록 먼저 적용됩니다.

| 컬럼  | 설명  |
|-----|-----|
| Priority | 규칙의 적용 순서. 번호가 작을수록 먼저 적용 |
| Type | 연결 유형 (`local` / `host` / `hostssl` / `hostnossl`) |
| 데이터베이스 별칭 | 규칙이 적용되는 데이터베이스 이름 |
| User | 규칙이 적용되는 사용자 이름 |
| Address | 허용 또는 차단할 클라이언트 주소 |
| Method | 인증 방식 |
| Auth Option | Method에 따른 세부 인증 옵션 |
| Comment | 규칙에 대한 설명 |

화면은 **조회 모드**와 **수정 모드** 두 가지 상태로 동작합니다. 조회 모드에서는 **생성**/**삭제** 버튼으로 규칙을 추가·제거하고, 수정 모드에서는 테이블 전체가 인라인 편집 가능한 상태로 전환되어 기존 규칙 값이나 Priority(순서)를 변경합니다.

{% hint style="warning" %}
**주의**\
시스템이 자동으로 생성한 고정 규칙은 수정 모드에서도 편집 및 순서 변경이 불가합니다. 최소 상위 3개 규칙이 시스템 고정 규칙에 해당하며, Barman 설정에 따라 최대 4개가 될 수 있습니다. 사용자가 추가하는 규칙의 Priority는 시스템 고정 규칙 다음 번호부터 지정할 수 있습니다.
{% endhint %}

### 규칙 조회


1. **관리 > 연결 정보 관리**에서 **Access Control** 탭을 클릭합니다.
2. 현재 적용된 pg_hba 규칙 목록을 Priority 오름차순으로 확인합니다. 목록 상단에 고정된 시스템 규칙은 수정 및 삭제할 수 없습니다.

### 규칙 생성


1. **\[생성\]** 버튼을 클릭합니다.
2. 오른쪽 드로어에서 아래 항목을 입력합니다.

| 항목  | 설명  | 입력 규칙 |
|-----|-----|-------|
| Priority | 규칙 적용 순서 (숫자가 작을수록 우선 적용) | 미입력 시 마지막 순서로 추가됩니다. 시스템 고정 규칙 번호 이후부터 입력 가능합니다. |
| Type \* | 연결 유형 | `local` / `host` / `hostssl` / `hostnossl` 중 선택. 기본값: `host` |
| Database \* | 규칙을 적용할 데이터베이스 | 데이터베이스 목록에서 하나 이상 선택하거나 특수 키워드(`all`, `sameuser`, `samerole`) 중 하나를 선택합니다. 특수 키워드와 데이터베이스 목록은 동시에 선택할 수 없습니다. |
| User \* | 규칙을 적용할 사용자 | 사용자 목록에서 하나 이상 선택하거나 `all`을 선택합니다. |
| Address \* | 접근을 허용할 클라이언트 주소 | CIDR 또는 Hostname을 직접 입력하거나 특수 키워드(`all`, `samehost`, `samenet`)를 선택합니다. Type이 `local`이면 비활성화됩니다. 단일 IP 입력 시 자동으로 CIDR 형식으로 변환됩니다 (IPv4: `/32`, IPv6: `/128`). |
| Method \* | 인증 방식 | 드롭다운에서 선택합니다. 기본값: `scram-sha-256` |
| Auth Option | Method에 대한 세부 인증 옵션 | Method에 따라 입력 방식이 달라집니다. `trust` 또는 `reject` 선택 시 비활성화됩니다. `scram-sha-256` 또는 `md5` 선택 시 드롭다운으로 선택합니다. 그 외 Method는 `key=value` 형식으로 입력합니다. |
| Comment | 규칙에 대한 메모 | 줄바꿈 입력 불가 |

\* 필수 항목


3. 입력을 완료한 후 **저장** 버튼을 클릭합니다.

### 규칙 수정


1. **\[수정\]** 버튼을 클릭합니다.
2. 수정 모드로 전환되면 테이블의 각 항목을 인라인으로 수정합니다.
   * Priority를 변경하면 영향을 받는 다른 규칙의 순서가 자동으로 조정됩니다.
   * Type을 `local`로 변경하면 Address 필드가 비활성화됩니다.
3. 수정이 완료되면 **저장** 버튼을 클릭합니다.
4. 변경사항 비교 모달에서 수정 전후 내용을 확인한 후 **저장** 버튼을 클릭합니다. 저장이 완료되면 pg_hba에 즉시 반영됩니다.

{% hint style="warning" %}
**주의**\
저장 시 연결이 다시 검증될 수 있습니다. 변경사항 비교 모달에서 내용을 충분히 확인한 후 저장합니다.
{% endhint %}

### 규칙 삭제


1. 삭제할 규칙의 체크박스를 선택합니다.
2. **삭제** 버튼을 클릭합니다.
3. 삭제 확인 모달에서 **삭제** 버튼을 클릭합니다.


---

## OpenProxy 탭

OpenProxy 파라미터를 **Scope** 단위로 조회하고 수정합니다. 화면 왼쪽의 **Select Scope** 영역에서 조회 범위를 선택하면 오른쪽 테이블에 해당 Scope의 파라미터 목록이 표시됩니다. 기본 선택값은 **General**입니다.

| Scope | 설명  |
|-------|-----|
| General | 전역 설정 파라미터 |
| Virtual Router | HA/VIP 관련 설정 파라미터 |
| Pool  | 특정 Pool 단위 파라미터 |
| User  | 특정 Pool 내 특정 사용자 단위 파라미터 |
| Shard | 특정 Pool 내 특정 Shard 단위 파라미터 |

Pool, User, Shard는 아코디언 구조로 표시됩니다.

**파라미터 목록 테이블**

| 컬럼  | 설명  |
|-----|-----|
| 이름  | 파라미터명 |
| 형식  | 파라미터 데이터 형식 |
| 기본값 | 사용자가 설정하지 않았을 때 적용되는 기본값 |
| 현재값 | 현재 적용된 값 |
| 동적 파라미터 | 재시작 없이 즉시 적용 가능 여부 (`예` / `아니요`) |

수정 모드는 화면 단위가 아닌 **세션 단위**로 동작하여, Scope를 변경하더라도 이미 수정한 내용은 유지됩니다. 저장 시 **동적 파라미터만 수정한 경우**는 재시작 없이 즉시 반영되고, **정적 파라미터(예: Port)도 함께 수정한 경우**는 OpenProxy 재기동 후 반영됩니다.

### 파라미터 조회


1. **관리 > 연결 정보 관리**에서 **OpenProxy** 탭을 클릭합니다.
2. 기본적으로 **General** Scope의 파라미터 목록이 표시됩니다.
3. **Select Scope**에서 원하는 조회 범위를 선택합니다.
4. 이름, 기본값, 현재값으로 파라미터를 검색하거나, **동적 파라미터** 여부로 필터링합니다.

### 파라미터 수정


1. **수정** 버튼을 클릭합니다.

   {% hint style="info" %}
**참고**\
DB 서비스 상태가 `Running` 상태일 때만 **수정** 버튼이 활성화됩니다.
{% endhint %}
2. 수정 모드로 전환되면 테이블에서 변경할 파라미터의 **현재값**을 직접 수정합니다. 임시 변경된 파라미터는 파란색으로 표시됩니다.
3. Scope를 변경해도 수정 중인 내용은 유지됩니다.
4. 수정이 완료되면 **저장** 버튼을 클릭합니다.
5. 저장 확인 모달에서 수정 사항을 확인한 후 **적용** 버튼을 클릭합니다.
   * 동적 파라미터만 수정한 경우: OpenProxy 재기동 없이 즉시 반영됩니다.
   * 정적 파라미터가 포함된 경우: OpenProxy 재기동 후 반영됩니다.

{% hint style="info" %}
**참고** **저장** 버튼을 클릭하기 전까지 변경 사항은 서버에 반영되지 않습니다. **\[취소\]** 버튼을 클릭하면 모든 변경 사항이 초기화됩니다.
{% endhint %}

### Pool / User / Shard 생성


1. **수정** 버튼을 클릭하여 수정 모드로 전환합니다.
2. Select Scope 영역에서 생성할 유형(Pool, User, Shard)의 ➕ 아이콘을 클릭합니다.
3. 생성 모달에서 아래 항목을 입력합니다.

{% tabs %}
{% tab title="Pool 생성" %}

| 항목  | 설명  | 입력 규칙 |
|-----|-----|-------|
| Pool Name \* | Pool 이름 | 1\~63자, 영어 소문자(a-z)·숫자(0-9)·언더바(`_`) 사용 가능. 첫 글자는 숫자 불가. DB 서비스 내 중복 불가. |
| User Name \* | Pool에 속할 사용자 이름 | 1\~63자, 영어 소문자(a-z)·숫자(0-9)·언더바(`_`) 사용 가능. 첫 글자는 숫자 불가. |
| Pool Size \* | 해당 사용자가 동시에 점유할 수 있는 DB 서버 연결 최대 개수 | 정수 입력. 범위: 1 \~ max connections. 기본값: 9 |
| Password \* | 사용자 비밀번호 | 1\~30자, 영어 소문자(a-z)·숫자(0-9)·특수문자(`-`, `_`, `#`, `$`) 사용 가능 |
| Shard Name \* | Pool에 생성할 Shard 이름 | 1\~30자, 영어 소문자(a-z)·숫자(0-9)·언더바(`_`) 사용 가능. 동일 Pool 내 중복 불가. |
| Database Name \* | Pool에 연결할 데이터베이스 | 드롭다운에서 선택 |
| Servers \* | 접속할 DB 서버 | 드롭다운에서 1개 이상 선택. 인스턴스의 역할(Role)과 별칭(Instance Alias)이 표시됩니다. |
| Use Patroni | Patroni를 통한 Auto Failover 사용 여부 | 항상 활성화(변경 불가) |

\* 필수 항목

{% endtab %}
{% tab title="User 생성" %}

| 항목  | 설명  | 입력 규칙 |
|-----|-----|-------|
| User Name \* | 추가할 사용자 이름 | 1\~63자, 영어 소문자(a-z)·숫자(0-9)·언더바(`_`) 사용 가능. 첫 글자는 숫자 불가. 동일 Pool 내 중복 불가. |
| Pool Size \* | 해당 사용자가 동시에 점유할 수 있는 DB 서버 연결 최대 개수 | 정수 입력. 범위: 1 \~ max connections. 기본값: 9 |
| Password \* | 사용자 비밀번호 | 1\~30자, 영어 소문자(a-z)·숫자(0-9)·특수문자(`-`, `_`, `#`, `$`) 사용 가능 |

\* 필수 항목

{% endtab %}
{% tab title="Shard 생성" %}

| 항목  | 설명  | 입력 규칙 |
|-----|-----|-------|
| Shard Name \* | 추가할 Shard 이름 | 1\~30자, 영어 소문자(a-z)·숫자(0-9)·언더바(`_`) 사용 가능. 동일 Pool 내 중복 불가. |
| Database Name \* | Shard에 연결할 데이터베이스 | 드롭다운에서 선택 |
| Servers \* | 접속할 DB 서버 | 드롭다운에서 1개 이상 선택. 인스턴스의 역할(Role), 별칭(Instance Alias), Health 정보가 표시됩니다. Health는 참고용 정보이며 서버 선택에는 영향을 미치지 않습니다. |
| Use Patroni | Patroni를 통한 Auto Failover 사용 여부 | 항상 활성화(변경 불가) |

\* 필수 항목

{% endtab %}
{% endtabs %}


4. 필수 항목을 모두 입력한 후 **생성** 버튼을 클릭합니다. 생성된 항목이 목록에 임시로 추가됩니다.
5. 최종 반영을 위해 **저장** 버튼을 클릭합니다.

{% hint style="info" %}
**참고** **저장** 버튼을 클릭하기 전까지 생성한 항목은 서버에 반영되지 않습니다. 저장 전 페이지를 이탈하면 변경 사항이 초기화됩니다.
{% endhint %}

### Pool / User / Shard 삭제


1. **수정** 버튼을 클릭하여 수정 모드로 전환합니다.
2. Select Scope 영역에서 삭제할 Pool, User 또는 Shard 항목의 🗑️ 아이콘을 클릭합니다. 해당 항목이 비활성화되고 아이콘이 🔃로 변경됩니다.
   * Pool을 삭제하면 해당 Pool 하위의 User와 Shard도 함께 비활성화됩니다.
3. 삭제를 취소하려면 🔃 아이콘을 클릭합니다.
4. 삭제를 확정하려면 **저장** 버튼을 클릭합니다.

{% hint style="info" %}
**참고**\
🗑️ 아이콘은 Pool이 2개 이상일 때 활성화됩니다. User와 Shard는 해당 Pool 내에 각각 2개 이상 존재할 때 삭제할 수 있습니다.
{% endhint %}

{% hint style="warning" %}
**주의**\
저장 전 페이지를 이탈하면 삭제 설정이 초기화됩니다. 현재 선택 중인 Pool, User 또는 Shard를 삭제하면 Scope가 자동으로 General로 변경됩니다.
{% endhint %}


---

## Replication Slot 탭

OpenSQL Primary 인스턴스에 생성된 Replication Slot 목록을 테이블 형식으로 표시합니다. Failover 또는 Switchover가 발생하더라도 항상 **현재 Primary 인스턴스를 기준**으로 조회됩니다.

| 컬럼  | 설명  |
|-----|-----|
| 이름  | Replication Slot 이름 |
| Type | `Physical`(WAL 로그를 그대로 저장) 또는 `Logical`(INSERT·UPDATE·DELETE 형태로 변환하여 저장) |
| Scope | `Permanent`(영구 유지) 또는 `Temporary`(세션 종료 시 자동 삭제) |
| Status | `Connected`(Replication Client 연결 중) 또는 `Disconnected`(연결된 Client 없음) |
| Backlog(MB) | 아직 소비되지 않고 보존 중인 WAL 데이터 용량 |

OwlDB 콘솔에서는 **Permanent** Slot만 생성할 수 있으며, Temporary 타입 Slot은 조회만 가능하고 선택하거나 삭제할 수 없습니다.

{% hint style="warning" %}
**주의**\
OwlDB 관리 범위를 벗어나 PostgreSQL에 직접 생성하거나 삭제한 Replication Slot은 OwlDB 콘솔에 반영되지 않습니다. 이 경우 Slot 상태 조회, 삭제, 장애 대응 등 관련 관리 기능이 정상적으로 동작하지 않을 수 있습니다. Replication Slot 생성 및 삭제는 반드시 OwlDB 콘솔에서 수행합니다.
{% endhint %}

### Replication Slot 조회


1. **관리 > 연결 정보 관리**에서 **Replication Slot** 탭을 클릭합니다.
2. 현재 Primary 인스턴스에 생성된 Replication Slot 목록을 확인합니다.
3. **Type**(Physical / Logical) 또는 **Status**(Connected / Disconnected) 필터를 사용하여 목록을 좁힙니다.

### Replication Slot 생성


1. **\[생성\]** 버튼을 클릭합니다.
2. 오른쪽 드로어에서 아래 항목을 입력합니다.

| 항목  | 설명  | 입력 규칙 |
|-----|-----|-------|
| 이름 \* | Replication Slot의 고유 이름 | 30자 이내 영어 소문자(a-z)·숫자(0-9)·언더바(`_`) 사용 가능. 공백 및 탭 입력 불가. 중복 불가. |
| Type \* | Slot 유형 | Physical 또는 Logical 중 선택. 기본값: Physical |
| Scope | 운영 관리 대상 | Permanent로 고정. Temporary Slot은 생성할 수 없습니다. |

\* 필수 항목


3. 항목을 입력한 후 **생성** 버튼을 클릭합니다.

{% hint style="info" %}
**참고**\
DB 서비스 상태가 `Updating` 또는 `Failover`인 경우 **생성** 버튼이 비활성화됩니다.
{% endhint %}

### Replication Slot 삭제


1. 삭제할 Slot을 선택합니다.
2. **삭제** 버튼을 클릭합니다.
3. 삭제 확인 모달에서 내용을 확인한 후 **삭제** 버튼을 클릭합니다.

{% hint style="info" %}
**참고**\
Status가 `Connected`인 Slot과 DB 서비스 상태가 `Updating` 또는 `Failover`인 경우에는 **\[삭제\]** 버튼이 비활성화됩니다.
{% endhint %}
