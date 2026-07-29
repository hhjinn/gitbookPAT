# 스펙 변경

운영 중인 데이터베이스의 노드 구성과 DR 구성을 변경하는 기능입니다.

스펙 변경은 **Overview** 또는 **변경 감지 DB**에서 시작하며, 엔진 옵션 → DR 구성 → 인스턴스 구성 → 데이터베이스 구성 → 구성 정보 확인의 5단계로 진행됩니다. 각 단계에서 현재 설정을 확인하고 필요한 항목을 수정한 뒤, 마지막 단계에서 변경 내용을 최종 확인하고 적용합니다.

스펙 변경은 데이터베이스 운영 상태가 `Running` 또는 `Degraded`일 때 가능합니다. 변경 가능한 항목의 범위는 DB 엔진(Tibero/OpenSQL)과 설치 방식(설치 DB/등록 DB)에 따라 다릅니다.

{% hint style="info" %}
**참고**
* `Degraded` 상태에서 진행 중이거나 사용 불가능한 노드가 하나라도 있는 경우 스펙 변경 버튼을 사용할 수 없습니다.
* `Degraded` 상태이면서 Failover된 Primary로 인한 이슈만 존재하는 경우에는 스펙 변경 버튼을 사용할 수 있습니다.
{% endhint %}

## 스펙 변경 진입 방법

스펙 변경 페이지는 다음 두 가지 방법으로 진입할 수 있습니다.

* **Overview에서 진입** : Overview 페이지 > 작업 > **스펙 변경** 클릭
* **변경 감지 DB에서 진입** : 대시보드 > 탐색 > 변경 감지 DB > **스펙 변경** 클릭

## 스펙 변경

Overview에서 진입한 경우 다음 순서로 진행합니다.


1. Overview 페이지의 **작업 > 스펙 변경**을 클릭하여 스펙 변경 페이지에 진입합니다.
2. **엔진 옵션** 단계에서 노드 구성을 확인하고, 필요 시 Scale In/Out을 설정합니다.
3. **DR 구성** 단계에서 DR 사용 여부와 장애 조치 자동화 레벨, Standby 노드 설정을 조정합니다.
4. **인스턴스 구성** 단계에서 노드별 정보를 확인하고, 필요 시 Backup Path를 수정합니다.
5. **데이터베이스 구성** 단계에서 추가된 노드의 VIP 등 구성 정보를 입력합니다.
6. **구성 정보 확인** 단계에서 변경 내용을 검토한 후 **완료**를 클릭합니다.

## Overview에서 스펙 변경 진입

Overview 페이지의 작업 메뉴를 통해 진입한 경우, DB 노드 구성 변경과 DR 수정 두 가지 작업을 수행할 수 있습니다.

### DB 노드 구성 변경

{% tabs %}
{% tab title="Tibero" %}

* **설치 DB**는 표준 아키텍처의 토폴로지 범위 내에서 Primary 노드의 Scale In/Out이 가능합니다. 예를 들어 TAC 구성은 Primary 노드를 최소 2개에서 최대 4개까지 조정할 수 있습니다.
* **등록 DB**는 Scale In/Out을 지원하지 않습니다.
* Scale Out을 진행한 경우, VIP를 사용 중인 DB라면 **데이터베이스 구성** 단계에서 추가된 노드의 VIP를 입력해야 합니다.

{% hint style="info" %}
**참고**
* 토폴로지 자체(예: Single → TAC)를 변경하는 것은 스펙 변경에서 지원하지 않습니다.
* Scale In/Out은 신규 설치와 동일하게 사전 환경 구성이 완료된 노드에 대해서만 가능합니다.
{% endhint %}
{% endtab %}

{% tab title="OpenSQL" %}

* **설치 DB**는 Single ↔ HA 토폴로지 전환이 가능하며, HA 토폴로지에서는 Standby 노드의 Scale In/Out도 지원합니다. 조정 가능한 노드 수는 Single 1개, HA 2\~3개입니다.
* **등록 DB**는 토폴로지 변경과 Scale In/Out을 지원하지 않습니다.
* 토폴로지 전환 또는 Scale Out을 진행한 경우, VIP를 사용 중인 DB라면 **데이터베이스 구성** 단계에서 추가된 노드의 VIP를 입력해야 합니다.
{% endtab %}
{% endtabs %}

