이 페이지에서는 데이터베이스 서버에 Agent를 설치하고 기동하는 방법을 설명합니다.

{% hint style="info" %}
**참고**

본 가이드는 [설치 DB 환경 준비 가이드](#W2TxdEHwoC3mStsQfJdo) 또는 [등록 DB 환경 준비 가이드](#pvI81bWJlNtie4nv4stI)를 완료한 후 진행합니다.
{% endhint %}

## Agent 설치 및 기동

### 1. Agent 바이너리 압축 해제

```bash
tar -zxvf $TB_HOME/tbagent_dist_latest.tar.gz -C $TB_HOME
```

### 2. agent.env 파일 작성

agent 기동에 필요한 정보를 env 파일에 입력합니다.

```bash
############################################################
#                      AGENT ENV FILE                      #
#----------------------------------------------------------#
# This file contains environment variables for the agent  #
# Do NOT add spaces around '='                            #
# Lines starting with '#' are comments                    #
############################################################

# ------------------------------
# Network Configuration (OwlDB)
# ------------------------------
IP=
PORT=

# ------------------------------
# User Configuration
# ------------------------------
USERNAME=

# ------------------------------
# Tibero Configuration
# ------------------------------
TB_HOME=

# ------------------------------
# Only Register Variables [OPTIONAL]
# ------------------------------
TB_SID=
TAS_SID=
CM_SID=
CM_HOME=

############################################################
# End of agent.env
############################################################
```

파라미터는 구성 방식에 따라 입력 여부가 다릅니다.

{% tabs %}
{% tab title="설치 DB" %}
| 옵션  | 설명  | 필수  |
|-----|-----|-----|
| `IP` | OwlDB 서버 IP 주소 | 필수  |
| `PORT` | OwlDB 서버 포트 (SERVER_PORT) | 필수  |
| `USERNAME` | DB OS 사용자 명 | 필수  |
| `TB_HOME` | Tibero 홈 디렉터리 경로 | 필수  |
| `TB_SID` | TB_SID 값 | 설치 시 불필요 |
| `TAS_SID` | TAS_SID 값 | 설치 시 불필요 |
| `CM_SID` | CM_SID 값 | 설치 시 불필요 |
| `CM_HOME` | CM_HOME 값 | 설치 시 불필요 |

{% hint style="info" %}
**참고**

`TB_SID` 등 DB 식별 값은 OwlDB가 설치 과정에서 자동으로 설정합니다. 설치 전 단계에서는 입력하지 않아도 됩니다.
{% endhint %}
{% endtab %}
{% tab title="등록 DB" %}
| 옵션  | 설명  | 필수  |
|-----|-----|-----|
| `IP` | OwlDB 서버 IP 주소 | 필수  |
| `PORT` | OwlDB 서버 포트 (SERVER_PORT) | 필수  |
| `USERNAME` | Tibero를 설치한 OS 사용자 명 | 필수  |
| `TB_HOME` | Tibero 홈 디렉터리 경로 | 필수  |
| `TB_SID` | TB_SID 값 | **필수** |
| `TAS_SID` | TAS_SID 값 | - TAC 구성 시 필수<br>- 미사용 시 불필요 |
| `CM_SID` | CM_SID 값 | - CM 구성 시 필수<br>- 미사용 시 불필요 |
| `CM_HOME` | CM_HOME 값 | - CM 구성 시 필수<br>- 미사용 시 불필요 |

{% hint style="warning" %}
**주의**

등록 DB는 기존에 운영 중인 데이터베이스를 대상으로 하므로, `TB_SID`는 반드시 기존 Tibero 인스턴스의 SID 값을 입력해야 합니다.
{% endhint %}
{% endtab %}
{% endtabs %}

### 3. Agent 설치 스크립트 실행

```bash
cd $TB_HOME/tbagent_dist
sh tbagent_start.sh
```

{% hint style="warning" %}
**주의**

`tbagent_start.sh` 스크립트는 Agent를 systemd 서비스 및 타이머로 등록하며, 이 과정에서 sudo 권한이 사용됩니다.

Agent를 재기동하는 경우 [참고 자료 > Agent 재기동 시 주의 사항](#nql9NMu6dh160KsnoVKa)을 반드시 확인합니다.
{% endhint %}

### 4. OS user sudoers 설정

데이터베이스 설치 및 운영 과정에서 일부 명령어를 sudo로 실행합니다. 스크립트 실행 중 비밀번호 입력이 요구되면 작업이 중단될 수 있으므로, 설치에 사용할 OS user에 NOPASSWD를 설정합니다.

```bash
# 1. sudoers 설정 파일 생성
sudo tee /etc/sudoers.d/{username} << 'EOF'
{username} ALL=(ALL) NOPASSWD: \
        /usr/bin/systemctl \
        /usr/bin/timedatectl \
        /usr/sbin/udevadm \
        /usr/bin/dd \
        /usr/bin/tar \
        /usr/bin/mkdir \
        /usr/bin/rm \
        /usr/bin/unlink \
        /usr/bin/ln \
        /usr/bin/chown \
        /usr/bin/chmod \
        /usr/bin/tee \
        /usr/bin/sed \
        /usr/bin/bash \
        /usr/bin/su \
        /usr/lib/udev/scsi_id \
        /usr/bin/grep \
        /usr/bin/touch \
        /usr/bin/kill \
    )
EOF

# 2. 파일 권한 설정
sudo chmod 440 /etc/sudoers.d/{username}

# 3. 적용 확인
sudo -l -U {username}
```
