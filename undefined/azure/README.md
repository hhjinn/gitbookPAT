# Azure

Azure 마켓플레이스에서 OwlDB를 이용하기 위한 정보를 확인합니다.

# 클라우드 환경 및 서버 사양

| 항목 | 상세 |
| --- | --- |
| 컴퓨팅 사양 | 2 vCPU, Memory 8 GiB |
| 운영 체제 | Rocky 9.3 |
| 스토리지 엔진 | Azure Premium SSD LRS |
| 지원 언어 | 한국어, 영어 |
| 권장 브라우저 | Google Chrome |
| 최적 해상도 | Full HD (1920*1080) |

# 리전 가용성

| 리전 이름 | 지역 |
| --- | --- |
| Brazil South | brazilsouth |
| Central India | centralindia |
| East Asia | eastasia |
| Germany West Central | germanywestcentral |
| Korea Central | koreacentral |
| Korea South | koreasouth |
| Spain Central | spaincentral |
| Indonesia Central | indonesiacentral |
| New Zealand North | newzealandnorth |
| Qatar Central | qatarcentral |
| Australia East | australiaeast |
| Canada Central | canadacentral |
| North Europe | northeurope |
| West Europe | westeurope |
| France Central | francecentral |
| Italy North | italynorth |
| Japan East | japaneast |
| Poland Central | polandcentral |
| South Africa North | southafricanorth |
| Southeast Asia | southeastasia |
| Sweden Central | swedencentral |
| Switzerland North | switzerlandnorth |
| UAE North | uaenorth |
| UK South | uksouth |
| Central US | centralus |
| East US | eastus |
| West US 2 | westus2 |
| West US 3 | westus3 |

# 인스턴스 유형

OwlDB는 워크로드 요구 사항에 맞춰 여러 인스턴스 유형을 지원합니다. 아래 표에서 vCPU와 메모리 구성을 확인해 선택합니다.

| 인스턴스 유형 | vCPU (CNT) | Memory (GiB) |
| --- | --- | --- |
| Standard_B2ls_v2 | 2 | 4 |
| Standard_B2s_v2 | 2 | 8 |
| Standard_B4s_v2 | 4 | 16 |
| Standard_B8s_v2 | 8 | 32 |
| Standard_D2s_v5 | 2 | 8 |
| Standard_D4s_v5 | 4 | 16 |
| Standard_D8s_v5 | 8 | 32 |
| Standard_D16s_v5 | 16 | 64 |
| Standard_D32s_v5 | 32 | 128 |
| Standard_D48s_v5 | 48 | 192 |
| Standard_D64s_v5 | 64 | 256 |
| Standard_D96s_v5 | 96 | 384 |
| Standard_E2s_v5 | 2 | 16 |
| Standard_E4s_v5 | 4 | 32 |
| Standard_E8s_v5 | 8 | 64 |
| Standard_E16s_v5 | 16 | 128 |
| Standard_E32s_v5 | 32 | 256 |
| Standard_E48s_v5 | 48 | 384 |
| Standard_E64s_v5 | 64 | 512 |
| Standard_E96s_v5 | 96 | 672 |
| Standard_E2s_v6 | 2 | 16 |
| Standard_E4s_v6 | 4 | 32 |
| Standard_E8s_v6 | 8 | 64 |
| Standard_E16s_v6 | 16 | 128 |
| Standard_E32s_v6 | 32 | 256 |
| Standard_E48s_v6 | 48 | 384 |
| Standard_E64s_v6 | 64 | 512 |
| Standard_E96s_v6 | 96 | 768 |

{% hint style="info" %}
**참고**

- Tibero Single: 2vCPU, Memory 8GiB 이상 권장합니다.
- Tibero TAC: 4vCPU 이상만 사용 가능하며, 8vCPU 이상 권장합니다.
{% endhint %}

# 스토리지/디스크 유형

워크로드에 맞춰 선택 가능한 스토리지/디스크 유형과 크기·IOPS 범위를 확인합니다.

Ultra Disk는 Azure 가상 머신(VM)을 위한 스토리지 옵션입니다. SAP HANA와 같은 데이터 집약적인 워크로드, 트랜잭션이 많은 워크로드에 적합합니다.

Premium SSD v2는 가상 머신이나 컨테이너에서 빅데이터 분석, 게임 실행 등 고성능 워크로드에 적합한 스토리지 옵션입니다.

| 유형 | Disk Size (GiB) | Disk IOPS (CNT) |
| --- | --- | --- |
| Ultra Disk | 100 ~ 65,536 | 3,000 ~ 400,000 |
| Premium SSD v2 | 100 ~ 65,536 | 3,000 ~ 80,000 |

{% hint style="info" %}
**참고**

Tibero TAC 토폴로지의 볼륨 크기는 최소 200GiB 이상이어야 합니다.
{% endhint %}