### DR 수정

{% tabs %}
{% tab title="Tibero" %}

* **설치 DB**는 DR 사용 ↔ 미사용을 변경할 수 있으며, 장애 조치 자동화 레벨도 변경할 수 있습니다. DR을 사용 중인 경우 Standby 노드의 Open Mode와 Log Replication Type도 변경할 수 있습니다.
* **등록 DB**는 DR 사용 여부 변경을 지원하지 않으며, DR을 사용 중인 경우 장애 조치 자동화 레벨과 Standby 노드 설정만 변경할 수 있습니다.
{% endtab %}

{% tab title="OpenSQL" %}

* DR 사용 여부는 **엔진 옵션** 단계에서 선택한 토폴로지에 따라 자동으로 결정됩니다. Single → HA로 전환하면 DR이 자동으로 사용 설정되고, HA → Single로 전환하면 DR이 자동으로 미사용 설정됩니다.
* **설치 DB**는 위와 같이 토폴로지 전환을 통해서만 DR 사용 여부가 변경되며, 장애 조치 자동화 레벨은 별도로 변경할 수 있습니다.
* **등록 DB**는 DR 사용 여부 변경을 지원하지 않으며, 장애 조치 자동화 레벨과 Standby 노드 설정만 변경할 수 있습니다.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**참고**
지원하는 장애 조치 자동화 레벨은 [**장애 조치 자동화 문서**](#GDPaQdLZBmgq4vRqB2Sz)를 참고하십시오.
{% endhint %}

## 스펙 변경 단계

각 단계에서 설정할 수 있는 항목은 엔진에 따라 다릅니다.

{% tabs %}
{% tab title="엔진 옵션" %}
데이터베이스 별칭과 엔진, 토폴로지 정보를 확인하는 단계입니다. 대부분의 항목은 변경할 수 없으며, 노드 구성만 설치 DB에 한하여 조정할 수 있습니다.

**Tibero**

| 항목  | 설명  | 변경 여부 |
|-----|-----|-------|
| Database Alias | 데이터베이스를 식별하기 위한 별칭 | 변경 불가 |
| Database Engine Type | Tibero | 변경 불가 |
| Topology | Single, TAC | 변경 불가 |
| Node Count | Single 1개, TAC 2\~4개 | 변경 불가 (하단 Scale In/Out 결과 반영) |
| Scale In/Out | Primary 노드 목록 | 설치 DB에 한해 사용 가능 |

**OpenSQL**

| 항목  | 설명  | 변경 여부 |
|-----|-----|-------|
| Database Alias | 데이터베이스를 식별하기 위한 별칭 | 변경 불가 |
| Database Engine Type | OpenSQL | 변경 불가 |
| Topology | Single, HA | 설치 DB는 Single ↔ HA 전환 가능 |
| Node Count | Single 1개, HA 2\~3개 | 변경 불가 (토폴로지 전환/Scale 결과 반영) |
| PostgreSQL Version | 사용할 PostgreSQL 버전 | 변경 불가 |

**Scale In/Out (Tibero TAC 설치 DB)**

설치된 Tibero TAC 토폴로지 DB의 경우, Node Count 하단에 Scale In/Out 항목이 표시됩니다. 현재 구성된 Primary 노드 목록을 확인하고 다음 작업을 수행할 수 있습니다.

* **추가** : 추가 버튼을 클릭하면 설치 가능한 호스트 목록에서 인스턴스를 선택할 수 있습니다. 선택한 호스트는 테이블에 추가되며 Scale Out 대상이 됩니다.
* **삭제** : 테이블에서 제거할 노드를 선택한 후 삭제 버튼을 클릭합니다. 선택한 노드는 Scale In 대상이 되며 테이블에서 흐리게 표시됩니다.
* **초기화** : 추가 또는 삭제한 변경 사항을 초기 상태로 되돌립니다.

설치된 OpenSQL DB에서 Single → HA로 토폴로지를 전환하면 DR 구성이 자동으로 사용 설정되며, Standby 노드 구성은 **DR 구성** 단계에서 확인 및 조정합니다.
{% endtab %}

{% tab title="DR 구성" %}
DR 사용 여부와 장애 조치 자동화 레벨, Standby 노드 설정을 변경하는 단계입니다. 엔진에 따라 변경 방식이 다릅니다.

**Tibero**

| 항목  | 설명  |
|-----|-----|
| Enable DR | DR 구성 사용 여부. 설치 DB에 한해 직접 변경할 수 있습니다. |
| Failover Automation Level | 0단계(수동), 1단계(자동 장애 조치), 3단계(완전 자동화) 지원.<br>2단계(자동 구성 복구)는 OwlDB v1.3에서 미지원 |
| Standby Count | 설치 : 1개 고정 / 등록 : 1\~9개 |
| Standby Mode | Standby 노드의 Open Mode 선택 (Recovery / Read Only) |
| Log Replication Type | Standby 노드의 로그 전송 방식 선택 (LGWR ASYNC / ARCH ASYNC) |

DR 사용 여부는 설치 DB에서만 직접 변경할 수 있으며, 변경 방향에 따라 다음과 같이 동작합니다.

| 변경 방향 | 동작  |
|-------|-----|
| 미사용 → 사용 | Standby 노드를 신규 구축하고, 장애 조치 자동화 레벨을 기본 1단계로 설정합니다. |
| 사용 → 미사용 | Standby 노드 clean up을 진행합니다. |

**OpenSQL**

| 항목  | 설명  |
|-----|-----|
| Enable DR | **엔진 옵션** 단계의 토폴로지 선택에 따라 자동 결정 (HA → 사용, Single → 미사용) |
| Failover Automation Level | 0단계(수동), 3단계(완전 자동화) 지원 |
| Standby Count | 1\~2개 |
| Standby Scale In/Out | 설치 DB는 추가/삭제 버튼으로 직접 조정 가능 |
| Log Replication Type | 비동기(ASYNC) 복제 방식으로 구성 (고정) |

**Standby 노드 설정** (DR 사용 시 노출)

현재 구성된 Standby 노드 목록이 표시되며, 다음 항목을 변경할 수 있습니다. Log Replication Type의 세부 옵션은 다음과 같습니다.

| 항목  | 설명  |
|-----|-----|
| Open Mode | Standby 노드의 Open Mode를 `Recovery` 또는 `Read Only` 중에서 선택합니다. (Tibero 해당) |
| Log Replication Type | Standby 노드의 로그 복제 방식을 선택합니다. (Tibero 해당)<br>-**LGWR ASYNC**: 트랜잭션 발생 시 실시간으로 생성되는 Redo log를 전송하는 복제 모드<br>-**ARCH ASYNC** : 로그 스위치 이후 생성된 아카이브 로그 파일을 모아서 전송하는 복제 모드 |

{% hint style="info" %}
**참고**
OpenSQL의 Standby 노드는 비동기(ASYNC) 복제 방식으로 고정 구성되며, Open Mode 및 Log Replication Type을 선택할 수 없습니다.
{% endhint %}
{% endtab %}

{% tab title="인스턴스 구성" %}
Primary / Standby 노드별 구성 정보를 확인하는 단계입니다. 토폴로지와 설치/등록 방식에 따라 조회되지 않는 항목이 있으며, **Backup Path**만 수정할 수 있습니다.

#### **Tibero**

| 항목  | 설명  | 비고  |
|-----|-----|-----|
| Hostname | 연결된 호스트 정보 | 수정 불가 |
| Service IP | OwlDB와 노드 간 통신에 사용할 IP | 수정 불가 |
| Service Port | OwlDB와 데이터베이스 서버 간 통신에 사용할 Port | 수정 불가 |
| Interconnect IP | 클러스터 내부 노드 간 통신에 사용할 Interconnect IP | 수정 불가 |
| Primary Destination IP | Standby DB에서 Primary DB로의 통신에 사용할 IP (Primary 인스턴스만 입력) | 수정 불가 |
| Primary Destination Port | Standby DB에서 Primary DB로의 통신에 사용할 Port (Primary 인스턴스만 입력) | 수정 불가 |
| Standby Destination IP | Primary DB에서 Standby DB로의 통신에 사용할 IP (Standby 인스턴스만 입력) | 수정 불가 |
| Standby Destination Port | Primary DB에서 Standby DB로의 통신에 사용할 Port (Standby 인스턴스만 입력) | 수정 불가 |
| Data Path | 데이터 Path | 수정 불가 |
| Redo Path | Redo Path | 수정 불가 |
| Archive Path | Archive Path | 수정 불가 |
| Backup Path | Backup Path | 파일 시스템 경로만 입력 가능 |
| SSH Port | SSH Port | 수정 불가 |
| SSH User | SSH User | 수정 불가 |
| SSH Key File Path | SSH Key File Path | 수정 불가 |

**Backup Path**는 파일 시스템 경로만 입력할 수 있으며, TAC 구성인 경우 **공유 볼륨**으로 구성되어 있어야 합니다.

#### OpenSQL

| 항목  | 설명  | 비고  |
|-----|-----|-----|
| Hostname | 연결된 호스트 정보 | 수정 불가 |
| Service IP | OwlDB와 노드 간 통신에 사용할 IP | 수정 불가 |
| Service Port | OwlDB와 데이터베이스 서버 간 통신에 사용할 Port | 수정 불가 |
| Replication Connection Ip | HA 구성일 때, 복제 연결에 사용할 IP | 수정 불가 |
| 네트워크 인터페이스 | 네트워크 인터페이스 | 수정 불가 |
| Data Path | 데이터 Path | 수정 불가 |
| SSH Port | SSH Port | 수정 불가 |
| SSH User | SSH User | 수정 불가 |
| SSH Key File Path | SSH Key File Path | 수정 불가 |

앞 단계에서 설정한 구성에 따라 Primary / Standby 노드별 구성 정보가 표시됩니다. Scale Out 시 추가된 노드의 입력 필드가 새로 생성되며, Scale In 시 해당 노드는 화면에서 제거됩니다.
{% endtab %}

{% tab title="데이터베이스 구성" %}
데이터베이스 구성 정보를 확인하는 단계입니다. 대부분의 항목은 OwlDB 메타데이터 및 실제 DB 탐색 데이터를 통해 자동으로 설정됩니다.

**공통 항목**

| 항목  | 설명  |
|-----|-----|
| Database Name | 사용할 데이터베이스의 이름 |
| SYS User Password | 데이터베이스 최고 권한 관리자 계정(SYS 유저) 비밀번호 |
| Target Memory Size | 대상 메모리 사이즈 |
| Character Set | 데이터베이스에 사용할 문자 인코딩 |
| Timezone | 데이터베이스가 설치될 OS 시간대 |
| Database Listener Port | 네트워크 통신을 위한 데이터베이스 리스너 포트 |
| Max Session Count | 동시 허용 최대 세션 수 |

**Tibero 전용 항목**

| 항목  | 설명  |
|-----|-----|
| VIP | VIP 사용 여부 선택 |
| Primary Node #N VIP | 데이터베이스 가상 IP (VIP 사용 선택 시 활성화) |
| Shared Memory Size (MB) | 공유 메모리 크기 |
| Redo Log File Size (MB) | Redo 로그 파일 크기 |
| System Data File Size (MB) | 시스템 테이블 및 주요 메타 데이터를 저장할 데이터 파일 크기 |
| Syssub Data File Size (MB) | 시스템 운영 관련 데이터 저장을 위한 서브 데이터 파일 크기 |
| User Tablespace Data File Size (MB) | 사용자 데이터를 저장할 테이블스페이스 데이터 파일 크기 |
| Temporary Tablespace Data File Size (MB) | 대용량 연산에 사용되는 임시 테이블스페이스 데이터 파일 크기 |
| Undo Tablespace Data File Size (MB) | Undo 테이블스페이스 크기 |

**OpenSQL 전용 항목**

| 항목  | 설명  |
|-----|-----|
| VIP | 데이터베이스 가상 IP (네트워크 인터페이스 선택 시 활성화) |
| Shared Buffers (%) | 공유 메모리 크기 |
| WAL File Size (MB) | WAL 파일 크기 |
| Connection Pooler Port | OpenProxy가 클라이언트 접속을 받는 포트 |

VIP를 사용 중인 DB에서 Scale Out이 발생한 경우, 추가된 노드에 대한 VIP를 이 단계에서 입력해야 합니다.
{% endtab %}

{% tab title="구성 정보 확인" %}
앞 단계에서 설정한 내용을 최종 확인합니다. 변경된 항목은 파란색으로 표시되며, 각 항목은 아코디언으로 확장/축소할 수 있습니다. **완료**를 클릭하면 스펙 변경이 반영됩니다.

{% hint style="warning" %}
**주의**
* 변경된 내용이 없는 경우 완료 버튼이 비활성화됩니다.
* Standby에 Retired 인스턴스가 존재하는 상태에서 DR 구성을 미사용으로 변경하면 확인 모달이 노출됩니다. 확인 시 Retired 인스턴스가 전체 삭제되며 DR 구성이 미사용으로 변경됩니다.
{% endhint %}

{% hint style="info" %}
**참고** **완료** 클릭 시 라이선스 파일 유무, CP Max Core 수, 라이선스 옵션 일치 여부를 확인하며, 다음의 경우 스펙 변경이 진행되지 않고 오류 메시지가 표시됩니다.

* 노드에 라이선스 파일이 존재하지 않는 경우 : 라이선스 파일 배치 후 다시 시도합니다.
* 요청한 Core 수가 라이선스 최대 Core 수를 초과하는 경우 : Core 수를 조정한 후 다시 시도합니다.
* 요청한 구성이 현재 라이선스 옵션과 일치하지 않는 경우 : 라이선스 옵션에 맞는 구성으로 다시 선택합니다.
* 동시에 다수의 스펙 변경 요청이 발생해 처리가 지연되는 경우 : 잠시 후 다시 시도합니다.
{% endhint %}
{% endtab %}
{% endtabs %}

## 변경 감지 DB에서 스펙 변경 진입

등록 DB에 한하여, OwlDB 외부에서 DB 구성이 변경된 경우 OwlDB가 이를 자동으로 감지합니다. 대시보드 탐색에서 변경이 감지된 DB를 확인하고 **스펙 변경**을 클릭하여 진입할 수 있습니다.

이 방법으로 진입하면 외부에서 변경된 내역이 스펙 변경 페이지에 자동으로 반영되어 있습니다. 변경된 항목은 별도로 표시되며, 사용자는 내용을 확인한 후 OwlDB에 반영할 수 있습니다.

**예시** VIP를 사용 중인 TAC DR DB(Primary 2노드 / Standby 1노드)를 외부에서 Primary 노드 4개로 늘린 경우, 각 단계에 다음과 같이 반영됩니다.


1. **엔진 옵션** : 추가된 2개 노드가 목록에 표시됩니다.
2. **인스턴스 구성** : 추가된 2개 노드의 구성 정보를 입력하는 필드가 생성됩니다.
3. **데이터베이스 구성** : 추가된 2개 노드의 VIP를 입력하는 필드가 생성됩니다.

각 단계에서 내용을 확인하고 입력을 완료한 후 **완료**를 클릭하면 변경 사항이 OwlDB에 반영됩니다.

## 스펙 변경 최대 처리 시간

스펙 변경 처리 시간은 데이터베이스의 용량, 부하 상태 및 구성 환경에 따라 달라질 수 있습니다. 일반적인 환경에서는 수 분 내 처리되며, 조건에 따라 다음과 같이 최대 처리 시간이 상이할 수 있습니다.

{% tabs %}
{% tab title="Primary 생성" %}

| 로직  | 최대 처리 시간 | 비고  |
|-----|----------|-----|
| Database 구성 | 6시간      | 스크립트 수행 |
|
{% endtab %}
|          |     |

{% tab title="Primary 제거" %}

| 로직  | 최대 처리 시간 | 비고  |
|-----|----------|-----|
| Database 중지 및 클러스터 리소스 제거 | 30분      |     |
| 환경 정리 | 5분       |     |
|
{% endtab %}
|          |     |

{% tab title="Standby 생성" %}

| 로직  | 최대 처리 시간 | 비고  |
|-----|----------|-----|
| Single Database에 CM 생성 | 30분      | Database 중지 |
|     | 6시간      | 클러스터 리소스 추가 |
|     | 30분      | Database 기동 |
| Primary node 백업 | 6시간      |     |
| Standby node 구성 | 6시간      |     |
| Primary node 파라미터 변경 | 30분      | Database 중지, tip에 파라미터 추가 |
|     | 30분      | Database 기동 |
|
{% endtab %}
|          |     |

{% tab title="Standby 제거" %}

| 로직  | 최대 처리 시간 | 비고  |
|-----|----------|-----|
| Database 중지 및 클러스터 리소스 제거 | 30분      |     |
| 환경 정리 | 5분       |     |
|
{% endtab %}
|          |     |
|
{% endtabs %}
|          |     |
