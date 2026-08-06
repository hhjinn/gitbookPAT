OwlDB는 OpenSQL 데이터베이스의 백업 및 복구를 Barman(Backup and Recovery Manager)으로 수행합니다. Barman 서버는 백업 데이터와 WAL(Write-Ahead Log)을 보관하는 서버로, **OwlDB와는 별도의 서버에 설치**합니다.

아래 절차대로 설정하면 OwlDB에서 Barman 서버를 연동(link)하여 사용할 수 있습니다. 데이터베이스별 설정 파일과 Agent 프로세스는 OwlDB가 연동 시점에 자동으로 생성하므로 직접 작성하지 않습니다.

Barman과 PostgreSQL 클라이언트의 설치 방법은 OpenSQL 공식 가이드를 따릅니다. 본 문서에서는 OwlDB와 연동하기 위해 추가로 설정해야 하는 내용을 안내합니다.

본 문서의 경로와 계정명은 예시입니다. 환경에 맞게 변경할 수 있으며, 변경할 수 없는 값은 해당 단계에 별도로 표기했습니다.

## 설치 개요

설치는 다음 순서로 진행합니다.

1. Barman 설치
2. Barman 실행 계정 설정
3. Barman 전역 설정
4. Barman Agent 등록
5. Barman cron 등록
6. OwlDB Agent 설치
7. SSH 키 등록
8. 데이터베이스 서버 설정
9. 설치 결과 확인

