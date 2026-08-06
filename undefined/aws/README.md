# AWS

AWS 마켓플레이스에서 OwlDB를 이용하기 위한 정보를 확인합니다.

# 클라우드 환경 및 서버 사양

OwlDB가 제공되는 클라우드 환경과 서버 사양을 확인합니다.

| 항목 | 상세 |
| --- | --- |
| 컴퓨팅 사양 | 2 vCPU, Memory 8 GiB |
| 운영 체제 | Rocky 9.5 |
| 스토리지 엔진 | Amazon gp3 |
| 지원 언어 | 한국어, 영어 |
| 권장 브라우저 | Google Chrome |
| 최적 해상도 | Full HD (1920*1080) |

# 리전 가용성

리전별 지원 여부를 확인합니다.

| 리전 이름 | 지역 |
| --- | --- |
| 미국 서부(오레곤) | us-west-2 |
| 미국 서부(캘리포니아 북부) | us-west-1 |
| 미국 동부(오하이오) | us-east-2 |
| 미국 동부(버지니아 북부) | us-east-1 |
| 남아메리카(상파울루) | sa-east-1 |
| 유럽(파리) | eu-west-3 |
| 유럽(런던) | eu-west-2 |
| 유럽(아일랜드) | eu-west-1 |
| 유럽(스톡홀름) | eu-north-1 |
| 유럽(프랑크푸르트) | eu-central-1 |
| 캐나다(중부) | ca-central-1 |
| 아시아 태평양(시드니) | ap-southeast-2 |
| 아시아 태평양(싱가포르) | ap-southeast-1 |
| 아시아 태평양(뭄바이) | ap-south-1 |
| 아시아 태평양(오사카) | ap-northeast-3 |
| 아시아 태평양(서울) | ap-northeast-2 |
| 아시아 태평양(도쿄) | ap-northeast-1 |

# 인스턴스 유형

선택 가능한 인스턴스 유형별 vCPU와 메모리 사양을 확인합니다.

| 인스턴스 유형 | vCPU | Memory (GiB) |
| --- | --- | --- |
| t3.medium | 2 | 4 |
| t3.large | 2 | 8 |
| t3.xlarge | 4 | 16 |
| t3.2xlarge | 8 | 32 |
| m6i.large | 2 | 8 |
| m6i.xlarge | 4 | 16 |
| m6i.2xlarge | 8 | 32 |
| m6i.4xlarge | 16 | 64 |
| m6i.8xlarge | 32 | 128 |
| m6i.12xlarge | 48 | 192 |
| m6i.16xlarge | 64 | 256 |
| m6i.24xlarge | 96 | 384 |
| r6i.large | 2 | 16 |
| r6i.xlarge | 4 | 32 |
| r6i.2xlarge | 8 | 64 |
| r6i.4xlarge | 16 | 128 |
| r6i.8xlarge | 32 | 256 |
| r6i.12xlarge | 48 | 384 |
| r6i.16xlarge | 64 | 512 |
| r6i.24xlarge | 96 | 768 |
| r5.large | 2 | 16 |
| r5.xlarge | 4 | 32 |
| r5.2xlarge | 8 | 64 |
| r5.4xlarge | 16 | 128 |
| r5.8xlarge | 32 | 256 |
| r5.12xlarge | 48 | 384 |
| r5.16xlarge | 64 | 512 |
| r5.24xlarge | 96 | 768 |

{% hint style="info" %}
**참고**

- **Tibero Single**: large 이상 인스턴스 유형을 권장합니다.
- **Tibero TAC**: large 이상만 사용 가능하며, xlarge 이상을 권장합니다.
{% endhint %}

# 스토리지/디스크 유형

OwlDB에서 사용 가능한 스토리지 유형과 각 유형의 용량·IOPS 범위를 확인합니다.

| 유형 | 특성 | 볼륨 크기(GiB) | 볼륨 IOPS(개) |
| --- | --- | --- | --- |
| gp3 | - SSD 기반 볼륨<br>- 높은 IOPS, 낮은 대기시간<br>- 용량당 비용 저렴 | 100 ~ 65,536 | 3,000 ~ 80,000 |
| gp2 | - SSD 기반 볼륨<br>- 대량 데이터 저장·처리에 적합<br>- 용량당 비용 저렴<br>- 일관성 편차 있음<br>- IOPS는 할당된 스토리지 크기에 따라 변경, 사용자 설정 불가 | 100 ~ 16,384 | 450 ~ 16,000 |
| io2 | - SSD 기반 볼륨<br>- 매우 높은 IOPS, 일관된 성능<br>- 용량당 비용 높음<br>- 대기시간 상대적으로 김 | 100 ~ 65,536 | 3,000 ~ 256,000 |

{% hint style="info" %}
**참고**

Tibero TAC 토폴로지는 io2만 사용 가능합니다.
{% endhint %}
