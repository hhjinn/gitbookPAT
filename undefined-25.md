# **Azure 마켓플레이스에서 OwlDB** 구독

1. Azure 마켓플레이스([https://azuremarketplace.microsoft.com](https://azuremarketplace.microsoft.com/))에서 OwlDB를 검색하고 선택합니다.
2. **Get It Now 및 Continue**를 클릭하여 Subscription을 구독합니다.
3. **Create**을 클릭하여 OwlDB 배포를 위한 설정을 시작합니다.

## 

---

# **ARM(Azure Resource Manager) Template을 통한 OwlDB** 배포

## 1. Project details

배포된 리소스와 비용을 관리하기 위한 구독과 리소스 그룹을 선택합니다. 리소스 그룹에는 OwlDB 배포 정보를 담는 Managed Application 리소스가 생성됩니다.

{% hint style="info" %}
**참고** OwlDB\*\* \*\*서비스 리소스는 Azure 시스템이 자동으로 생성하는 별도의 리소스 그룹에 배포되고, Publisher(운영자)에 의해 관리됩니다.
{% endhint %}

| 항목 | 설명 |
| --- | --- |
| Subscription | **Azure 구독 계정**<br>- 모든 리소스의 집합으로, 구독의 모든 리소스는 함께 청구됨 |
| Resource Group | **Azure 리소스 그룹**<br>- 동일한 수명 주기, 권한 및 정책을 공유하는 리소스 모음 |

## 2. Instance details

OwlDB 배포를 위한 ARM(Azure Resource Manager) Template의 파라미터를 직접 지정합니다.

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| Region | OwlDB를 배포할 리전 | [제공 리전 확인](#undefined-1) |
| Project Name | 배포하고자 하는 프로젝트의 이름 | 사용 중인 프로젝트 이름과 중복 사용 불가 |
| Vnet CIDR | 신규로 생성할 VNet의 IP 주소 범위(CIDR block) |   |
| Availability Zone | 인프라 리소스를 배포할 대상 가용 영역(Availability Zone) | 선택한 리전(Region) 내 AZ |
| Public Subnet CIDR | 지정한 VNet 내에서 사용할 Public Subnet의 CIDR 범위<br>* Public Subnet : 인터넷 게이트웨이를 통해 외부 통신이 가능한 네트워크 | VPC CIDR에 포함되는 범위여야 함 |
| App Gateway Subnet CIDR | 지정한 VNet 내에서 사용할 Azure Application Gateway의 CIDR 범위 | VPC CIDR에 포함되는 범위여야하고 Public Subnet CIDR와 달라야 함 |
| OwlDB Ingress CIDR | OwlDB 인스턴스에 대한 인바운드 접속을 허용할 IP 주소 범위(CIDR) | 접근 제한 불필요할 경우, 0.0.0.0/0 입력 |
| SSH Public Key | OwlDB 인스턴스에 SSH로 접근하기 위한 Key Pair 이름 | • SSH Key 로 미리 생성되어 있어야 함<br>• PEM 파일은 로컬에 보관 필요 |
| OwlDB Root Username | OwlDB에 로그인하기 위한 기본 관리자 계정의 ID | **Default : admin**<br>설정 이후 변경 불가 |
| User Email | 서비스 이용과 계정 관리를 위한 이메일 | 개인 정보 이용 동의 필요 |

## 3. Managed Application Details

애플리케이션의 고유 식별자와 효율적인 리소스 관리를 위해 리소스 그룹을 지정합니다.&nbsp;&nbsp;

| 항목 | 설명 |
| --- | --- |
| Application Name | 애플리케이션 고유 식별자 |
| Managed Resource Group | 리소스 관리를 위한 그룹 |

## 

---

# **OwlDB 배포 이후 필수 작업**

## **SSH Key 생성**

OwlDB 배포 이후, 자동으로 생성된 OwlDB 서비스 리소스 그룹 내에 SSH Key 리소스를 반드시 생성해야 합니다. SSH Key는 OwlDB의 데이터베이스 프로비저닝 과정에 사용됩니다.&nbsp;&nbsp;

1. Azure Portal([https://portal.azure.com/#home](https://portal.azure.com/#home))에서 **SSH keys**를 검색하고 선택합니다.
2. 좌측 상단 **Create**을 클릭하여 SSH Key를 생성합니다. Project details에서 OwlDB 생성 후 자동으로 생성된 리소스 그룹을 선택합니다. Instance details에서 원하는 Key pair name과 type을 선택합니다. Tags에서 효율적인 리소스 관리를 위한 Tag를 설정합니다.
3. **Next**를 클릭하여 생성을 완료하고, SSH Key 파일을 다운로드할 수 있습니다.

{% hint style="warning" %}
**주의** 보안을 위해 SSH Key 파일은 1회만 다운로드할 수 있습니다. 다운로드한 파일은 반드시 안전한 위치에 보관하시기 바랍니다.
{% endhint %}

## 

---

# **OwlDB 접속 안내**

## 최초 접속

마켓플레이스에서 OwlDB 배포가 완료되면, [**OwlDB 배포 시 입력한 메일 계정**](#id-2.-instance-details)으로 OwlDB 접속 주소와 계정 정보가 포함된 안내 메일이 발송됩니다. 해당 메일을 통해 OwlDB에 접속할 수 있습니다. 메일이 수신되지 않을 경우, [azure_owldb_support@tibero.com](mailto:azure_owldb_support@tibero.com)으로 문의 주시기 바랍니다.

## **URL 접속**

{% hint style="success" %}
[https://고객별](https://xn--i49alow70b) 고유한 [도메인값.owl-db.com](http://xn--639ay9l9xfq8o.owl-db.com)
{% endhint %}

고객별 고유한 도메인값은

1. **Azure 포털 > OwlDB 리소스 그룹 > Settings - Deployments > OwlDB에 해당하는 Deployment > Outputs > sub Domain Name**에서 확인하거나
2. **DNS zones** 서비스의 리소스 중 owl db에 해당하는 호스팅 영역 이름을 참조하시면 됩니다.
