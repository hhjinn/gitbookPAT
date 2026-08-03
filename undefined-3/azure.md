# 데이터베이스 생성 (Azure)

OwlDB에서 데이터베이스를 운영하기 위해 데이터베이스를 생성합니다. 데이터베이스 생성(이하 프로비저닝)이 완료되면 OwlDB에서 제공하는 모든 기능을 사용할 수 있습니다. 프로비저닝 진행 상태는 콘솔 화면 우측 상단 :1f514:아이콘을 클릭하여 확인할 수 있습니다. OwlDB에서 지원하는 데이터베이스 엔진 및 인스턴스 타입에 대한 내용은 '[서비스 개요](#XDj4D6jZeLIG3hl9e9W4)' 페이지를 참고하시기 바랍니다.

{% hint style="info" %}
**참고** DB 서비스 생성은 **Root 권한을 가진 사용자만** 수행할 수 있습니다.
{% endhint %}

# 새로운 DB 서비스 생성

1. **OwlDB 콘솔 화면** > **대시보드** 메뉴로 이동합니다.
2. **생성** 버튼을 클릭합니다.
3. 생성할 데이터베이스의 스펙을 선택하고, 정보를 입력합니다.
4. 페이지 우측에서 각 단계에서 입력한 정보를 요약해서 확인할 수 있습니다.
5. **구성 정보 확인** 단계에서 전체 생성 정보를 검토합니다.
6. License Option이 **LI**인 경우 **생성** 버튼을, **BYOL**인 경우 **라이선스 등록** 버튼을 클릭하여 프로비저닝을 시작합니다. BYOL의 라이선스 등록 절차는 [BYOL 라이선스 등록](#byol-라이선스-등록)을 참고하시기 바랍니다.

{% hint style="info" %}
**참고**

- **OwlDB 콘솔 화면 > 대시보드 > 카드뷰 > + 아이콘** 또는 **GNB > DB Alias 드롭다운 > DB Service 생성 버튼**을 클릭하여 데이터베이스 생성 페이지로 이동할 수 있습니다.
- Azure 환경에서는 라이선스 옵션이 BYOL(Bring Your Own License)로 고정되어 있어, 데이터베이스 생성을 완료하려면 보유한 라이선스 파일을 등록해야 합니다. 자세한 내용은 아래 '라이선스 등록' 문단을 참고하시기 바랍니다.
{% endhint %}

---

# **생성 옵션**

데이터베이스 생성 시 선택한 옵션에 따른 예상 금액을 확인할 수 있습니다. 해당 금액은 서울 리전을 기준으로 산정된 값이며, 실제 금액은 리전이나 실사용량 등 여러 요소에 따라 달라질 수 있습니다.

### 1단계: 엔진 옵션

| 항목 | 설명 |
| --- | --- |
| DB Service Name* | 데이터베이스 서비스를 식별하기 위한 이름- OwlDB 게정 내 중복 사용 불가<br>- 6~30자 이내, 영어 대소문자(a-z, A-Z), 숫자(0-9), 하이픈(-)만 사용 가능, 공백 사용 불가 |
| Database Engine Type* | 사용할 데이터베이스 엔진<br>-**Tibero**<br>-**OpenSQL** |
| License Option* | 사용할 라이선스 옵션<br>-**LI** (License Included)<br>-**BYOL** (Bring Your Own License) |
| Topology* | 데이터베이스 구조를 결정할 토폴로지 유형<br>-**Tibero**: Single, TAC<br>-**OpenSQL**: Single, HA |
| Edition* | 라이선스의 에디션<br>-**Standard Edition (SE)**: 단일 서버 구성 전용, 최대 8vCPU까지 사용 가능<br>-**Enterprise Edition (EE)**: 고가용성 및 대규모 구성 지원, vCPU 제한 없음<br>- Topology를 TAC 또는 HA로 선택하면 Enterprise Edition으로 자동 적용되며 변경할 수 없음 |
| Node Count* | 클러스터 구성 노드 수<br>-**Tibero**: Single 1개 (고정), TAC 2~4개 중 선택<br>-**OpenSQL**: Single, HA 모두 1개로 고정 |
| PostgreSQL Version | OpenSQL 선택 시 사용할 PostgreSQL 버전<br>- 3.16.12.5 (기본값)<br>- 3.17.8.5 |

{% hint style="info" %}
**참고**

- Azure 환경에서는 License Option이 BYOL로 고정되며, 데이터베이스 생성을 완료하려면 보유한 라이선스 파일을 등록해야 합니다.
- Edition에서 Standard Edition(SE)을 선택하면 인스턴스 구성 단계에서 최대 8vCPU까지의 인스턴스 유형만 선택할 수 있습니다.
{% endhint %}

### 2단계: DR 구성

| 항목 | 설명 |
| --- | --- |
| Enable DR* | DR 구성 사용 여부<br>-**Tibero**: 사용자가 직접 선택<br>-**OpenSQL**: Topology에 따라 자동으로 결정되며 수정할 수 없음 (Single: DR 미사용 / HA: DR 사용) |
| Failover Automation Level* | 장애 조치 자동화 레벨<br>-**0단계 : 수동**<br>-**1단계 : 자동 장애 조치**<br>-**2단계 : 자동 구성 복구**<br>-**3단계 : 완전 자동화** |
| Standby/Replica Count* | Standby(또는 Replica) DB 개수<br>-**Tibero**: 최대 2개까지 선택 가능<br>-**OpenSQL**: 1개로 고정 |
| Standby Mode* | Standby Mode 옵션 (Tibero 엔진에서만 노출되며, Standby 노드별로 개별 설정 가능)<br>-**Recovery**<br>-**Read Only** |
| Log Replication Type | Primary(Leader)의 로그를 Standby(Replica)에 전송하는 방식<br>-**LGWR ASYNC**: 트랜잭션이 발생하면 실시간으로 생성되는 Redo log를 곧바로 전송하는 복제 모드<br>-**ARCH ASYNC**: 로그 스위치가 일어난 뒤, 아카이브 로그 파일이 생성되면 그 파일을 모아서 전송하는 복제 모드<br>- OpenSQL 엔진은 **ASYNC 방식**으로 고정되며 수정할 수 없음. |

{% hint style="info" %}
**참고**

- Failover Automation Level, Standby/Replica Count, Standby Mode, Log Replication Type은 Enable DR에서 사용을 선택한 경우에만 노출됩니다.
- Azure 환경은 License Option이 BYOL로 고정되어 있어 Failover Automation Level은 0, 2, 3단계 중에서 선택할 수 있습니다. (1단계는 지원되지 않음)
- OpenSQL 엔진은 Failover Automation Level에서 0단계 또는 3단계만 선택할 수 있습니다.
{% endhint %}

### 3단계: AZ 구성

| 항목 | 설명 |
| --- | --- |
| OwlDB Availability Zone(AZ)* (disabled) | OwlDB의 가용 영역 |
| Primary(Leader) DB Availability Zone(AZ)* | Primary(Leader) DB의 가용 영역<br>**기본값**<br>• DR 사용 안함 : OwlDB와 같은 영역<br>• DR 사용 : OwlDB와 다른 영역 |
| Standby(Replica) DB Availability Zone(AZ)* | Standby(Replica) DB의 가용 영역<br>• 기본값 : OwlDB와 같은 가용 영역에 배치, 이후 다른 영역에 자동 배치 |

{% hint style="info" %}
**참고**

- Standby(Replica) DB Availability Zone은 Enable DR에서 사용을 선택한 경우에만 노출됩니다. Primary(Leader) DB의 가용 영역은 사용자가 선택할 수 있으나, 안정적인 장애 대응 및 Failover를 위해 Primary(Leader) DB는 OwlDB와 다른 가용 영역에 배치하는 것을 권장합니다.
{% endhint %}

### 4단계: 인스턴스 구성

{% tabs %}
{% tab title="Tibero" %}
| 구분 | 항목 | 설명 |
| --- | --- | --- |
| Instance Setting | DB Virtual Machine Size* | 성능과 사양을 결정할 인스턴스 유형 |
| Instance Access Setting | DB Instance SSH Key Name* | DB 인스턴스에 접근하기 위한 설정 |
| Data Disk | Data Disk Type* | 주요 데이터를 저장할 디스크의 유형 |
|   | Data Disk Size* | 주요 데이터를 저장할 디스크의 크기 |
|   | Data Disk IOPS* | 주요 데이터를 저장할 디스크의 입출력 처리량 |
|   | Data Disk MBps | 주요 데이터를 저장할 디스크의 최대 처리 속도 |
| Redo Log Disk | Redo Log Disk Type | Redo log를 저장할 디스크의 유형 |
|   | Redo Log Disk Size (disabled) | Redo log를 저장할 디스크의 크기<br>• 입력한 Redo Log File Size(MB)에 따라 자동 계산 |
|   | Redo Log Disk IOPS | Redo log를 저장할 디스크의 입출력 처리량 |
|   | Redo Log Disk MBps | Redo log를 저장할 디스크의 최대 처리 속도 |
| Archive Log Volume | Archive Log Disk Type | Archive log를 저장할 디스크의 유형 |
|   | Archive Log Disk Size | Archive log를 저장할 디스크의 크기 |
|   | Archive Log Disk IOPS | Archive log를 저장할 디스크의 입출력 처리량 |
|   | Archive Log Disk MBps | Archive log를 저장할 디스크의 최대 처리 속도 |
| Auto Scale | 사용 여부* | 데이터 볼륨 사용량에 따라 데이터 디스크 크기를 자동으로 확장할지 여부 |
|   | 최대 확장 한도* | Auto Scale 사용 시 증가할 수 있는 데이터 디스크의 최대 크기 |
{% endtab %}
{% tab title="Tab" %}
| | |
{% endtab %}
{% tab title="OpenSQL" %}
| 구분 | 항목 | 설명 |
| --- | --- | --- |
| Instance Setting | DB Virtual Machine Size* | 성능과 사양을 결정할 인스턴스 유형 |
| Instance Access Setting | DB Instance SSH Key Name* | DB 인스턴스에 접근하기 위한 설정 |
| Disk | Disk Type* | 주요 데이터를 저장할 디스크의 유형 |
|   | Disk Size* | 주요 데이터를 저장할 디스크의 크기 |
|   | Disk IOPS* | 주요 데이터를 저장할 디스크의 입출력 처리량 |
|   | Disk MBps | 주요 데이터를 저장할 디스크의 최대 처리 속도 |
| Auto Scale | 사용 여부* | 데이터 볼륨 사용량에 따라 데이터 디스크 크기를 자동으로 확장할지 여부 |
|   | 최대 확장 한도* | Auto Scale 사용 시 증가할 수 있는 데이터 디스크의 최대 크기 |
{% endtab %}
{% tab title="Tab" %}
| | | |
{% endtab %}
{% endtabs %}

| | |

{% hint style="info" %}
**참고**

- 안정적인 운영 환경을 위해, 클러스터 내 모든 인스턴스는 동일한 스펙으로 자동 구성됩니다.
- Edition을 **Standard Edition(SE)**으로 선택한 경우, DB Instance Type 목록에는 최대 8vCPU 사양까지만 표시됩니다.
- 일부 가용 영역에서는 지원하지 않는 DB Instance Type이 있을 수 있으며, 이 경우 목록에는 표시되나 선택할 수 없습니다.
- 각 디스크의 IOPS는 선택한 디스크 유형에서 허용하는 범위 내에서만 입력할 수 있습니다.
- Redo Log Disk, Archive Log Volume은 Tibero 엔진에서만 노출됩니다.
{% endhint %}

### 5단계: 데이터베이스 구성

{% tabs %}
{% tab title="Tibero" %}
| 항목 | 설명 |
| --- | --- |
| Database Name* | 사용할 데이터베이스의 이름 |
| SYS User Password* | 데이터베이스 최고 권한 관리자 계정(SYS 유저)의 비밀번호 |
| Character Set* | 데이터베이스에 사용할 문자 인코딩 |
| Timezone* | 데이터베이스가 설치될 OS 시간대 |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 |
| Target Memory Ratio | 대상 메모리 비율 |
| Shared Memory Ratio | 공유 메모리 비율 |
| Redo Log File Size (GB) | Redo 로그 파일 크기 |
| System Data File Size (GB) | 시스템 테이블 및 주요 메타 데이터를 저장할 데이터 파일의 크기 |
| Syssub Data File Size (GB) | 시스템 운영 관련 데이터 저장을 위한 서브 데이터 파일 크기 |
| User Tablespace Data File Size (GB) | 사용자 데이터를 저장할 테이블 스페이스 데이터 파일 크기 |
| Temporary Tablespace Data File Size (GB) | 대용량 연산에 사용되는 임시 테이블스페이스 데이터 파일 크기 |
| Undo Tablespace Data File Size (GB) | Undo 테이블스페이스 크기 |
{% endtab %}
{% tab title="Tab" %}
| |
{% endtab %}
{% tab title="OpenSQL" %}
| 항목 | 설명 |
| --- | --- |
| Database Name* | 사용할 데이터베이스의 이름 |
| Postgres User Password* | 데이터베이스 최고 권한 관리자 계정(Postgres 유저)의 비밀번호 |
| Character Set* | 데이터베이스에 사용할 문자 인코딩 |
| Timezone* | 데이터베이스가 설치될 OS 시간대 |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 |
| Shared Buffers | Shared Buffers를 표시 → 권장값으로 계산하여 표시 |
| Wal File Size | WAL File 한 개 사이즈를 설정 |
| Connection Pooler Port | OpenProxy가 클라이언트 접속을 받는 포트 |
| Extensions | Extension을 선택 |
{% endtab %}
{% tab title="Tab" %}
| | |
{% endtab %}
{% endtabs %}

| |

{% hint style="warning" %}
**주의** Database Name, Character Set, Timezone, Database Listener Port는 최초 설정 이후 수정이 불가합니다.
{% endhint %}

****표기는 필수 입력 항목을 의미합니다.*&nbsp;&nbsp;&nbsp;&nbsp;

## 

---

# **BYOL 라이선스 등록**

Azure 환경에서는 라이선스 옵션이 BYOL(Bring Your Own License)로 고정되어 있어, 데이터베이스를 생성하려면 보유한 라이선스 파일을 등록해야 합니다.

1. **구성 정보 확인** 단계에서 입력한 정보를 검토한 후, **라이선스 등록** 버튼을 클릭합니다.
2. 라이선스 등록 창에서 **올리기** 버튼을 클릭하거나 파일을 드래그 앤 드롭하여 보유한 라이선스 파일을 업로드합니다.
3. 업로드한 라이선스 파일 목록에서 정보를 확인합니다.
4. 검증할 파일을 **선택**한 후, **검증** 버튼을 클릭하여 업로드한 라이선스 파일의 유효성을 확인합니다.
5. 검증에 성공하면 **생성** 버튼을 클릭하여 데이터베이스 생성을 요청합니다.

### 업로드 항목

| 항목 | 설명 |
| --- | --- |
| 라이선스 파일 | 업로드한 라이선스 파일명 |
| Edition | 라이선스 파일에 기재된 Edition 정보 |
| CSP | 라이선스 파일에 기재된 CSP 정보 |
| Limit CPU | 라이선스 파일에 기재된 CPU 제한 값 (vCPU 기준) |
| Expired Date | 라이선스 만료일 |
| Signature | 라이선스 시그니처 정보 |

{% hint style="info" %}
**참고**

- 라이선스 파일은 최대 6개까지 업로드할 수 있습니다.
- 업로드한 라이선스 파일을 삭제하려면 목록에서 파일을 선택한 후 **삭제** 버튼을 클릭합니다.
{% endhint %}

{% hint style="warning" %}
**주의** 다음의 경우 라이선스 검증에 실패합니다.

- 라이선스 파일의 만료일이 지난 경우
- 업로드한 라이선스 파일 간 Signature 값이 중복되는 경우
- 이미 등록한 라이선스 파일을 다시 업로드한 경우
- 업로드한 라이선스 파일 간 정보(Edition, CSP, Limit CPU, Expired Date)가 서로 다른 경우
- 선택한 데이터베이스 구성 정보(Edition, CSP, 노드 수, 인스턴스 타입의 vCPU)와 라이선스 파일의 정보가 일치하지 않는 경우
{% endhint %}

## 

---

# **생성 결과 확인**

데이터베이스 생성 요청이 접수되면 시스템 알림을 통해 진행 상태를 확인할 수 있습니다.

- 데이터베이스 생성이 시작되면 **DB 서비스 생성 시작** 알림이 발송됩니다.
- 생성이 정상적으로 완료되면 **DB 서비스 생성 완료** 알림이 발송됩니다.
- 서버 환경 구성 또는 데이터베이스 설치 과정에서 오류가 발생하면 **DB 서비스 생성 실패** 알림이 발송되며, 안내에 따라 다시 시도할 수 있습니다.

{% hint style="info" %}
**참고** OpenSQL 엔진에서 선택한 Extension 설치가 모두 또는 일부 실패한 경우에도 데이터베이스 서비스 자체는 정상적으로 생성되며, 별도의 안내 알림이 함께 발송됩니다.
{% endhint %}

##
