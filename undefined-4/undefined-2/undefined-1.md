**관리 > 백업/복구 > 백업**에서 데이터베이스 백업 목록을 조회하고 생성·수정·삭제·복구를 수행합니다. Full/Incremental Backup을 지원하며, 스케줄러를 통해 자동 생성되거나 사용자가 수동으로 생성할 수 있습니다.

{% hint style="info" %}
**참고**

On-Premise 환경에서는 Full/Incremental Backup을 지원하며, 보관(Archive) 기능은 제공하지 않습니다.
{% endhint %}

## 백업 목록 조회

**관리 > 백업/복구 > 백업** 메뉴에 진입하면 현재 데이터베이스의 백업 목록이 나타납니다. Full Backup을 루트로 하여 Incremental Backup이 트리 구조로 중첩되어 표시됩니다.

목록에 표시되는 컬럼은 다음과 같습니다.

<table data-full-width="true"><thead><tr><th>항목</th><th>설명</th></tr></thead><tbody><tr><td>이름</td><td>백업 이미지 이름</td></tr><tr><td>백업 경로</td><td>백업 경로 이름</td></tr><tr><td>백업 방식</td><td>Full Backup 또는 Incremental Backup</td></tr><tr><td>생성일</td><td>백업 이미지 생성된 일시 (yyyy.mm.dd HH:mm:ss)</td></tr><tr><td>만료일</td><td><ul><li>보존 기간으로 계산된 만료 일시(yyyy.mm.dd HH:mm:ss)</li><li>Incremental Backup은 연관된 Full Backup의 만료일을 그대로 표시</li></ul></td></tr><tr><td>Size(MB)</td><td><ul><li>백업 이미지의 총 용량 표시</li><li>Full Backup은 이후 생성된 Incremental Backup 용량을 합산하여 표시</li><li>Incremental Backup은 해당 백업의 용량만 표시</li></ul></td></tr><tr><td>생성 방식</td><td>자동/수동</td></tr><tr><td>상태</td><td>현재 백업 상태</td></tr></tbody></table>

### 백업 상태 이력

<table data-full-width="true"><thead><tr><th>항목</th><th>설명</th></tr></thead><tbody><tr><td>상태</td><td><ul><li>생성 시작</li><li>복구 가능</li><li>생성 실패</li><li>복구 시작</li><li>복구 실패</li><li>삭제 시작</li><li>삭제됨</li><li>사용 불가</li></ul></td></tr><tr><td>발생일</td><td>상태가 변경된 일시 표시 (yyyy.mm.dd HH:mm:ss)</td></tr></tbody></table>

{% hint style="info" %}
**참고**

OpenSQL에서 OpenBackup 사용을 **미사용**으로 전환하면 상태는 삭제됨으로 표시됩니다.
{% endhint %}

## 백업 생성

이름과 보존 기간을 입력하면 원하는 시점의 단일 백업 이미지가 생성됩니다. Tibero는 RMGR 기반으로 Full Backup 단위를 관리하며, OpenSQL은 rsync 또는 postgres 방식 중 선택한 방식에 따라 백업 단위가 달라집니다.

1. **백업** 페이지에서 **생성** 버튼을 클릭합니다.
2. **Type**을 선택합니다.
3. **이름**을 입력합니다.
4. 달력에서 **보존 기간**의 종료일을 선택합니다.
5. **생성** 버튼을 클릭합니다.

{% hint style="info" %}
**참고**

* 데이터베이스 운영 상태가 `Running`일 때만 가능합니다.
* Incremental Backup은 사용 가능한 Full Backup이 있어야 생성할 수 있습니다.
{% endhint %}

## 복구

백업 목록에서 백업을 하나 선택하고 **복구** 버튼을 클릭하면 복구 모달이 나타납니다. 복구 유형을 선택하여 데이터베이스를 특정 시점으로 복구합니다.

<table><thead><tr><th>복구 유형</th><th>설명</th></tr></thead><tbody><tr><td>Full Restore</td><td>마지막 커밋 시점까지 전체 데이터 복구</td></tr><tr><td>PITR (Point-in-Time Recovery)</td><td><ul><li>지정 날짜·시각(분 단위)까지 복구</li><li>지정 시점 이후 데이터 손실</li></ul></td></tr></tbody></table>

{% hint style="warning" %}
**주의**

