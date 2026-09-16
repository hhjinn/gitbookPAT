인스턴스 상태(Status)와 세부 상태(Health) 조합에 따라 OwlDB 라이선스 요금이 결정됩니다. 이 페이지는 각 상태 조합에서 라이선스 요금이 부과되는지 여부와 그 의미를 설명합니다.

{% hint style="info" %}
**참고**

인스턴스, 스토리지, 네트워크 등 인프라 리소스 요금은 이 기준과 무관하게 각 CSP의 과금 정책에 따라 별도로 발생합니다.
{% endhint %}

# 미터링 기준

DB 서비스 라이선스 요금은 사용한 시간을 기준으로 산정하며, 최소 과금 단위는 1분입니다.

| 항목 | 기준 |
| --- | --- |
| 최소 과금 단위 | 1분 |
| 초 단위 처리 | 분 단위로 올림 |
| 요금 표시 | 달러($), 소수점 둘째 자리 반올림 |

# 청구 기준

<table data-full-width="true"><thead><tr><th>Status</th><th>Health</th><th>청구 여부</th><th>설명</th></tr></thead><tbody><tr><td>Provisioning</td><td>-</td><td>미청구</td><td>인스턴스 생성 중</td></tr><tr><td>Running</td><td>available</td><td>청구</td><td>정상 동작 중</td></tr><tr><td>Updating</td><td><ul><li>available</li><li>in progress</li></ul></td><td>청구</td><td>스펙 변경·재시작·마이그레이션·복구·백업 등 작업 진행 중</td></tr><tr><td>Degraded</td><td><ul><li>available</li><li>in progress</li><li>limited</li></ul></td><td>청구</td><td><ul><li>일부 인스턴스 재시작·재구성 중</li><li>일부 관리 기능 제한</li></ul></td></tr><tr><td>Degraded</td><td>unavailable</td><td>미청구</td><td>DB·VM 중단 등으로 사용 불가</td></tr><tr><td>Degraded</td><td>retired</td><td>미청구</td><td>Failover 이후 미사용 인스턴스</td></tr><tr><td>Failover</td><td>in progress</td><td>청구</td><td>자동 Failover 진행 중</td></tr><tr><td>Down</td><td>unavailable</td><td>미청구</td><td>DB 서비스 전체 중단</td></tr><tr><td>Stopping</td><td>in progress</td><td>청구</td><td>중지 상태로 전환 중</td></tr><tr><td>Stopped</td><td>unavailable</td><td>미청구</td><td>모든 리소스 일시 비활성화</td></tr><tr><td>Starting</td><td>in progress</td><td>미청구</td><td>중지 상태에서 재시작 중</td></tr><tr><td>Terminating</td><td>unavailable</td><td>미청구</td><td>리소스·데이터 영구 삭제 중</td></tr></tbody></table>

{% hint style="info" %}
**참고**

일시적으로 DB 서비스를 사용하지 않을 예정이라면 **중지**를 권장합니다.

중지 상태에서는 DB 서비스 라이선스와 인스턴스 사용 요금이 청구되지 않고, 데이터가 보존되므로 필요할 때 다시 시작할 수 있습니다. 다만 저장된 데이터를 보관하기 위해 볼륨이 유지되므로 스토리지 요금은 계속 부과됩니다.

스토리지 요금을 포함한 모든 요금 발생을 중단하려면 DB 서비스를 삭제해야 합니다. DB 서비스를 삭제하면 인스턴스, 볼륨, 백업 등 모든 리소스와 데이터가 영구적으로 삭제되며 복구할 수 없습니다.
{% endhint %}
