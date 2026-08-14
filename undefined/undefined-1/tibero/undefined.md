# 데이터베이스 서버 공통 준비 사항

OwlDB에서 관제하는 데이터베이스 서버에 해당하는 공통 준비 절차입니다. 디스크 요구사항, 네트워크 설정, 배포 파일 구성 등 구성 방식에 따른 다른 절차는 각각 [설치 DB 환경 준비 가이드](undefined.md#W2TxdEHwoC3mStsQfJdo)와 [등록 DB 환경 준비 가이드](undefined.md#pvI81bWJlNtie4nv4stI)를 참고합니다.

### Timezone 설정

데이터베이스 서버의 Timezone을 올바르게 설정합니다. TAC, DR 등 다중 노드 구성의 경우 **모든 노드의 Timezone이 반드시 동일**해야 합니다. Timezone이 다를 경우 데이터 정합성 문제 및 로그 시간 불일치가 발생할 수 있습니다.

```bash
# 현재 Timezone 확인
timedatectl

# Timezone 설정 (예: Asia/Seoul)
sudo timedatectl set-timezone Asia/Seoul

# 설정 확인
timedatectl
```

### OS 사용자 / SSH 키 설정

{% hint style="info" %}
**참고**

해당 내용은 **DR 구성을 사용하는 경우**에만 필요합니다.
{% endhint %}

DR 구성에서는 OwlDB가 노드 간 SCP를 통해 데이터 파일을 전송합니다. 이를 위해 각 노드에 전용 OS 사용자를 생성하고, 패스워드 없이 SSH 접속이 가능하도록 공용 키페어를 배포합니다.

이하 절차에서 사용하는 예시 값은 다음과 같습니다. 실제 환경에 맞게 치환하여 사용하시기 바랍니다.

* DB OS 사용자: `tibero`
* SSH 포트: `22`
* 데이터베이스 노드 Node1 (Primary): `10.10.0.11` Node2 (Standby): `10.10.0.12` Node3 (Standby): `10.10.0.13`
* 클러스터 내부 네트워크 CIDR: `10.10.0.0/16`

#### 1. DB 전용 OS 사용자 생성

모든 노드에서 **동일한 UID/GID**로 사용자를 생성합니다.

```bash
# 각 노드에서 root 계정으로 실행
groupadd -g 1100 dba
useradd  -u 1100 -g dba -m -s /bin/bash tibero
passwd tibero
```

{% hint style="warning" %}
**주의**

UID/GID가 노드 간에 다르면 SCP로 전송된 파일의 소유권이 어긋나 DB 프로세스가 파일을 읽을 수 없습니다. 명시적으로 동일한 숫자를 지정하여 생성합니다.
{% endhint %}

#### 2. SSH 키페어 생성 (Node 1에서만 1회 수행)

Node1(`10.10.0.11`)에서 단 1회 키페어를 생성합니다. **이 키페어가 전체 노드의 공용 키가 됩니다.**

```bash
# node1에서 tibero 계정으로 실행
su - tibero
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -N "" -f ~/.ssh/id_ed25519 -C "owldb-shared-key"
```

| 옵션           | 설명                                 |
| ------------ | ---------------------------------- |
| `-t ed25519` | 권장 알고리즘. RSA 사용 시 `-t rsa -b 4096` |
| `-N ""`      | 자동화 SCP를 위해 passphrase 없음          |
| `-C`         | 키 식별용 주석                           |

{% hint style="warning" %}
**주의**

개인키도 모든 노드에 배포되어야 합니다. 모든 노드가 서로에게 SCP를 수행해야 하므로(node1 → {node2, node3}, node2 → {node1, node3}, …), **모든 노드가 SSH 클라이언트 역할을 겸합니다.** SSH 클라이언트 측에서 챌린지에 서명하려면 개인키가 반드시 로컬에 존재해야 합니다.
{% endhint %}

#### 3. authorized\_keys 구성

생성된 공개키를 `authorized_keys`에 등록합니다.

```bash
# node1에서 tibero 계정으로 실행
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

#### 4. 키 셋 배포

각 Standby 노드에서 node1의 키 파일을 SCP로 가져옵니다.

```bash
# 각 Standby 노드(node2, node3)에서 tibero 계정으로 실행
mkdir -p ~/.ssh && chmod 700 ~/.ssh

scp -P 22 tibero@10.10.0.11:~/.ssh/id_ed25519      ~/.ssh/id_ed25519      # 개인키
scp -P 22 tibero@10.10.0.11:~/.ssh/id_ed25519.pub  ~/.ssh/id_ed25519.pub  # 공개키

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519 ~/.ssh/authorized_keys
```

배포 완료 후 모든 노드의 `~/.ssh/` 권한 상태를 확인합니다.

```
drwx------   tibero:dba  ~/.ssh
-rw-------   tibero:dba  ~/.ssh/id_ed25519
-rw-r--r--   tibero:dba  ~/.ssh/id_ed25519.pub
-rw-------   tibero:dba  ~/.ssh/authorized_keys
```

#### 5. known\_hosts 사전 등록

자동화 스크립트 실행 시 host key prompt가 발생하는 것을 방지하기 위해, 각 노드에서 클러스터 전 노드의 호스트 공개키를 미리 수집합니다.

```bash
# 각 노드에서 tibero 계정으로 실행
ssh-keyscan -p 22 -t ed25519 10.10.0.11 10.10.0.12 10.10.0.13 > ~/.ssh/known_hosts
chmod 644 ~/.ssh/known_hosts
```

#### 6. sshd 설정 강화

보안 강화를 위해 아래 sshd 설정을 적용합니다.

본 매뉴얼의 SSH 공용 키 인증이 정상 동작하기 위한 **필수 항목**과, 보안 강화를 위한 **권장 항목**을 함께 정리합니다. 모든 노드에서 동일하게 적용하시기 바랍니다.

**1) 설정 항목**

| 항목                       | 권장값                    | 구분     | 설명                                                                 |
| ------------------------ | ---------------------- | ------ | ------------------------------------------------------------------ |
| `PubkeyAuthentication`   | `yes`                  | **필수** | 공개키 기반 인증 허용. 본 매뉴얼의 인증 방식이므로 반드시 활성화                              |
| `AuthorizedKeysFile`     | `.ssh/authorized_keys` | **필수** | 사용자별 공개키 목록 파일 경로. sshd 기본값이며, 변경 시 3 및 4단계에서 사용하는 파일 위치도 함께 변경 필요 |
| `StrictModes`            | `yes`                  | 권장     | 사용자 홈 디렉터리 및 키 파일 권한 검증. 권한이 느슨하면 인증 거부. 4단계에서 명시한 권한 값 준수가 필요한 이유 |
| `PermitRootLogin`        | `no`                   | 권장     | root 계정 SSH 접속 차단으로 공격 표면 축소                                       |
| `PasswordAuthentication` | `no`                   | 권장     | 비밀번호 기반 인증 차단으로 공개키 인증만 허용. 무차별 대입 공격 차단                           |
| `AllowUsers`             | `tibero`               | 권장     | DB 전용 OS 사용자만 SSH 접속 허용, 다른 계정 접근 차단                               |

**2) 적용 방법 (필요 시)**

`/etc/ssh/sshd_config` 또는 `/etc/ssh/sshd_config.d/` 하위의 해당 항목을 편집한 후 sshd를 reload하여 적용합니다.

```conf
# /etc/ssh/sshd_config 권장 설정 예시
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
StrictModes yes
PermitRootLogin no
PasswordAuthentication no
AllowUsers tibero
```

```bash
# 설정 reload (sshd 프로세스 재시작 없이 설정만 다시 읽음)
systemctl reload sshd      # RHEL/CentOS/Rocky 계열  
# systemctl reload ssh       # Ubuntu/Debian 계열

# 적용 검증 — Active: active (running) 이면 reload 성공
systemctl status sshd      
# systemctl status ssh
```

{% hint style="warning" %}
**주의**

원격 SSH 세션에서 sshd 설정을 변경하는 경우, 잘못된 설정으로 sshd가 reload에 실패하면 추가 SSH 접속이 거부될 수 있습니다. 작업 시에는 콘솔 접근 수단을 별도로 확보한 상태에서 진행하시기 바랍니다.

변경 적용 직후 반드시 `systemctl status` 명령으로 sshd가 정상 동작 중인지 확인하시기를 권장합니다. (`Active: failed` 또는 에러 메시지 출력 시 즉시 설정을 되돌려야 합니다)
{% endhint %}

#### 7. Source IP 제한

`authorized_keys` 항목 앞에 `from=` 옵션을 부여하여, 클러스터 내부 네트워크에서만 키가 유효하도록 제한합니다.

```
from="10.10.0.0/16,127.0.0.1" ssh-ed25519 AAAA... owldb-shared-key
```

{% hint style="warning" %}
**주의**

본 키는 반드시 DB 전용 OS 사용자(`tibero`)로만 사용하며, **root** 계정의 SSH 키로 절대 공용화하지 않습니다. 복구 작업 및 SCP가 root 권한을 필요로 하지 않도록 데이터 디렉터리 소유권을 사전에 정리합니다.
{% endhint %}
