역할 전환은 DR(재해 복구) 구성의 Primary DB와 Standby DB 역할을 수동으로 전환하는 기능입니다. **관리 > Overview** 페이지에서 역할 전환을 수행합니다.

{% hint style="info" %}
**참고**

AWS 환경에서는 Tibero 엔진에서만 역할 전환을 지원합니다. Azure 환경에서는 Tibero와 OpenSQL 모두 지원합니다.
{% endhint %}

## 역할 전환

역할 전환 모달은 **보안 인증**과 **전환 설정**의 두 단계로 구성됩니다. 첫 번째 단계에서 현재 로그인 계정의 비밀번호를 입력해 관리자 권한을 확인하고, 두 번째 단계에서 새로운 Primary DB를 선택합니다.

1. **관리 > Overview** 페이지로 이동합니다.
2. **작업** 버튼을 클릭합니다.
3. 드롭다운 목록에서 **역할전환**을 클릭합니다.
4. **역할 전환 전 보안인증** 단계에서 현재 로그인 계정의 비밀번호를 입력합니다.
5. **확인**을 클릭합니다.

   {% hint style="info" %}
**참고**

   비밀번호가 일치하지 않으면 "비밀번호가 일치하지 않습니다." 오류 메시지가 나타납니다.
   {% endhint %}
6. **새로운 Primary DB 선택** 드롭다운에서 승격할 Standby DB를 선택합니다.
7. 필요한 경우 **비고**를 입력합니다.
8. **확인**을 클릭합니다.

전환 요청 후, 시스템은 현재 Primary DB의 상태를 자동으로 진단하여 전환 방식을 결정합니다.

* **Primary DB가 정상 상태**: Switchover 방식으로 수행됩니다. 현재 Primary를 안전하게 종료하고 데이터 동기화를 완료한 후, 선택한 Standby DB를 새로운 Primary로 승격합니다.
* **Primary DB가 비정상 상태**: Failover 방식으로 수행됩니다. 선택한 Standby DB를 즉시 새로운 Primary로 승격시켜 서비스를 복구합니다.

{% hint style="warning" %}
**주의**

* 현재 Primary DB의 Health가 `In Progress` 상태이거나, 모든 Standby DB가 `Unavailable` 또는 `In Progress` 상태인 경우에는 역할 전환을 수행할 수 없습니다.
* `Available` 상태가 아닌 Standby DB는 새로운 Primary DB 선택 목록에서 선택할 수 없습니다.
{% endhint %}

{% hint style="warning" %}
**주의**

역할 전환 완료 후 "구성 정상화 실패" 알림을 수신한 경우, 고가용성 및 DR 유지를 위해 수동 조치가 필요합니다.
{% endhint %}
