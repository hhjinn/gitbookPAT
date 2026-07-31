# Azure

## Azure

Azure 마켓플레이스에서 OwlDB를 이용하기 위한 정보를 확인합니다.

## **클라우드 환경 및 서버 사양**

| 항목      | 상세                    |
| ------- | --------------------- |
| 컴퓨팅 사양  | 2 vCPU, Memory 8 GiB  |
| 운영 체제   | Rocky 9.3             |
| 스토리지 엔진 | Azure Premium SSD LRS |
| 지원 언어   | 한국어, 영어               |
| 권장 브라우저 | Google Chrome         |
| 최적 해상도  | Full HD (1920\*1080)  |

## **리전 가용성**

| 리전 이름                | 지역                 |
| -------------------- | ------------------ |
| Brazil South         | brazilsouth        |
| Central India        | centralindia       |
| East Asia            | eastasia           |
| Germany West Central | germanywestcentral |
| Korea Central        | koreacentral       |
| Korea South          | koreasouth         |
| Spain Central        | spaincentral       |
| Indonesia Central    | indonesiacentral   |
| New Zealand North    | newzealandnorth    |
| Qatar Central        | qatarcentral       |
| Australia East       | australiaeast      |
| Canada Central       | canadacentral      |
| North Europe         | northeurope        |
| West Europe          | westeurope         |
| France Central       | francecentral      |
| Italy North          | italynorth         |
| Japan East           | japaneast          |
| Poland Central       | polandcentral      |
| South Africa North   | southafricanorth   |
| Southeast Asia       | southeastasia      |
| Sweden Central       | swedencentral      |
| Switzerland North    | switzerlandnorth   |
| UAE North            | uaenorth           |
| UK South             | uksouth            |
| Central US           | centralus          |
| East US              | eastus             |
| West US 2            | westus2            |
| West US 3            | westus3            |

## 인스턴스 유형

OwlDB는 사용 목적에 따라 적절한 리소스 조합을 선택할 수 있는 유연성을 제공합니다. 다양한 인스턴스 유형을 제공하므로, 목표로 하는 워크로드 요구 사항까지 데이터베이스를 확장할 수 있습니다.

| 인스턴스 유형            | vCPU (CNT) | Memory (GiB) |
| ------------------ | ---------- | ------------ |
| Standard\_B2ls\_v2 | 2          | 4            |
| Standard\_B2s\_v2  | 2          | 8            |
| Standard\_B4s\_v2  | 4          | 16           |
| Standard\_B8s\_v2  | 8          | 32           |
| Standard\_D2s\_v5  | 2          | 8            |
| Standard\_D4s\_v5  | 4          | 16           |
| Standard\_D8s\_v5  | 8          | 32           |
| Standard\_D16s\_v5 | 16         | 64           |
| Standard\_D32s\_v5 | 32         | 128          |
| Standard\_D48s\_v5 | 48         | 192          |
| Standard\_D64s\_v5 | 64         | 256          |
| Standard\_D96s\_v5 | 96         | 384          |
| Standard\_E2s\_v5  | 2          | 16           |
| Standard\_E4s\_v5  | 4          | 32           |
| Standard\_E8s\_v5  | 8          | 64           |
| Standard\_E16s\_v5 | 16         | 128          |
| Standard\_E32s\_v5 | 32         | 256          |
| Standard\_E48s\_v5 | 48         | 384          |
| Standard\_E64s\_v5 | 64         | 512          |
| Standard\_E96s\_v5 | 96         | 672          |
| Standard\_E2s\_v6  | 2          | 16           |
| Standard\_E4s\_v6  | 4          | 32           |
| Standard\_E8s\_v6  | 8          | 64           |
| Standard\_E16s\_v6 | 16         | 128          |
| Standard\_E32s\_v6 | 32         | 256          |
| Standard\_E48s\_v6 | 48         | 384          |
| Standard\_E64s\_v6 | 64         | 512          |
| Standard\_E96s\_v6 | 96         | 768          |

{% hint style="info" %}
**참고** Tibero의 Single은 2vCPU, Memory 8GiB 이상의 인스턴스 유형을 권장합니다.   Tibero의 TAC는 4vCPU 이상의 인스턴스 유형만 사용 가능하며, 8vCPU 이상을 권장합니다.
{% endhint %}

## 스토리지/디스크 유형

#### Ultra Disk

Azure 가상 머신(VM)을 위한 최고 성능의 스토리지 옵션입니다. SAP HANA와 같은 데이터 집약적인 워크로드, 최상위 데이터베이스, 트랜잭션이 많은 워크로드에 적합합니다.

| Disk Size (GiB) | Disk IOPS (CNT)  |
| --------------- | ---------------- |
| 100 \~ 65,536   | 3,000 \~ 400,000 |

#### Premium SSD v2

Premium SSD v2는 기존 Premium SSD보다 향상된 성능을 제공하며, 비용 효율적인 스토리지 옵션입니다. 이 제품은 가상 머신이나 컨테이너에서 빅데이터 분석, 게임 실행 등 다양한 고성능 워크로드에 적합합니다.

| Disk Size (GiB) | Disk IOPS (CNT) |
| --------------- | --------------- |
| 100 \~ 65,536   | 3,000 \~ 80,000 |

{% hint style="info" %}
**참고** Tibero TAC 토폴로지의 볼륨크기는 최소 200GiB 이상의이어야 합니다.
{% endhint %}
