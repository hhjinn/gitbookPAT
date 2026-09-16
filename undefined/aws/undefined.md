# OwlDB 구독 전 필수 작업

이 페이지에서는 OwlDB를 구독하기 전에 반드시 완료해야 하는 OS 이미지 구독과 SSH Key 생성 작업을 설명합니다.

## 1. OS 이미지 구독

OwlDB를 통해 데이터베이스를 구축하기 위해선 `Rocky Linux 9 (Official) - x86_64` AMI에 대한 사전 구독이 필요합니다. 구독 방법은 다음과 같습니다.

1. [AWS 마켓플레이스](https://aws.amazon.com/marketplace)에서 [Rocky Linux 9 (Official) - x86_64](https://aws.amazon.com/marketplace/pp/prodview-ygp66mwgbl2ii)을 검색하고 선택합니다.
2. **Continue to Subscribe**를 클릭하여 Subscription을 구독합니다.

## 2. SSH Key 생성

OwlDB 구독 전, SSH Key pair 리소스를 반드시 생성해야 합니다. SSH Key pair는 OwlDB의 배포와 데이터베이스 프로비저닝 과정에 사용됩니다.

1. [AWS Console](https://console.aws.amazon.com/console/home)에서 Key pairs를 검색하고 선택합니다.
2. 우측 상단 **Create key pair**를 클릭합니다.
3. Name에 원하는 Key pair 이름을 입력합니다.
4. Key pair type을 선택합니다. 호환성을 위해 기본 설정인 RSA를 권장합니다.
5. Private key file format을 선택합니다. 호환성을 위해 기본 설정인 .pem 포맷을 권장합니다.
6. Tags에서 효율적인 리소스 관리를 위한 Tag를 설정합니다.
7. **Create key pair**를 클릭하여 생성을 완료하고 SSH Key pair 파일을 다운로드합니다.

{% hint style="warning" %}
**주의**

보안을 위해 SSH Key pair 파일은 1회만 다운로드할 수 있습니다. 다운로드한 파일은 반드시 안전한 위치에 보관합니다.
{% endhint %}

---

# AWS 마켓플레이스에서 OwlDB 구독

이 페이지에서는 AWS 마켓플레이스에서 OwlDB를 구독하고 배포 설정을 시작하는 방법을 설명합니다.

1. [AWS 마켓플레이스](https://aws.amazon.com/marketplace)에서 OwlDB를 검색하고 선택합니다.
2. **Continue to Subscribe**를 클릭하여 Subscription을 구독합니다.
3. **Continue to Configuration**을 클릭하여 OwlDB 배포를 위한 설정을 시작합니다.

---

# AWS CloudFormation을 통한 OwlDB 배포

이 페이지에서는 AWS CloudFormation을 사용하여 OwlDB를 배포하는 과정을 단계별로 설명합니다.

## 1. Configure this software

- 아래 항목을 선택하고, **Continue to Launch**를 클릭합니다.

| 항목 | 옵션 |
| --- | --- |
| Fulfillment option | OwlDB for Tibero7 |
| Software version | 1.2.0 (Dec 29, 2025) |
| Region | 선택 |

## 2. Launch this software

- Configuration 세부 항목을 확인합니다.
- Choose Action에서 Launch CloudFormation을 선택하고, **Launch**를 클릭합니다.

## 3. CloudFormation 스택 생성

CloudFormation 스택 생성은 아래 네 단계를 순서대로 모두 거쳐야 합니다.

### 3-1. 스택 생성

{% hint style="warning" %}
**주의**

해당 단계에서는 기본으로 선택된 옵션을 그대로 사용해야 합니다.
{% endhint %}

사전 조건의 템플릿 준비 항목에서는 기존 템플릿 선택을 사용합니다.

**템플릿 지정**

| 항목 | 옵션 |
| --- | --- |
| 템플릿 소스 | Amazon S3 URL |
| Amazon S3 URL | Default 템플릿 그대로 사용 |

### 3-2. 스택 세부 정보 지정

**스택 이름 제공**

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| 스택 이름 | 배포하고자 하는 스택의 고유식별자 | 사용 중인 스택 이름과 중복 사용 불가 |

**파라미터 [Fulfillment option : Deploy into new VPC]**

OwlDB Infra Setting

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| VPC CIDR | 신규로 생성할 VPC의 IP 주소 범위(CIDR block) 정의 | 기 존재하는 VPC와 동일하거나 겹치는 범위 사용 불가 |
| Availability Zone 1 | 인프라 리소스를 배포할 대상 가용 영역(Availability Zone) | 선택한 리전(Region) 내 AZ이어야 함 |
| Public Subnet 1 CIDR | - 지정한 VPC 내에서 사용할 Public Subnet의 CIDR 범위<br>- Public Subnet : 인터넷 게이트웨이를 통해 외부 통신이 가능한 네트워크 | VPC CIDR에 포함되는 범위여야 함 |
| Availability Zone 2 | 인프라 리소스를 배포할 대상 가용 영역(Availability Zone) | 선택한 리전(Region) 내 AZ이어야 하고 Availability Zone 1과 달라야 함 |
| Public Subnet 2 CIDR | 지정한 VPC 내에서 사용할 Public Subnet2의 CIDR 범위 | VPC CIDR에 포함되는 범위여야 하고 Public Subnet 1 CIDR와 달라야 함 |
| CIDR Range for OwlDB Access | OwlDB 인스턴스에 대한 인바운드 접속을 허용할 IP 주소 범위(CIDR) | 접근 제한 불필요할 경우, 0.0.0.0/0 입력 |
| KeyPair Name | OwlDB 인스턴스에 SSH로 접근하기 위한 Key Pair 이름 | - 해당 키는 AWS EC2 Key Pair로 미리 생성되어 있어야 함<br>- PEM 파일은 로컬에 보관 필요 |
| Image Id | OwlDB가 구축될 Image의 Id | Default 그대로 사용 |

OwlDB Setting

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| OwlDB Root User Name | OwlDB에 로그인하기 위한 기본 관리자 계정의 ID | - Default : admin<br>- 설정 이후 변경 불가 |

Personal Information

| 항목 | 설명 | 비고 |
| --- | --- | --- |
| User Email | 서비스 이용과 계정 관리를 위한 이메일 | 개인 정보 이용 동의 필요 |
| Consent to Personal Data Use | 개인 정보 이용 동의 | - |

### 3-3. 스택 옵션 구성

{% hint style="info" %}
**참고**

해당 단계에서는 기본으로 선택된 옵션을 그대로 사용합니다.
{% endhint %}

| 항목 | 설명 |
| --- | --- |
| 태그 (선택 사항) | 리소스 구성·식별·분류용 태그 추가(스택당 최대 50개) |
| 권한 (선택 사항) | IAM으로 스택에서 사용할 역할 지정 |
| 스택 실패 옵션 | 프로비저닝 실패 동작 및 롤백 중 생성된 리소스 삭제 방식 선택 |
| 추가 설정 (선택 사항) | 스택 알림 옵션·정책 등 추가 옵션 설정 |
| 기능 | CloudFormation의 IAM 리소스 생성 승인 |

### 3-4. 검토 및 작성

지정한 템플릿과 스택 세부 정보를 확인하고 검토합니다.

**전송**을 클릭하여 프로비저닝을 시작합니다.

---

# OwlDB 접속 안내

이 페이지에서는 OwlDB 배포 완료 후 최초 접속 방법과 접속 URL을 확인하는 방법을 설명합니다.

## 최초 접속

마켓플레이스에서 OwlDB 배포가 완료되면, 스택 생성 시 입력한 이메일로 OwlDB 접속 주소와 계정 정보가 포함된 안내 메일이 발송됩니다. 해당 메일을 통해 OwlDB에 접속합니다. 메일이 수신되지 않을 경우, [aws_owldb_support@tibero.com](mailto:aws_owldb_support@tibero.com)으로 문의합니다.

## URL 접속

{% hint style="info" %}
**참고**

접속 주소는 `https://<고객별-고유-도메인>.owl-db.com` 형식입니다.
{% endhint %}

고객별 고유한 도메인 값은 다음 방법으로 확인합니다.

1. CloudFormation 콘솔 > 스택 > 스택 이름 > 출력 탭 > ManagementPlaneStackId에서 확인합니다.
2. Route 53 서비스의 호스팅 영역 중 owl db에 해당하는 호스팅 영역 이름을 참조합니다.
