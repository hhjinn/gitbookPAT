# DB 서비스

OwlDB에서 운영 중인 데이터베이스의 상태를 조회하고, 수정·중지·시작·삭제·역할 전환·라이선스 갱신·스펙 변경 등을 수행합니다.

{% hint style="info" %}
**참고**

* 인스턴스 상태가 `Running`이 아닌 경우, 일부 정보가 누락될 수 있습니다.
* 데이터베이스 관리 페이지는 데이터베이스 타임존을 기준으로 시간이 표시되므로, 로컬 시스템 시간(브라우저 시간)과 차이가 있을 수 있습니다.
{% endhint %}

***

### 데이터베이스 정보 조회

1. **관리 > Overview** 메뉴를 클릭합니다.
2. **DB Service Name** 드롭다운 버튼을 클릭하여 정보를 조회할 데이터베이스를 선택합니다.
3. 운영정보, 인스턴스, 버전, (DR/HA) 전환 이력 관리 탭에서 상세 정보를 확인합니다.

{% hint style="info" %}
**참고**

데이터베이스 운영 상태는 **전체 상태 요약 정보** 페이지를 참고하시기 바랍니다.
{% endhint %}

{% tabs %}
{% tab title="운영 정보" %}
<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>그림 1. 운영 정보</p></figcaption></figure>

해당 데이터베이스에 접근 가능한 계정정보, 컨트롤 파일, 로그, 체크포인트 등 데이터베이스 상세 정보를 확인할 수 있으며, 데이터베이스 구성을 다이어그램으로 시각적으로 확인 할 수 있습니다.

{% hint style="info" %}
**참고**

일시가 표시되는 항목은 모두 데이터베이스 타임존을 기준으로 표시됩니다.
{% endhint %}
{% endtab %}

{% tab title="인스턴스" %}
<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>그림 2. 인스턴스</p></figcaption></figure>

구성 인스턴스 목록과 정보를 확인합니다.

* **인스턴스 별칭**을 클릭하면 "[인스턴스 관리](db.md#dF57s45IXBUgU7RX1UvL)" 페이지로 이동합니다.
* ☑️ 아이콘으로 1개 이상의 인스턴스를 먼저 선택한 후 **재시작** 버튼을 클릭하거나, 선택 없이 **재시작** 버튼을 바로 클릭해 열리는 모달에서 재시작할 인스턴스와 재시작 옵션을 선택할 수 있습니다. 자세한 내용은 "[인스턴스 재시작](db.md#undefined-2)"을 참고하시기 바랍니다.

{% hint style="info" %}
**참고**

DR 구성을 사용하는 경우, Primary(Leader) DB와 Standby(Replica) DB를 구분하여 조회합니다.
{% endhint %}
{% endtab %}

{% tab title="설치 정보" %}
데이터베이스 버전 정보와 시스템 및 컴파일 정보, 패치(또는 Extension) 정보를 확인합니다.

시스템 및 컴파일 정보는 엔진과 무관하게 바이너리 OS 정보 등을 리스트로 표시합니다. Basic Info와 패치 정보는 엔진에 따라 다음과 같이 달라집니다.

<table data-full-width="true"><thead><tr><th>항목</th><th>Tibero</th><th>OpenSQL</th></tr></thead><tbody><tr><td>Basic Info</td><td><ul><li>메이저 버전</li><li>마이너 버전</li><li>패치셋 버전</li></ul></td><td><ul><li>OpenSQL 버전(예: 3.0)</li><li>PostgreSQL 버전(예: 17.5)</li></ul></td></tr><tr><td>시스템 및 컴파일 정보</td><td>바이너리 OS 정보 등 리스트 표시</td><td>바이너리 OS 정보 등 리스트 표시</td></tr><tr><td>패치 정보 / Extensions</td><td><ul><li>적용된 패치 현황 리스트 표시</li><li>없으면 "적용된 패치가 없습니다" 문구 표시</li></ul></td><td><ul><li>현재 설치된 Extension 목록 리스트 표시</li><li>운영 중 DDL로 추가한 Extension도 조회 시점 기준 반영</li></ul></td></tr></tbody></table>

{% hint style="info" %}
**참고**

값을 조회할 수 없는 항목은 `-`로 표시됩니다.
{% endhint %}
{% endtab %}

{% tab title="(DR/HA) 전환 이력 관리" %}
Tibero DR 구성 혹은 OpenSQL HA 구성일 때 제공하는 탭입니다.

데이터베이스 역할 전환 이벤트가 발생한 이력을 확인합니다.

{% hint style="info" %}
**참고**

OpenSQL의 역할 전환은 Patroni가 수행하며, OwlDB는 노드 역할(Role)이 변경된 것을 확인해 이력에 반영합니다. 이때 해당 전환이 사용자가 수행한 Switchover인지 Patroni가 수행한 Failover인지 구분하지 않으므로, 유형은 모두 `Failover`로 기록됩니다.
{% endhint %}

<table data-full-width="true"><thead><tr><th>컬럼명</th><th>설명</th><th>데이터 형식</th><th>기본값</th><th>필수값</th></tr></thead><tbody><tr><td>ID</td><td><ul><li>전환 이벤트를 고유하게 식별하는 번호</li><li>형식: {이벤트유형-랜덤 문자열 16바이트}</li><li>이벤트 유형: FO / SO / FB</li></ul></td><td><ul><li>FO-3f9a7c1e2d8b45f0</li><li>SO-b17e4a93d2c68f5e</li><li>FB-7a2d9e14c6b83f05</li></ul></td><td>O</td><td>X</td></tr><tr><td>시작 시간</td><td>전환 이벤트가 발생한 시각</td><td>yyyy.mm.dd HH:mm:ss</td><td>O</td><td>O</td></tr><tr><td>완료 시간</td><td>전환 이벤트가 완료된 시각</td><td>yyyy.mm.dd HH:mm:ss</td><td>X</td><td>X</td></tr><tr><td>유형</td><td>전환 이벤트 유형</td><td><ul><li>Failover</li><li>Switchover</li><li>Failback</li></ul></td><td>O</td><td>O</td></tr><tr><td>수행 대상</td><td>해당 이벤트를 수행한 대상</td><td><ul><li>Switchover: {사용자 아이디}</li><li>Failover: {사용자 아이디} / system(Auto Failover)</li><li>Failback: {사용자 아이디}</li></ul></td><td>O</td><td>X</td></tr><tr><td>결과</td><td>해당 이벤트 상태 표시</td><td><ul><li>성공</li><li>실패</li></ul></td><td>O</td><td>X</td></tr><tr><td>원인/비고</td><td>해당 이벤트가 발생한 원인, 사용자가 입력한 값 또는 실패 사유 표시</td><td><ul><li>역할 전환 시 사용자가 선택적으로 입력한 값(최대 200자, 미입력 시 빈칸)</li><li>Auto Failover의 트리거 조건</li><li>Failback 실패 시: Standby/Replica Reboot Failed 또는 Switchover Failed</li><li>Failover 실패 시: Standby/Replica Promotion Failed</li><li>Failover 성공 후 후처리 실패 시: Cluster Normalization Failed(Primary scale out failed / New Standby/Replica creation failed, 복수 실패 시 콤마로 표시)</li></ul></td><td>O</td><td>X</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

### DB Service 정보 수정

1. DB Service 별칭 옆 **연필 아이콘**을 클릭합니다.
2. DB Service 별칭과 설명을 수정합니다.
3. **저장** 버튼을 클릭합니다.