Barman Agent의 유닛 이름과 설정 파일 경로는 OwlDB에 고정되어 있어 변경할 수 없습니다. 다른 이름이나 경로를 사용하면 연동이 실패하므로 [4. Barman Agent 등록](#4-barman-agent-등록)의 값을 그대로 사용해주십시오.

## 1. Barman 설치

Barman은 `3.11.1` 버전으로 설치합니다. 데이터베이스 서버에 설치하는 `barman-cli` 도 동일하게 `3.11.1` 이어야 합니다.

{% hint style="warning" %}
**주의**

WAL을 보내는 쪽과 받는 쪽의 버전이 다르면 연동 및 백업이 실패합니다.
{% endhint %}

| 설치 위치 | 패키지 | 버전 | 제공 명령 | 역할 |
| --- | --- | --- | --- | --- |
| Barman 서버 | `barman` | `3.11.1` | `barman put-wal`, `barman get-wal` | WAL을 받아 보관 |
| 데이터베이스 서버 | `barman-cli` | `3.11.1` | `barman-wal-archive`, `barman-wal-restore` | WAL을 전송 |

`pip install barman` 과 같이 버전을 지정하지 않고 설치하면 최신 버전이 설치되어 `3.11.1` 과 어긋납니다. 설치 시 반드시 버전을 명시해주십시오.

설치 후 버전을 확인합니다.

```bash
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)
```

Barman 서버에는 `barman-cli` 패키지를 별도로 설치하지 않아도 됩니다. Barman 설치 시 `barman-wal-archive` 등의 실행 파일이 함께 배치됩니다.

## 2. Barman 실행 계정 설정

Barman을 실행할 전용 OS 계정을 생성하고 NOPASSWD sudo 권한을 부여합니다.

```bash
groupadd -g 1001 barman
useradd -u 1001 -g 1001 -m -d /var/lib/barman -s /bin/bash barman

echo "barman ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/barman
chmod 440 /etc/sudoers.d/barman
```

| 항목 | 설명 | 예시 | 필수 여부 |
| --- | --- | --- | --- |
| 계정 | Barman 실행 전용 OS 계정 | `barman` | 필수 |
| 홈 디렉토리 | - 계정 홈 디렉토리<br>- 백업 데이터 저장 경로 | `/var/lib/barman` | 필수 |
| sudo 권한 | NOPASSWD sudo | `barman ALL=(ALL) NOPASSWD: ALL` | 필수 |

OwlDB는 Barman Agent를 `sudo systemctl restart barman-agent@<데이터베이스 ID>` 로 제어합니다. 이 명령이 비밀번호 입력 없이 수행되어야 하므로 NOPASSWD sudo 권한을 설정해주십시오.

Barman 서버는 이 계정 하나로 동작합니다. 백업 수행, OwlDB Agent 실행, 데이터베이스 서버 접속이 모두 같은 계정으로 이루어집니다.

## 3. Barman 전역 설정

`/etc/barman.conf` 파일을 아래와 같이 작성합니다.

```ini
[barman]
path_prefix = /opt/postgresql/bin
barman_user = barman
configuration_files_directory = /etc/barman.d
barman_home = /var/lib/barman
log_file = /var/log/barman/barman.log
log_level = INFO
```

| 항목 | 설명 | 예시 | 필수 여부 |
| --- | --- | --- | --- |
| path_prefix | PostgreSQL 클라이언트 실행 파일 경로 | `/opt/postgresql/bin` | 필수 |
| barman_user | Barman 실행 계정 | `barman` | 필수 |
| configuration_files_directory | - 데이터베이스별 설정 파일 디렉토리<br>- OwlDB Agent의 `BARMAN_CONF_DIR` 과 동일 값 | `/etc/barman.d` | 필수 |
| barman_home | 백업 데이터 저장 경로 | `/var/lib/barman` | 필수 |
| log_file | 로그 파일 경로 | `/var/log/barman/barman.log` | 필수 |
| log_level | 로그 레벨 | `INFO` | 필수 |

`path_prefix` 는 반드시 설정해주십시오. Barman cron은 최소한의 PATH 환경에서 실행되므로 계정의 `.bashrc` 등에 설정한 PATH가 적용되지 않습니다.

{% hint style="warning" %}
**주의**

`path_prefix` 가 없으면 cron이 `pg_receivewal` 을 찾지 못해 WAL 수집이 영구적으로 실패합니다.
{% endhint %}

설정에 사용한 디렉토리를 생성하고 소유권을 지정합니다.

```bash
mkdir -p /var/log/barman /etc/barman.d /var/lib/barman/agent

chown -R barman:barman /var/lib/barman /var/log/barman /etc/barman.d
chmod 700 /var/lib/barman
```

| 디렉토리 | 용도 |
| --- | --- |
| `/etc/barman.d` | - 데이터베이스별 Barman 설정 파일<br>- 연동 시 OwlDB가 생성 |
| `/var/lib/barman/agent` | - 데이터베이스별 Agent 설정 파일<br>- 연동 시 OwlDB가 생성 |
| `/var/log/barman` | Barman 로그 |

`barman_home` 은 설정한 값을 OwlDB가 연동 시점에 조회하여 사용합니다. 백업 데이터는 `<barman_home>/<데이터베이스 ID>` 에 저장됩니다. 단 [4번](#4-barman-agent-등록)의 Agent 설정 파일 경로는 `barman_home` 과 무관하게 고정입니다.

## 4. Barman Agent 등록

Barman Agent는 Patroni 클러스터의 리더 변경을 감지하여 Barman 설정을 갱신하는 프로세스입니다. OpenSQL 배포본의 `barman_agent/server.py` 를 사용합니다.

### 4-1. 실행 파일 배치

```bash
cp <OpenSQL 배포본 경로>/barman_agent/server.py /usr/local/bin/barman-agent
chmod 755 /usr/local/bin/barman-agent

pip3 install --no-cache-dir -r <OpenSQL 배포본 경로>/barman_agent/requirements.txt
```

### 4-2. systemd 템플릿 유닛 등록

하나의 Barman 서버가 여러 데이터베이스를 담당할 수 있으므로, 데이터베이스별로 Agent가 기동되도록 systemd **템플릿 유닛**으로 등록합니다. 인스턴스 이름(`%i`)에는 OwlDB의 데이터베이스 ID가 전달됩니다.

`/usr/lib/systemd/system/barman-agent@.service` 파일을 아래와 같이 작성합니다.

```ini
[Unit]
Description=Barman Agent for %i
After=network.target

[Service]
Type=simple
User=barman
Group=barman
ExecStart=/usr/local/bin/barman-agent /var/lib/barman/agent/%i.config.yml
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

아래 두 값은 OwlDB에 고정되어 있으므로 그대로 사용해주십시오.

| 항목 | 값 | 변경 가능 여부 |
| --- | --- | --- |
| 유닛 이름 | `barman-agent@.service` | 불가 |
| Agent 설정 파일 경로 | `/var/lib/barman/agent/<데이터베이스 ID>.config.yml` | 불가 |

`barman_home` 을 `/var/lib/barman` 이외의 경로로 설정한 경우에도 Agent 설정 파일 경로는 `/var/lib/barman/agent/` 로 고정입니다.

{% hint style="warning" %}
**주의**

`ExecStart` 경로를 `barman_home` 기준으로 작성하면 OwlDB가 생성한 설정 파일을 읽지 못해 Agent가 기동되지 않습니다.
{% endhint %}

설정 파일의 내용(클러스터 이름, Patroni 접속 정보, listen 포트)은 OwlDB가 연동 시점에 생성합니다. 이 단계에서는 유닛 파일만 작성하고 기동하지 않습니다.

## 5. Barman cron 등록

WAL 수집과 아카이빙이 1분 주기로 수행되도록 cron을 등록합니다.

`/etc/cron.d/barman-cron` 파일을 아래와 같이 작성합니다.

```cron
* * * * * barman /usr/local/bin/barman cron > /var/log/barman/cron.log 2>&1
```

```bash
chmod 644 /etc/cron.d/barman-cron
systemctl enable --now crond
```

| 항목 | 설명 | 예시 |
| --- | --- | --- |
| 주기 | 1분 | `* * * * *` |
| 실행 계정 | Barman 실행 계정 | `barman` |
| 명령 | Barman cron 실행 | `/usr/local/bin/barman cron` |

cron은 최소한의 PATH 환경에서 실행되므로, `barman cron` 이 `pg_receivewal` 을 찾으려면 [3번](#3-barman-전역-설정)의 `path_prefix` 가 설정되어 있어야 합니다.

## 6. OwlDB Agent 설치

OwlDB Agent는 OwlDB 서버의 명령을 받아 Barman 서버에서 작업을 수행합니다. Barman 서버의 모든 연동 작업이 이 Agent를 통해 이루어지므로 반드시 설치해주십시오.

### 6-1. 배포 파일 압축 해제

```bash
tar xzf owlagent_dist_latest.tar.gz -C /var/lib/barman
chown -R barman:barman /var/lib/barman/owlagent_dist
```

### 6-2. owlagent.env 설정

`/var/lib/barman/owlagent_dist/owlagent.env` 파일을 아래와 같이 작성합니다.

```bash
AGENT_TYPE=barman
IP=192.168.0.10
PORT=8080
USERNAME=barman
OPENSQL_HOME=/opt/postgresql
BARMAN_NAME=barman01
BARMAN_SSH_USER=barman
BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/id_rsa
BARMAN_SSH_IP=192.168.0.20
BARMAN_SSH_PORT=22
BARMAN_CONF_DIR=/etc/barman.d
```

| 항목 | 설명 | 예시 | 필수 여부 |
| --- | --- | --- | --- |
| AGENT_TYPE | - Agent 종류<br>- Barman 서버는 `barman` 입력 | `barman` | 필수 |
| IP | OwlDB 서버 IP | `192.168.0.10` | 필수 |
| PORT | OwlDB 서버 포트 | `8080` | 필수 |
| USERNAME | Agent 실행 계정 | `barman` | 필수 |
| OPENSQL_HOME | PostgreSQL 및 Barman 설치 경로 | `/opt/postgresql` | 필수 |
| BARMAN_NAME | OwlDB에 표시되는 Barman 서버 이름 | `barman01` | 필수 |
| BARMAN_SSH_USER | Barman 서버 SSH 접속 계정 | `barman` | 필수 |
| BARMAN_SSH_KEYPATH | Barman 서버가 데이터베이스 서버에 접속할 때 사용하는 개인키 경로 | `/var/lib/barman/.ssh/id_rsa` | 필수 |
| BARMAN_SSH_IP | Barman 서버 IP | `192.168.0.20` | 필수 |
| BARMAN_SSH_PORT | Barman 서버 SSH 포트 | `22` | 필수 |
| BARMAN_CONF_DIR | - 데이터베이스별 설정 파일 디렉토리<br>- `barman.conf` 의 `configuration_files_directory` 와 동일 값 | `/etc/barman.d` | 필수 |

{% hint style="warning" %}
**주의**

`AGENT_TYPE` 이 `barman` 이 아니면 OwlDB가 Barman 서버로 인식하지 않아 백업 설정 화면의 Barman 서버 목록에 표시되지 않습니다.
{% endhint %}

### 6-3. Agent 기동

`barman` 계정으로 기동 스크립트를 실행합니다. 설정 파일 생성 → systemd 서비스 등록 → 기동이 자동으로 수행됩니다.

```bash
su - barman
cd /var/lib/barman/owlagent_dist
./owlagent_start.sh
```

등록되는 서비스는 다음과 같습니다.

| 서비스 | 역할 |
| --- | --- |
| `owldb-barman-agent.service` | Agent 데몬 |
| `owldb-barman-agent.timer` | Agent 상태를 1분 주기로 점검하여 중단 시 재기동 |

설정을 변경한 경우 `owlagent_stop.sh` 로 중지한 뒤 `owlagent_start.sh` 를 다시 실행합니다.

## 7. SSH 키 등록

Barman 서버와 데이터베이스 서버 간에 **양방향**으로 SSH 키를 등록합니다.

| 방향 | 용도 | 필수 여부 |
| --- | --- | --- |
| 데이터베이스 서버 → Barman 서버 | WAL 아카이브 전송(`barman-wal-archive`), WAL 복원(`barman-wal-restore`) | WAL 수집 방식이 ARCHIVER 또는 BOTH인 경우 필수 |
| Barman 서버 → 데이터베이스 서버 | 복구 시 데이터 파일 전송(rsync), RSYNC 백업 방식 | 복구 사용 시 필수 |

### 7-1. 데이터베이스 서버 → Barman 서버

Barman 서버에서 `.ssh` 디렉토리를 만들고, 데이터베이스 노드 등록 시 사용한 키의 공개키를 `authorized_keys` 에 등록합니다.

```bash
install -d -m 700 -o barman -g barman /var/lib/barman/.ssh

cat <데이터베이스 노드 등록 키의 공개키> >> /var/lib/barman/.ssh/authorized_keys
chmod 600 /var/lib/barman/.ssh/authorized_keys
chown barman:barman /var/lib/barman/.ssh/authorized_keys
```

### 7-2. Barman 서버 → 데이터베이스 서버

Barman 서버에 개인키를 배치합니다. 경로는 [6-2](#6-2-owlagentenv-설정)의 `BARMAN_SSH_KEYPATH` 와 같아야 합니다.

```bash
cp <개인키 파일> /var/lib/barman/.ssh/id_rsa
chmod 600 /var/lib/barman/.ssh/id_rsa
chown barman:barman /var/lib/barman/.ssh/id_rsa
```

데이터베이스 서버에서는 해당 개인키의 공개키를 OpenSQL 계정의 `authorized_keys` 에 등록합니다.

```bash
cat <공개키> >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

### 7-3. 접속 확인

양방향 모두 비밀번호나 확인 프롬프트 없이 접속되어야 합니다.

Barman 서버에서 실행합니다.

```bash
su - barman
ssh -o BatchMode=yes -i /var/lib/barman/.ssh/id_rsa <OpenSQL 계정>@<데이터베이스 서버 IP> hostname
```

데이터베이스 서버에서 실행합니다.

```bash
ssh -o BatchMode=yes -i <등록 키> barman@<Barman 서버 IP> hostname
```

OwlDB는 SSH 접속 가능 여부를 검증하지 않습니다. 설치 시점에는 어떤 Barman 서버를 사용할지 알 수 없기 때문입니다. 연동 시 OwlDB는 데이터베이스 서버의 `~/.ssh/config` 에 Barman 서버용 접속 설정만 생성하며, 키는 등록하지 않습니다.

{% hint style="warning" %}
**주의**

키가 없으면 연동은 성공한 것처럼 보이지만 WAL 전송이 `Permission denied` 로 실패하고 백업 상태가 정상으로 표시되지 않습니다.
{% endhint %}

## 8. 데이터베이스 서버 설정

Barman 서버 외에 데이터베이스 서버에도 아래 항목을 준비합니다.

| 항목 | 설명 | 필수 여부 |
| --- | --- | --- |
| `barman-cli` | `3.11.1` 버전으로 설치 | WAL 수집 방식이 ARCHIVER 또는 BOTH인 경우 필수 |
| `rsync` | 복구 시 데이터 파일 전송용 | 복구 사용 시 필수 |
| 데이터베이스 계정 | OpenSQL 설치 시 입력한 계정이 슈퍼유저로 존재 필요 | 필수 |

`barman-cli` 설치 여부를 확인합니다.

```bash
rpm -q barman-cli
barman-wal-archive --version
```

설치되지 않은 경우 아래와 같이 출력됩니다.

```bash
$ rpm -q barman-cli
package barman-cli is not installed

$ barman-wal-archive --version
bash: barman-wal-archive: command not found
```

이 경우 `barman-cli` 를 `3.11.1` 버전으로 설치합니다. `barman-cli` 는 OpenSQL과 별개의 패키지이므로 OpenSQL이 설치된 서버에도 없을 수 있습니다.

{% hint style="warning" %}
**주의**

WAL 수집 방식을 ARCHIVER 또는 BOTH로 연동하는 경우, 데이터베이스 서버에 `barman-wal-archive` 가 없으면 OwlDB가 연동을 거부합니다. WAL 전송이 영구적으로 실패하여 백업이 동작하지 않기 때문입니다.
{% endhint %}

WAL 수집 방식이 STREAMING인 경우에는 `barman-cli` 가 필요하지 않습니다.

Barman은 OpenSQL 설치 시 입력한 계정으로 데이터베이스에 접속합니다. 이 계정이 슈퍼유저이면 복제 및 백업 권한을 이미 보유하므로 별도의 전용 계정을 만들지 않아도 됩니다.

## 9. 설치 결과 확인

아래 명령으로 Barman 서버가 연동 가능한 상태인지 점검합니다.

```bash
# Barman 버전
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)

# 전역 설정
$ cat /etc/barman.conf

# Agent 템플릿 유닛
$ systemctl cat barman-agent@

# Barman cron
$ cat /etc/cron.d/barman-cron

# OwlDB Agent
$ systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer
active
active

# 서비스 상태
$ systemctl is-active sshd crond
active
active
```

| 확인 항목 | 정상 결과 |
| --- | --- |
| `barman --version` | `3.11.1` |
| `/etc/barman.conf` | `path_prefix`, `configuration_files_directory` 설정됨 |
| `barman-agent@` 유닛 | 유닛 존재, `ExecStart` 가 `/var/lib/barman/agent/%i.config.yml` 참조 |
| `/etc/cron.d/barman-cron` | 1분 주기 `barman cron` 등록 |
| `owldb-barman-agent.service` / `.timer` | 모두 `active` |
| `sshd`, `crond` | 모두 `active` |

마지막으로 OwlDB 웹 UI에서 데이터베이스의 백업 설정 화면으로 이동하여 Barman 서버 목록에 해당 서버가 표시되는지 확인합니다.

목록에 표시되면 **연동 가능한 상태**입니다. 이 상태에서 Barman 서버를 선택하여 연동하면 데이터베이스별 설정은 OwlDB가 자동으로 수행합니다.

## 참고: 연동 시 OwlDB가 자동 수행하는 작업

아래 항목은 OwlDB가 연동 시점에 자동으로 생성하거나 설정하므로 직접 작성하지 않습니다.

| 대상 | 위치 | 내용 |
| --- | --- | --- |
| Barman 설정 파일 | Barman 서버 `/etc/barman.d/<데이터베이스 ID>.conf` | 데이터베이스 접속 정보, WAL 수집 방식, 백업 방식, `backup_directory` |
| Agent 설정 파일 | Barman 서버 `/var/lib/barman/agent/<데이터베이스 ID>.config.yml` | 클러스터 이름, Patroni 접속 정보, listen 포트 |
| Agent 서비스 기동 | Barman 서버 `barman-agent@<데이터베이스 ID>` | `sudo systemctl restart` 로 기동 |
| SSH 접속 설정 | 데이터베이스 서버 `~/.ssh/config` | Barman 서버 접속 시 사용할 키 지정 (키 등록은 [7번](#7-ssh-키-등록) 참고) |
| 접속 허용 규칙 | 데이터베이스 서버 `patroni.yml` | Barman 서버 IP의 접속 허용 규칙 추가 |
| 복제 슬롯 | 데이터베이스 | WAL 수집 방식이 STREAMING 또는 BOTH인 경우 자동 생성 |
| 리더 변경 연동 | 데이터베이스 서버 `patroni.yml` | 리더 변경 시 Barman 설정을 갱신하도록 설정 |

---

## 문제 해결

### WAL이 수집되지 않고 로그에 `pg_receivewal not present in $PATH` 가 출력되는 경우

`/etc/barman.conf` 에 `path_prefix` 가 설정되지 않았습니다. PostgreSQL 클라이언트 실행 파일 경로로 설정합니다.

```ini
[barman]
path_prefix = /opt/postgresql/bin
```

cron이 매회 설정 파일을 다시 읽으므로 서비스를 재기동하지 않아도 됩니다.

### 연동은 되었으나 백업 상태가 정상으로 표시되지 않고 SSH 접속 실패가 출력되는 경우

Barman 서버와 데이터베이스 서버 간 SSH 키가 등록되지 않았습니다. OwlDB는 접속 가능 여부를 검증하지 않으므로 연동 자체는 성공합니다. [7번](#7-ssh-키-등록)을 참고하여 양방향으로 키를 등록하고 아래 명령으로 접속을 확인합니다.

```bash
ssh -o BatchMode=yes -i <개인키> <계정>@<대상 서버 IP> hostname
```

### 백업 시 Barman 버전 관련 오류가 발생하는 경우

Barman 서버의 `barman` 과 데이터베이스 서버의 `barman-cli` 버전이 다릅니다. 양쪽 모두 `3.11.1` 인지 확인합니다.

```bash
# Barman 서버
barman --version

# 데이터베이스 서버
rpm -q barman-cli
```

### 연동 시 `barman-wal-archive` 가 없다는 오류로 실패하는 경우

WAL 수집 방식을 ARCHIVER 또는 BOTH로 선택했으나 데이터베이스 서버에 `barman-cli` 가 설치되지 않았습니다. [8번](#8-데이터베이스-서버-설정)을 참고하여 `3.11.1` 버전으로 설치합니다.

### Barman Agent가 기동되지 않는 경우

systemd 템플릿 유닛의 이름 또는 설정 파일 경로가 OwlDB의 고정 값과 다를 수 있습니다. 아래 두 항목을 확인합니다.

```bash
systemctl cat barman-agent@
```

1. 유닛 이름이 `barman-agent@.service` 인지 확인합니다.
2. `ExecStart` 가 `/var/lib/barman/agent/%i.config.yml` 을 읽는지 확인합니다. `barman_home` 을 다른 경로로 설정했더라도 이 경로는 고정입니다.

### OwlDB의 Barman 서버 목록에 서버가 표시되지 않는 경우

OwlDB Agent가 OwlDB 서버에 등록되지 않은 상태입니다.

```bash
# Agent 기동 확인
systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer

# 설정 확인
cat /var/lib/barman/owlagent_dist/owlagent.env
```

1. Agent 서비스가 `active` 인지 확인합니다.
2. `AGENT_TYPE` 이 `barman` 인지, `IP` 와 `PORT` 가 OwlDB 서버 주소와 일치하는지 확인합니다.
3. 설정을 수정한 경우 아래와 같이 재등록합니다.

```bash
su - barman
cd /var/lib/barman/owlagent_dist
./owlagent_stop.sh
./owlagent_start.sh
```
