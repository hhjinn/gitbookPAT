OwlDB는 DR(또는 HA)로 구성된 Primary 데이터베이스의 상태를 지속적으로 모니터링하여 장애 상황을 감지합니다. 시스템은 1초마다 health check를 수행하며, Primary(Tibero) 또는 Leader(OpenSQL) DB가 `Unavailable` 상태로 30초 동안 지속되면 자동으로 장애 조치가 수행됩니다. Tibero TAC 구성에서는 모든 Primary 노드가 `Unavailable` 상태일 때 장애로 판정합니다.

{% hint style="info" %}
**참고**

이 문서에서 Tibero의 **Primary / Standby**는 OpenSQL의 **Leader / Replica**에 대응합니다. 표와 시나리오에서 공통으로 적용되는 항목은 `Primary/Leader`, `Standby/Replica`와 같이 함께 표기합니다.
{% endhint %}

# 단계별 알림 정책

| 구분 | Standby/Replica 승격 | 구성 정상화 |
| --- | --- | --- |
| 수행 시점 | 장애 감지 직후 | Standby/Replica 승격 이후 |
| 알림 | 시작 / 요청 실패 / 완료 / 실패 | 완료 / 실패 |
| 전환 이력 관리 | 승격 성공 여부를 결과 컬럼에 표시<br>• 성공 / 실패<br>• Standby/Replica가 승격하여 새로운 Primary/Leader가 된 것을 기준으로 성공 여부를 정의 | 원인/비고 컬럼에 표시<br>• 전체 성공 시 빈칸<br>•**승격 실패**: Standby Promotion Failed<br>•**승격 성공 후 구성 정상화 실패**: Cluster Normalization Failed — Primary scale out failed 또는 New standby/replica creation failed<br>(둘 다 실패하면 콤마로 표시) |

# 장애 조치 자동화 단계별 동작 요약

| 자동화 레벨 | 자동 Failover | 자동 구성 정상화 | 추가 동작 |
| --- | --- | --- | --- |
| 0단계 (**수동**) | ❌ | ❌ | 사용자가 `역할전환` 버튼으로 직접 승격 및 정상화 수행 |
| 1단계 (**자동 장애 조치**) | ✓ | △ TAC 구성은 Scale-Out으로 노드 수를 복구하지만, 신규 Standby/Replica는 생성하지 않아 DR은 복구되지 않습니다(`Degraded`). | old Primary/Leader를 `retired` 처리 |
| 2단계 (**자동 구성 복구**) | — | — | 현재 미지원 (OwlDB v1.3 기준) |
| 3단계 (**완전 자동화**) | ✓ | ✓ old Primary/Leader 역동기화 및 신규 Standby/Replica 자동 생성 | 없음 |

{% hint style="info" %}
**참고**

기존 Primary cluster가 TAC인 경우, 구성 복구를 위하여 Scale-out을 수행하여 새 Primary cluster도 TAC 구성으로 복구됩니다. (Tibero TAC 해당)
{% endhint %}

{% hint style="info" %}
**참고**

자동 장애 조치 과정에서 기존 Primary/Leader가 비정상 종료되면 Standby/Replica로 전송되지 않은 로그가 있을 수 있습니다. 데이터 일관성을 보장할 수 없고 Split-Brain 현상을 방지하기 위해 해당 인스턴스는 **retired** 상태로 처리됩니다.
{% endhint %}

{% hint style="warning" %}
**주의**

장애 조치 자동화 레벨이 **1단계**일 때 Standby가 1개만 있는 경우, 해당 Standby가 Primary로 승격되면서 장애 조치 이후 Standby가 없을 수 있습니다. 이 경우 고가용성 구성이 유지되지 않으므로 주의가 필요합니다. (Tibero 해당)
{% endhint %}

# 토폴로지별 자동화 제공 범위

엔진과 토폴로지에 따라 제공되는 자동화 레벨이 다릅니다.

| 자동화 레벨 | Tibero Single + DR | Tibero TAC + DR | OpenSQL HA |
| --- | --- | --- | --- |
| 0단계 (**수동**) | ✓ | ✓ | ✓ |
| 1단계 (**자동 장애 조치**) | ✓ | ✓ | — |
| 2단계 (**자동 구성 복구**) | — | — | — |
| 3단계 (**완전 자동화**) | ✓ | — | ✓ |

{% hint style="info" %}
**참고**

- **1단계(자동 장애 조치)** 는 Tibero(Single+DR, TAC+DR)에서만 제공되며, OpenSQL은 지원하지 않습니다.
- **3단계(완전 자동화)** 는 Tibero Single+DR과 OpenSQL HA에서 제공되며, **Tibero TAC+DR은 지원하지 않습니다.**
- **2단계(자동 구성 복구)** 는 현재 어떤 토폴로지에서도 제공되지 않습니다.
{% endhint %}