복구를 실행하면 복구 유형과 DB 엔진에 따라 기존 백업이 삭제되고 DR 구성 Standby의 동작이 달라집니다. 장기 보관이 필요한 데이터는 복구 전에 미리 보관하세요.
{% endhint %}

**복구 유형별 백업·DR 영향**

<table data-full-width="true"><thead><tr><th>복구 유형</th><th>기존 백업</th><th>DR 구성 Standby 처리</th></tr></thead><tbody><tr><td>Full Restore (Tibero)</td><td><ul><li>기존 백업 유지</li><li>Incremental Backup 선택 복구 시 백업 체인 변경으로 해당 백업 이후 생성 백업 사용 불가</li></ul></td><td>Standby 정상 재기동 시 자동 재동기화 (등록 DB·설치 DB 동일)</td></tr><tr><td>Full Restore (OpenSQL)</td><td>선택한 백업 이후 생성된 백업 삭제</td><td>Standby 정상 재기동 시 자동 재동기화</td></tr><tr><td>PITR (Tibero)</td><td>현재 저장된 모든 백업 삭제</td><td><ul><li>설치 DB : Standby 자동 재구축</li><li>등록 DB : Standby Down 유지, 복구 후 관리자 직접 재구축</li></ul></td></tr><tr><td>PITR (OpenSQL)</td><td>복구 시점 이후 생성된 백업·WAL 삭제, 지정 시각 이전 백업 유지</td><td><ul><li>설치 DB : Standby 자동 재구축</li><li>등록 DB : Standby Down 유지, 복구 후 관리자 직접 재구축</li></ul></td></tr></tbody></table>

복구에는 최대 몇 시간이 소요될 수 있으며, 진행 상황은 복구 내역에서 확인합니다.

1. 복구할 백업을 하나 선택합니다.
2. **복구** 버튼을 클릭합니다.
3. **복구 유형**을 선택합니다.
4. **PITR**을 선택한 경우, 복구할 날짜와 시각을 선택합니다. 선택 가능한 범위는 해당 백업의 복구 가능 시점과 현재 시각 사이입니다.
5. **복구** 버튼을 클릭합니다.

{% hint style="info" %}
**참고**

* 데이터베이스 운영 상태가 `Running`, `Down`, `Degraded`일 때만 가능합니다.
* OpenSQL은 백업 방식에 따라 복구 후 Incremental Backup 생성 동작이 다릅니다. `rsync` : 복구 이후에도 기존 백업 체인을 이어서 Incremental Backup을 생성할 수 있습니다. `postgres` : 복구 시 새 timeline으로 분기되어 기존 체인을 이어갈 수 없습니다. 복구 후 Full Backup을 1회 수행해야 하며, 해당 백업이 새로운 기준점이 됩니다. `postgres` 방식은 PostgreSQL 17 이상에서만 선택할 수 있습니다.
* 복구가 실패하는 경우 이전에 선택한 시점보다 더 이전 시점을 선택해 다시 시도해 주세요. 여러 차례 시도해도 복구되지 않으면 기술지원을 요청해 주세요.
{% endhint %}

## 백업 수정

백업의 이름과 보존 기간을 수정합니다.

1. 수정할 백업을 하나 선택합니다.
2. **수정** 버튼을 클릭합니다.
3. **이름** 또는 **보존 기간**을 변경합니다.
4. **저장** 버튼을 클릭합니다.

## 백업 삭제

선택한 백업 이미지를 영구적으로 삭제합니다.

1. 삭제할 백업을 하나 이상 선택합니다.
2. **삭제** 버튼을 클릭합니다.
3. 삭제 안내 모달을 확인합니다.
4. **삭제** 버튼을 클릭합니다.

{% hint style="warning" %}
**주의**

삭제된 백업은 복구할 수 없으므로 삭제 전 필요한 조치를 취했는지 다시 한 번 확인합니다.

* **Tibero(RMGR)**: Incremental Backup만 단독으로 선택하면 삭제할 수 없습니다. Full Backup 단위로 선택해야 하며, 이 경우 하위 Incremental Backup이 모두 함께 삭제됩니다.
* **OpenSQL(rsync)**: Full Backup과 Incremental Backup을 구분 없이 개별적으로 선택할 수 있으며, 선택한 백업만 삭제되어 다른 백업에는 영향을 주지 않습니다.
* **OpenSQL(postgres)**: Incremental Backup을 단독으로 선택할 수 있으며, 선택한 백업 이후에 생성된 Incremental Backup까지 함께 삭제됩니다.
{% endhint %}
