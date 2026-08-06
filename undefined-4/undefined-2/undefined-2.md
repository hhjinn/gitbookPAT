**백업 설정** 페이지에서 자동 백업 스케줄러를 구성하고 운영 상태를 확인합니다. On-Premise 환경에서는 Full Backup과 Incremental Backup을 각각 독립적으로 설정하며, 실행 주기·보존 기간·시작 시간을 지정합니다. 페이지 하단의 스케줄러 운영 상태 섹션에서 최근 실행 결과와 30일 이력 차트로 백업 안정성을 점검하고, 백업 스토리지 사용량도 함께 확인합니다.

{% hint style="warning" %}
**주의**

OpenSQL은 백업/복구 기능을 사용하기 위해 OpenBackup을 사용해야 합니다.
{% endhint %}

# 백업 설정

**관리 > 백업 설정**을 클릭하면 현재 자동 백업 구성 정보를 확인합니다. On-Premise 환경에서는 Full Backup과 Incremental Backup이 구분되어 표시됩니다.

1. **관리 > 백업 설정**을 클릭합니다.
2. **수정**을 클릭합니다.
3. 설정 항목을 입력합니다. OpenSQL의 경우, OpenBackup 설정 정보를 추가로 입력합니다.
4. **저장**을 클릭합니다.

## [OpenSQL] OpenBackup 설정

OpenSQL On-Premise 환경에서는 자동 백업 항목 위에 OpenBackup 설정 정보가 추가로 표시됩니다. OpenBackup이 미사용 상태이면 페이지 상단에 안내 배너가 나타나고 자동 백업 항목은 표시되지 않습니다.

| 항목 | 설명 |
| --- | --- |
| OpenBackup | OpenBackup 사용 여부 (사용/미사용) |
| Health | 백업 서버 연결 상태 (연결됨/연결안됨) |
| 백업 서버 | 사용 중인 OpenBackup(Barman) 서버 정보 |
| OpenBackup Agent Port | OpenBackup 서버의 Agent와 통신하는 포트 번호 |
| WAL 보관 방식 | `archiver` / `streaming` / `archiver + streaming` |
| 백업 방식 | `rsync` / `postgres` |
| 백업 재사용 방식 | 백업 방식이 `rsync`인 경우에만 표시 (`link` / `copy`) |

## 수정 모드 입력 항목

| 항목 | 설명 | 입력 규칙 |
| --- | --- | --- |
| Full Backup | Full Backup 사용 여부 | 기본값: 꺼짐 |
| Full Backup 주기 | Full Backup 실행 주기 | * 시간마다: 1~23<br>* 일마다: 1~7 |
| 보존 기간 | Full Backup 이미지 보존 기간 | * 시간마다: 1~23<br>* 일마다: 1~35<br>* 영구보관 |
| 시작 시간 | Full Backup 시작 일시 | 현재보다 과거 일시 선택 불가 |
| Incremental Backup | Incremental Backup 사용 여부 | 기본값: 꺼짐 |
| Incremental Backup 주기 | Incremental Backup 실행 주기 | * 시간마다: 1~23<br>* 일마다: 1~6 |

Incremental Backup 설정 시 다음 사항에 유의합니다.

- Full Backup이 꺼짐인 상태에서 Incremental Backup을 켜면 Full Backup도 자동으로 켜짐으로 전환됩니다.
- Incremental Backup 주기는 Full Backup 주기보다 작아야 합니다.

{% hint style="info" %}
**참고**

OpenSQL On-Premise에서 PostgreSQL 16 이하 버전이고 WAL 보관 방식이 `streaming`인 경우 Incremental Backup을 사용할 수 없습니다.
{% endhint %}

# 백업 스케줄러 운영 상태

백업 설정 페이지 하단의 **백업 스케줄러 운영 상태** 섹션에서 자동 백업의 최근 실행 이력과 안정성 지표를 확인합니다. 자동 백업이 꺼진 상태에서도 마지막 실행 정보가 표시될 수 있습니다.

| 항목 | 설명 | 미설정 시 |
| --- | --- | --- |
| 최근 실행 결과 | Full Backup과 Incremental Backup 각각의 가장 최근 완료 실행 결과 (성공/실패) | `-` |
| 최근 7일 성공률 | 최근 7일 이내 완료된 자동 백업 전체의 성공 비율 (예: 90% (9/10)) | `-` |
| 연속 실패 횟수 | 가장 최근 완료 건부터 연속으로 실패한 횟수 (모두 성공이면 0회) | `-` |
| 최근 30일 실행 결과 차트 | 최근 30일간 일자별 자동 백업 성공/실패 건수를 누적 막대 차트로 표시 | No Data |

최근 7일 성공률과 연속 실패 횟수는 Full Backup과 Incremental Backup 전체 실행 건을 합산하여 계산합니다. 최근 실행 결과는 설정 상태에 따라 다음과 같이 표시됩니다.

- **Full Backup만 설정**: Full Backup 최근 실행 결과만 표시합니다.
- **Full + Incremental Backup 동시 설정**: 두 유형의 실행 결과를 각각 구분하여 표시합니다.
- **미설정**: `-`를 표시합니다.

차트의 막대 위에 마우스를 올리면 해당 일자의 Full Backup과 Incremental Backup 각각의 성공·실패 건수를 확인합니다. 당일 실행 예정이나 아직 완료되지 않은 백업은 차트에 포함되지 않습니다.

1. **관리 > 백업 설정**을 클릭합니다.
2. 페이지 하단 **백업 스케줄러 운영 상태** 섹션에서 최근 실행 결과, 최근 7일 성공률, 연속 실패 횟수를 확인합니다.
3. 차트에서 막대 위에 마우스를 올려 일자별 상세 실행 결과를 확인합니다.

# 백업 스토리지 사용량

백업 스토리지 사용량 섹션에서 현재 백업에 사용 중인 디스크 스토리지 현황을 가로 막대 차트로 확인합니다. 차트 중앙에 전체 스토리지 대비 사용량 비율이 표시되며, 각 항목은 색상과 범례로 구분됩니다.

{% tabs %}
{% tab title="Tibero" %}
| 항목 | 설명 |
| --- | --- |
| 전체 스토리지 | 백업 스토리지 총 용량 |
| Backup | Full/Incremental Backup 스토리지 사용량 |
| Archive Log | 복구 시점 보장을 위해 보관되는 Archive Log 사용량 |
| Others | 현재 DB 서비스의 Backup/Archive Log 외에 동일 경로에서 사용 중인 용량 |
| Free | 사용되지 않은 여유 용량 |
{% endtab %}
{% tab title="OpenSQL" %}
| 항목 | 설명 |
| --- | --- |
| 전체 백업 스토리지 | 현재 DB 서비스가 사용할 수 있는 백업 스토리지 총 용량 |
| Backup | Full/Incremental Backup 스토리지 사용량 |
| WAL | 복구 시점 보장을 위해 보관되는 WAL 사용량 |
| Others | 현재 DB 서비스의 Backup/WAL 외에 동일 경로에서 사용 중인 용량 |
| Free | 사용되지 않은 여유 용량 |

{% hint style="info" %}
**참고**

Others 항목에는 동일 Barman 서버를 사용하는 다른 DB 서비스의 사용량이나 임의 파일 등이 포함될 수 있습니다.
{% endhint %}
{% endtab %}
{% endtabs %}
