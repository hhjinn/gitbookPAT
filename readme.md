OwlDB는 클라우드와 온프레미스 환경에서 데이터베이스를 설치하고 DB Service로 등록·통합 관리하는 관리형 데이터베이스 서비스입니다. 이 페이지에서는 OwlDB의 운영 환경과 주요 기능, 지원 엔진·토폴로지를 확인합니다.

# 운영 환경

## 클라우드 환경 지원

클라우드 인프라 자원을 활용하여 데이터베이스를 동적으로 생성하고 운영하는 방식입니다. IaC(Infrastructure as Code) 기반으로 인프라 프로비저닝과 데이터베이스 설정을 자동화하여, 사용자가 콘솔에서 원하는 사양의 데이터베이스 환경을 구축하고 확장할 수 있습니다.

## 온프레미스 환경 지원

고객이 자체 보유한 서버, 네트워크, 스토리지 등 물리적 인프라 자원을 기반으로 데이터베이스를 운영하는 방식입니다. 고정된 인프라 리소스를 효율적으로 활용할 수 있도록 설계되었으며, 외부 네트워크와 단절된 폐쇄망 환경을 지원합니다. OwlDB를 통해 호스트에 신규 데이터베이스를 설치하거나, 기존에 이미 운영 중인 데이터베이스를 OwlDB 관리 대상으로 연동하여 통합 제어할 수 있습니다.

# 주요 기능

OwlDB는 두 환경에서 보편적으로 사용되는 공통 관리 기능을 바탕으로, 클라우드와 온프레미스 각각의 인프라 특성에 최적화된 전용 기능을 제공합니다.

**공통 기능**

<table><thead><tr><th>kW2I1txkIn8l</th><th>vzQn7o9RHrsc</th></tr></thead><tbody><tr><td><strong>기능</strong></td><td><strong>설명</strong></td></tr><tr><td><strong>데이터베이스 상태 조회</strong></td><td>데이터베이스·인스턴스 가동 상태 실시간 확인</td></tr><tr><td><strong>모니터링 & 알림</strong></td><td><ul><li>핵심 성능 지표·운영 상태 감시</li><li>이상 징후·이벤트 발생 시 즉시 알림 발송</li></ul></td></tr><tr><td><strong>마이그레이션</strong></td><td>이종 데이터베이스 전환 시 사전 호환성 검증 및 가이드 기반 마이그레이션 지원</td></tr><tr><td><strong>계정 관리 (RBAC)</strong></td><td>역할 기반 접근 제어를 통한 사용자별 권한 분리·보안 관리</td></tr></tbody></table>

{% tabs %}
{% tab title="클라우드 특화" %}
| yrwoiEEv1XW8 | t5QSJoP8Piuk |
| --- | --- |
| **기능** | **설명** |
| **자동화된 프로비저닝** | 클라우드 자원 생성부터 데이터베이스 아키텍처 구성까지 전 과정 자동화 |
| **리소스 확장/변경** | 워크로드 증감에 맞춘 인스턴스 사양·스토리지 확장 및 변경 |
| **클라우드 스냅샷 백업** | CSP 스냅샷 기능 연동 기반 백업·복구 |
{% endtab %}
{% tab title="온프레미스 특화" %}
<table><thead><tr><th>urVh530GRf0j</th><th>4vhMNTWPqZQm</th></tr></thead><tbody><tr><td><strong>기능</strong></td><td><strong>설명</strong></td></tr><tr><td><strong>데이터베이스 설치/등록</strong></td><td><ul><li>고객 호스트에 신규 DB 원격 배포</li><li>기존 운영 중인 외부 DB 관리 대상 등록</li></ul></td></tr><tr><td><strong>인프라 자원 탐색</strong></td><td>Agent를 통한 하드웨어 스펙·구성 정보 자동 수집 및 현황 파악</td></tr><tr><td><strong>물리 백업/복구</strong></td><td>데이터베이스 자체 유틸리티(Tibero RMGR 등) 기반 백업·복구</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

## 데이터베이스 엔진 및 토폴로지

OwlDB가 지원하는 관계형 데이터베이스(RDBMS) 엔진 사양 및 환경별 아키텍처 구성은 다음과 같습니다.

{% tabs %}
{% tab title="Cloud" %}
### AWS

<table data-full-width="true"><thead><tr><th>데이터베이스 엔진</th><th>버전</th><th>토폴로지</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr></tbody></table>

### Azure

<table data-full-width="true"><thead><tr><th>데이터베이스 엔진</th><th>버전</th><th>토폴로지</th></tr></thead><tbody><tr><td>Tibero</td><td>7.2.5</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td><ul><li>3.16.12.5</li><li>3.17.8.5</li></ul></td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% tab title="On-Premise" %}
### OwlDB Operation

<table data-full-width="true"><thead><tr><th>데이터베이스 엔진</th><th>버전</th><th>토폴로지</th></tr></thead><tbody><tr><td>Tibero</td><td>7 패치셋 이후</td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>

### OwlDB Automation

<table data-full-width="true"><thead><tr><th>데이터베이스 엔진</th><th>버전</th><th>토폴로지</th></tr></thead><tbody><tr><td>Tibero</td><td><ul><li>등록: 7 패치셋 이후</li><li>설치: 7.2.5</li></ul></td><td><ul><li>Single</li><li>Single + DR</li><li>TAC</li><li>TAC + DR</li></ul></td></tr><tr><td>OpenSQL</td><td>3.0(PostgreSQL 17.9)</td><td><ul><li>Single</li><li>HA</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