# 자동화 단계별 상세 시나리오

{% hint style="info" %}
**참고**

**참고 — 토폴로지 상태 판별 기준**

승격 이후 구성 정상화 상태는 토폴로지 방식과 동일한 형상인지를 기준으로 판별합니다.

1. **Tibero TAC 구성** : Failover 이후에도 Scale-out을 수행하여 TAC 구조를 유지하고 있는지 확인합니다. Primary Node가 2개 이상인 경우 : `Running` Primary Node가 2개 미만인 경우 : `Degraded`
2. **DR / HA 구성 (Tibero DR · OpenSQL HA)** : Standby/Replica를 보유하고 있는지 확인합니다. Standby/Replica가 1개 이상인 경우 : `Running` (단, Standby/Replica가 비정상 상태이면 `Degraded`일 수 있음) Standby/Replica가 0개인 경우 : `Degraded`
{% endhint %}

## 0단계: 수동

**적용** : Tibero Single+DR · TAC+DR, OpenSQL HA

자동 조치를 수행하지 않으며, 장애 발생 시 사용자가 `역할전환` 버튼으로 Standby/Replica를 Primary/Leader로 직접 승격합니다. 승격 이후의 구성 정상화도 사용자가 수동으로 진행합니다.

## 1단계: 자동 장애 조치

**적용** : Tibero Single+DR · TAC+DR (OpenSQL 미지원)

Old Primary는 재기동하지 않고 종료되며, 신규 Standby를 생성하지 않습니다.

| 단계 | 주요 동작 | 상태 | 시스템 알림 |
| --- | --- | --- | --- |
| 1. 장애 감지 | Auto Failover 요청 전송 | `Failover` | • **Auto Failover 시작**<br>• 요청 실패 시 "**Auto Failover 요청 실패**" |
| 2. Standby 승격 | 가장 최신 로그(TSN)를 반영한 Standby를 Primary로 승격 | - |   |
| 3. new Primary 구성 변경 | TAC 구성인 경우 Scale Out 수행 | `Updating` | • new Primary DB 사용 가능 → "**Auto Failover 완료**"<br>• 실패 시 "**Auto Failover 실패**" |
| 4. Old Primary 처리 | Retired 상태로 표시 후 인스턴스 종료 | - |   |
| 5. 완료 | 구성 정상화 완료 | `Degraded` | • "**구성 정상화 완료**"<br>• 실패 시 "**구성 정상화 실패**" |

## 2단계: 자동 구성 복구

**현재 미지원** (OwlDB v1.3 기준). 자동 장애 조치 후 신규 Standby/Replica를 자동 생성하여 고가용성 구성을 복구하는 단계로 정의되어 있으나, 현재 버전에서는 어떤 토폴로지에서도 제공되지 않습니다.

## 3단계: 완전 자동화

**적용** : Tibero Single+DR, OpenSQL HA (Tibero TAC+DR 미지원)

장애 조치부터 old Primary/Leader 역동기화, 신규 Standby/Replica 생성까지 자동으로 처리합니다. 단일 Primary/Leader 토폴로지에만 적용되므로 TAC Scale Out 과정은 포함되지 않습니다.

| 단계 | 주요 동작 | 상태 | 시스템 알림 |
| --- | --- | --- | --- |
| 1. 장애 감지 | Auto Failover 요청 전송 | `Failover` | • **Auto Failover 시작**<br>• 요청 실패 시 "**Auto Failover 요청 실패**" |
| 2. Old Primary/Leader 재기동 시도 | 인스턴스 재기동 후 DB 재기동 시도 (실패 시 삭제) | - |   |
| 3. 승격 | 가장 최신 로그를 반영한 Standby/Replica를 Primary/Leader로 승격 | - |   |
| 4. new Primary/Leader 전환 | 승격된 노드를 새로운 Primary/Leader로 서비스 시작 | `Updating` | • new Primary/Leader 사용 가능 → "**Auto Failover 완료**"<br>• 실패 시 "**Auto Failover 실패**" |
| 5. 역동기화 시도 | • 재기동 **성공**시 Old Primary/Leader를 Standby/Replica로 재연결 → 8번으로 이동<br>• 재기동**실패** 시 해당 인스턴스 삭제 | - |   |
| 6. 신규 Standby/Replica 생성 | 동일 스펙으로 신규 Standby/Replica 생성 | - |   |
| 7. 연결 및 동기화 | Standby/Replica 연결 및 동기화 | - |   |
| 8. 완료 | 구성 정상화 완료 | `Running` / `Degraded` | • "**구성 정상화 완료**"<br>• 실패 시 "**구성 정상화 실패**" |
