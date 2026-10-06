# OpenBackup 서버 설치 가이드

[OpenBackup 서버 환경 준비](openbackup-prerequisites.md)의 시스템 및 네트워크 요구사항을 충족한 상태에서 진행합니다.\
아래 절차를 완료하면 OwlDB에서 OpenBackup 서버를 연동할 수 있는 상태가 됩니다.&#x20;

{% hint style="info" %}
**참고**

데이터베이스별 설정 파일과 Agent 프로세스는 OwlDB가 연동 시점에 자동으로 생성하므로 직접 작성하지 않습니다.
{% endhint %}

## OpenBackup 설치

### 1. OpenBackup 설치

OpenBackup은 `3.1.1` 버전으로 설치합니다. 데이터베이스 서버에 설치하는 `barman-cli` 도 동일하게 `3.11.1` 이어야 합니다.&#x20;

| 설치 위치         | 패키지          | 제공 명령                                      | 역할         |
| ------------- | ------------ | ------------------------------------------ | ---------- |
| OpenBackup 서버 | `barman`     | `barman put-wal`, `barman get-wal`         | WAL을 받아 보관 |
| 데이터베이스 서      | `barman-cli` | `barman-wal-archive`, `barman-wal-restore` | WAL을 전송    |

{% hint style="warning" %}
**주의**

`pip install barman` 과 같이 버전을 지정하지 않고 설치하면 최신 버전이 설치되어 `3.11.1` 과 어긋납니다.

이 경우 WAL을 보내는 쪽과 받는 쪽의 버전이 일치하지 않아 연동 및 백업이 실패합니다.
{% endhint %}

#### 1-1. 배포본 압축 해제

OpenSQL 배포본을 원하는 경로에 압축해제합니다.

```bash
mkdir -p /opt/opensql-src
tar xzf <OpenSQL 배포본>.tar.gz -C /opt/opensql-src --strip-components=1
```

#### 1-2. 설치 경로 환경 변수 설정

설치 스크립트가 아래 환경 변수를 요구하므로 **설치 전에 먼저 설정**합니다.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

| 항목            | 설명                                               | 예시                     | 필수 여부 |
| ------------- | ------------------------------------------------ | ---------------------- | ----- |
| OPENSQL\_HOME | OpenSQL 설치 경로                                    | `/opt/postgresql`      | 필수    |
| PG\_HOME      | PostgreSQL 설치 경로. 실행 파일은 `<PG_HOME>/bin` 에 배치됩니다 | `/opt/postgresql`      | 필수    |
| PG\_DATA\_DIR | PostgreSQL 데이터 경로                                | `/opt/postgresql/data` | 필수    |

설정하지 않으면 다음 단계에서 아래 오류가 출력되며, 설치가 진행되지 않습니다.

```bash
Error: Environment variable 'OPENSQL_HOME' is not set.
```

`PG_HOME` 에 지정한 값은 3번 단계의 `path_prefix`(`<PG_HOME>/bin`)와 같아야 합니다.

#### 1-3. 설치 진행

```bash
cd /opt/opensql-src/scripts
. ./setenv.sh "$(pwd)"
./install.sh postgresql
./install.sh barman
```

OpenBackup 서버에서는 PostgreSQL 서비스를 기동하지 않습니다. `pg_receivewal`, `pg_basebackup` 등 클라이언트 유틸리티만 사용합니다.

#### 1-4. 버전 확인

`3.11.1` 이 출력되어야 합니다.

```bash
$ barman --version
3.11.1 Barman by EnterpriseDB (www.enterprisedb.com)
```

### 2. OpenBackup 실행 계정 설정

#### 2-1. 계정 생성

OpenBackup을  실행할 전용 OS 계정을 생성하고 NOPASSWD sudo 권한을 부여합니다.

```bash
groupadd -g 1001 barman
useradd -u 1001 -g 1001 -m -d /var/lib/barman -s /bin/bash barman

echo "barman ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/barman
chmod 440 /etc/sudoers.d/barman
```

| 항목      | 설명                             | 예시                               | 필수 여부 |
| ------- | ------------------------------ | -------------------------------- | ----- |
| 계정      | OpenBackup 실행 전용 OS 계정         | `barman`                         | 필수    |
| 홈 디렉토리  | 계정 홈 디렉토리. 백업 데이터가 이 하위에 저장됩니다 | `/var/lib/barman`                | 필수    |
| sudo 권한 | NOPASSWD sudo                  | `barman ALL=(ALL) NOPASSWD: ALL` | 필수    |

OwlDB는 Barman Agent를 `sudo systemctl restart barman-agent@<데이터베이스 ID>` 로 제어합니다. 이 명령이 비밀번호 입력 없이 수행되어야 하므로 NOPASSWD sudo 권한이 필요합니다.

OpenBackup 서버는 이 계정 하나로 동작합니다. 백업 수행, OwlDB Agent 실행, 데이터베이스 서버 접속이 모두 같은 계정으로 이루어집니다.

#### 2-2. PATH 설정

`barman` 계정에서 `pg_receivewal`, `pg_basebackup` 등 PostgreSQL 클라이언트 명령을 경로 없이 실행할 수 있도록 PATH를 추가합니다. 1-2의 `PG_HOME` 값에 맞춰 작성합니다.

```bash
su - barman
echo 'export PATH=$PATH:/opt/postgresql/bin' >> ~/.bashrc
source ~/.bashrc
exit
```

이 설정은 운영자가 직접 명령을 실행할 때 사용됩니다. Barman cron은 이 PATH를 사용하지 않으므로 3번 단계의 `path_prefix` 를 별도로 설정해야 합니다.

### 3. OpenBackup 전역 설정

`/etc/barman.conf` 파일을 아래와 같이 작성합니다.

```bash
[barman]
path_prefix = /opt/postgresql/bin
barman_user = barman
configuration_files_directory = /etc/barman.d
barman_home = /var/lib/barman
log_file = /var/log/barman/barman.log
log_level = INFOb
```

| 항목                              | 설명                                                                | 예시                           | 필수 여부 |
| ------------------------------- | ----------------------------------------------------------------- | ---------------------------- | ----- |
| path\_prefix                    | PostgreSQL 클라이언트 실행 파일 경로                                         | `/opt/postgresql/bin`        | 필수    |
| barman\_user                    | Barman 실행 계정                                                      | `barman`                     | 필수    |
| configuration\_files\_directory | 데이터베이스별 설정 파일 디렉토리. OwlDB Agent의 `BARMAN_CONF_DIR` 과 같은 값으로 설정합니다 | `/etc/barman.d`              | 필수    |
| barman\_home                    | 백업 데이터 저장 경로                                                      | `/var/lib/barman`            | 필수    |
| log\_file                       | 로그 파일 경로                                                          | `/var/log/barman/barman.log` | 필수    |
| log\_level                      | 로그 레벨                                                             | `INFO`                       | 필수    |

`path_prefix` 는 반드시 설정해주십시오. Barman cron은 최소한의 PATH 환경에서 실행되므로 계정의 `.bashrc` 등에 설정한 PATH가 적용되지 않습니다. `path_prefix` 가 없으면 cron이 `pg_receivewal` 을 찾지 못해 WAL 수집이 영구적으로 실패합니다.

설정에 사용한 디렉토리를 생성하고 소유권을 지정합니다.

```bash
mkdir -p /var/log/barman /etc/barman.d /var/lib/barman/agent

chown -R barman:barman /var/lib/barman /var/log/barman /etc/barman.d
chmod 700 /var/lib/barman
```

<table><thead><tr><th width="254">디렉토리</th><th>용도</th></tr></thead><tbody><tr><td><code>/etc/barman.d</code></td><td>데이터베이스별 OpenBackup 설정 파일. 연동 시 OwlDB가 생성</td></tr><tr><td><code>/var/lib/barman/agent</code></td><td>데이터베이스별 Agent 설정 파일. 연동 시 OwlDB가 생성</td></tr><tr><td><code>/var/log/barman</code></td><td>Barman 로그</td></tr></tbody></table>

백업 데이터는 `<barman_home>/<데이터베이스 ID>` 에 저장됩니다. 단 4번 단계의 Agent 설정 파일 경로는 `barman_home` 과 무관하게 고정입니다.

### 4. Barman Agent 등록

Barman Agent는 Patroni 클러스터의 리더 변경을 감지하여 Barman 설정을 갱신하는 프로세스입니다. OpenSQL 배포본의 `barman_agent/server.py` 를 사용합니다.

#### 4-1. 실행 파일 배치

```bash
cp /opt/opensql-src/barman_agent/server.py /usr/local/bin/barman-agent
chmod 755 /usr/local/bin/barman-agent

pip3 install --no-cache-dir -r /opt/opensql-src/barman_agent/requirements.txt
```

#### 4-2. systemd 템플릿 유닛 등록

하나의 OpenBackup 서버가 여러 데이터베이스를 담당할 수 있으므로, 데이터베이스별로 Agent가 기동되도록 systemd **템플릿 유닛**으로 등록합니다. 인스턴스 이름(`%i`)에는 OwlDB의 데이터베이스 ID가 전달됩니다.

`/usr/lib/systemd/system/barman-agent@.service` 파일을 아래와 같이 작성합니다.

```bash
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

유닛 파일을 새로 작성했으므로 systemd에 반영합니다.

```bash
systemctl daemon-reload
```

아래 두 값은 OwlDB에 고정되어 있으므로 그대로 사용해주십시오.

| 항목             | 값                                              | 변경 가능 여부 |
| -------------- | ---------------------------------------------- | -------- |
| 유닛 이름          | `barman-agent@.service`                        | 불가       |
| Agent 설정 파일 경로 | `/var/lib/barman/agent/<데이터베이스 ID>.config.yml` | 불가       |

`barman_home` 을 `/var/lib/barman` 이외의 경로로 설정한 경우에도 Agent 설정 파일 경로는 `/var/lib/barman/agent/` 로 고정입니다. `ExecStart` 경로를 `barman_home` 기준으로 작성하면 OwlDB가 생성한 설정 파일을 읽지 못해 Agent가 기동되지 않습니다.

{% hint style="info" %}
**참고**

설정 파일의 내용은 OwlDB가 연동 시점에 생성합니다. 이 단계에서는 유닛 파일만 작성하고 기동하지 않습니다.
{% endhint %}

### 5. Barman cron 등록

WAL 수집과 아카이빙이 1분 주기로 수행되도록 cron을 등록합니다.

`/etc/cron.d/barman-cron` 파일을 아래와 같이 작성합니다.

```bash
* * * * * barman /usr/local/bin/barman cron > /var/log/barman/cron.log 2>&1
```

```bash
chmod 644 /etc/cron.d/barman-cron
systemctl enable --now sshd crond
```

`sshd` 를 함께 기동합니다. 데이터베이스 서버가 WAL 아카이브를 전송할 때 OpenBackup 서버의  `sshd` 로 접속합니다. 이미 기동 중인 서버에서는 그대로 유지됩니다.

### 6. OwlDB Agent 설치

OwlDB Agent는 OwlDB 서버의 명령을 받아 OpenBackup 서버에서 작업을 수행합니다. OpenBackup 서버의 모든 연동 작업이 이 Agent를 통해 이루어지므로 반드시 설치해주십시오.

#### 6-1. 배포 파일 압축 해제

```bash
tar xzf owlagent_dist_latest.tar.gz -C /var/lib/barman
chown -R barman:barman /var/lib/barman/owlagent_dist
```

#### 6-2. owlagent.env 설정

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

| 항목                   | 설명                                                                                   | 예시                            | 필수 여부 |
| -------------------- | ------------------------------------------------------------------------------------ | ----------------------------- | ----- |
| AGENT\_TYPE          | Agent 종류. Barman 서버는 `barman` 을 입력합니다                                                | `barman`                      | 필수    |
| IP                   | OwlDB 서버 IP                                                                          | `192.168.0.10`                | 필수    |
| PORT                 | OwlDB 서버 포트                                                                          | `8080`                        | 필수    |
| USERNAME             | Agent 실행 계정                                                                          | `barman`                      | 필수    |
| OPENSQL\_HOME        | PostgreSQL 및 OpenBackup 설치 경로                                                        | `/opt/postgresql`             | 필수    |
| BARMAN\_NAME         | OwlDB에 표시되는 OpenBackup 서버 이름                                                         | `barman01`                    | 필수    |
| BARMAN\_SSH\_USER    | OpenBackup 서버 SSH 접속 계정                                                              | `barman`                      | 필수    |
| BARMAN\_SSH\_KEYPATH | OpenBackup 서버가 데이터베이스 서버에 접속할 때 사용하는 개인키 경로. 이 단계에서는 경로만 지정하고, 실제 키 파일은 7-2 에서 배치합니다 | `/var/lib/barman/.ssh/id_rsa` | 필수    |
| BARMAN\_SSH\_IP      | OpenBackup 서버 IP                                                                     | `192.168.0.20`                | 필수    |
| BARMAN\_SSH\_PORT    | OpenBackup 서버 SSH 포트                                                                 | `22`                          | 필수    |
| BARMAN\_CONF\_DIR    | 데이터베이스별 설정 파일 디렉토리. `barman.conf` 의 `configuration_files_directory` 와 같은 값을 입력합니다    | `/etc/barman.d`               | 필수    |

{% hint style="warning" %}
**주의**

`AGENT_TYPE` 이 `barman` 이 아니면 OwlDB가 OpenBackup 서버로 인식하지 않아 **백업 서버** 목록에 표시되지 않습니다.
{% endhint %}

### 7. SSH 설정

OpenBackup 서버와 데이터베이스 서버는 **양방향**으로 SSH 접속이 필요합니다. 방향별로 준비할 내용이 다릅니다.

| 방향                        | 용도                                 | 준비                                 |
| ------------------------- | ---------------------------------- | ---------------------------------- |
| 데이터베이스 서버 → OpenBackup 서버 | WAL 아카이브 전송 (`barman-wal-archive`) | OpenBackup 서버가 해당 키로의 접속을 허용 (7-1) |
| OpenBackup 서버 → 데이터베이스 서버 | 복구 시 데이터 파일 전송, `rsync` 백업 방식      | OpenBackup 서버에 인증 정보 배치 (7-2)      |

데이터베이스 서버가 **어떤 키로** OpenBackup 서버에 접속할지는 OwlDB가 연동 시 자동으로 설정합니다. 데이터베이스 서버의 `~/.ssh/config` 에 노드 등록 시 입력한 SSH 키를 사용하도록 기록합니다. 다만 **OpenBackup** **서버가 그 키를 허용하도록 하는 것은 자동이 아니므로** 7-1에서 직접 등록해야 합니다.

#### 7-1. 데이터베이스 서버에서 OpenBackup 서버로 접속 허용

데이터베이스 노드 등록 시 사용한 키의 공개키를 OpenBackup 실행 계정의 `authorized_keys` 에 등록합니다.

```bash
install -d -m 700 -o barman -g barman /var/lib/barman/.ssh

cat <데이터베이스 노드 등록 키의 공개키> >> /var/lib/barman/.ssh/authorized_keys
chmod 600 /var/lib/barman/.ssh/authorized_keys
chown barman:barman /var/lib/barman/.ssh/authorized_keys
```

이 등록이 없으면 데이터베이스 서버에서 WAL을 전송할 때 `Permission denied (publickey)` 로 실패합니다. 그러면 연동 시 `barman check` 의 **WAL archive**와 **continuous archiving** 항목이 실패하여 연동이 롤백되고 **Health**가 **연결안됨** 으로 표시됩니다.

#### 7-2. OpenBackup 서버에서 데이터베이스 서버로 접속 설정

OpenBackup 실행 계정에서 데이터베이스 서버의 OpenSQL 계정으로 **비밀번호 입력 없이** 접속되도록 설정합니다.

개인키 파일을 OpenBackup 실행 계정이 읽을 수 있는 위치에 배치합니다.

```bash
cp <개인키 파일> /var/lib/barman/.ssh/id_rsa
chmod 600 /var/lib/barman/.ssh/id_rsa
chown barman:barman /var/lib/barman/.ssh/id_rsa
```

**파일 이름과 경로는 자유롭게 정할 수 있습니다.** 배치한 경로를 6-2의 `BARMAN_SSH_KEYPATH` 에 정확히 입력하면 됩니다.

```bash
# 다른 이름으로 사용하는 예
cp <키 파일> /var/lib/barman/.ssh/<키 이름>
chmod 600 /var/lib/barman/.ssh/<키 이름>
chown barman:barman /var/lib/barman/.ssh/<키 이름>

# owlagent.env 에는 같은 경로를 입력
# BARMAN_SSH_KEYPATH=/var/lib/barman/.ssh/<키 이름>
```

키 파일의 **형식과 확장자는 제한이 없습니다.** OpenSSH 형식(`-----BEGIN OPENSSH PRIVATE KEY-----`)이든 PEM 형식이든, 이름이 `id_rsa` 든 `.pem` 이든 사용할 수 있습니다.

데이터베이스 서버에는 해당 키로 접속이 허용되어 있어야 합니다.

단 **개인키 파일 자체는 반드시 있어야 합니다.** OwlDB는 연동 시 Barman 설정 파일에 `ssh -i <BARMAN_SSH_KEYPATH>` 형태의 접속 명령을 생성하며, 이 값은 **절대 경로**여야 합니다. 따라서 키 파일을 쓰지 않는 방식(ssh-agent, 비밀번호 인증 등)은 사용할 수 없습니다.

#### 7-3. 호스트 키 등록

OpenBackup 실행 계정으로 데이터베이스 서버에 처음 접속하여 호스트 키를 등록합니다. 데이터베이스 서버가 여러 대인 경우 **모든 노드에 대해** 수행합니다.

```bash
su - barman
ssh -o StrictHostKeyChecking=accept-new <OpenSQL 계정>@<데이터베이스 서버 IP> hostname
```

데이터베이스 서버의 hostname이 출력되면 정상입니다.

이 등록은 7-4의 확인 명령과 운영자가 직접 접속할 때 필요합니다. OpenBackup이 백업·복구 작업에서 접속할 때는 OwlDB가 생성한 설정 파일의 접속 명령에 호스트 키 검사를 생략하는 옵션이 포함되어 있어 영향을 받지 않습니다.

#### 7-4. 접속 확인

OpenBackup 서버에서 데이터베이스 서버로 비밀번호나 확인 프롬프트 없이 접속되어야 합니다. 데이터베이스 서버가 여러 대인 경우 모든 노드에 대해 수행합니다.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL 계정>@<데이터베이스 서버 IP> hostname
```

| 출력                              | 의미                                |
| ------------------------------- | --------------------------------- |
| 데이터베이스 서버의 hostname             | 정상                                |
| `Host key verification failed`  | 7-3의 호스트 키 등록이 되지 않았습니다           |
| `Permission denied (publickey)` | 데이터베이스 서버가 7-2에서 배치한 키를 허용하지 않습니다 |

반대 방향(데이터베이스 서버 → OpenBackup 서버)은 **이 방향은 연동 시 OwlDB가 자동 처리하므로 사전 확인이 필요하지 않습니다.** OwlDB가 연동 시점에 데이터베이스 서버의 `~/.ssh/config` 에 접속 설정을 작성하며, 이 설정에는 처음 접속하는 OpenBackup 서버의 호스트 키를 자동으로 수락하는 옵션이 포함되어 있습니다. 따라서 연동 전에 이 방향으로 직접 접속을 시도하면 호스트 키가 등록되어 있지 않아 `Host key verification failed` 가 출력되며, 이는 설치가 잘못된 것이 아닙니다.

이 방향에서 직접 준비해야 하는 것은 7-1의 `authorized_keys` 등록뿐입니다. 이 등록이 누락되면 연동 시점에 `barman check` 의 **WAL archive** 와 **continuous archiving** 항목 실패로 드러납니다.

***



## OpenBackup 서버 연동 확인

OpenBackup 서버 설치를 완료한 상태에서 진행합니다.

### 1. 데이터베이스 서버 설정

OpenBackup 서버 외에 데이터베이스 서버에도 아래 항목이 준비되어 있어야 합니다.

| 항목           | 설명                                    | 필수 여부                                                      |
| ------------ | ------------------------------------- | ---------------------------------------------------------- |
| `barman-cli` | OpenBackup 서버와 같은 `3.11.1` 버전으로 설치합니다 | WAL 보관방식을 `archiver` 또는 `archiver+streaming` 으로 사용하는 경우 필수 |
| `rsync`      | 복구 시 데이터 파일 전송에 사용합니다                 | 복구 사용 시 필수                                                 |
| 데이터베이스 계정    | OpenSQL 설치 시 입력한 계정이 슈퍼유저로 존재해야 합니다   | 필수                                                         |

OpenBackup은 OpenSQL 설치 시 입력한 계정으로 데이터베이스에 접속합니다. 이 계정이 슈퍼유저이면 복제 및 백업 권한을 이미 보유하므로 별도의 전용 계정을 만들지 않아도 됩니다.

#### 1-1. barman-cli 설치 확인

```bash
rpm -q barman-cli
barman-wal-archive --version
```

설치되어 있으면 아래와 같이 출력됩니다.

```bash
$ barman-wal-archive --version
barman-wal-archive 3.11.1
```

설치되지 않은 경우 아래와 같이 출력됩니다.

```bash
$ rpm -q barman-cli
package barman-cli is not installed

$ barman-wal-archive --version
bash: barman-wal-archive: command not found
```

`barman-cli` 는 OpenSQL과 별개의 패키지이므로 OpenSQL이 설치된 서버에도 없을 수 있습니다. 연동 전에 반드시 확인해주십시오.

WAL 보관방식을 `archiver` 또는 `archiver+streaming` 으로 연동하는 경우, 데이터베이스 서버에 `barman-wal-archive` 가 없으면 OwlDB가 연동을 거부합니다. WAL 보관방식이 `streaming` 인 경우에는 `barman-cli` 가 필요하지 않습니다.

#### 1-2. barman-cli 설치

`barman-cli` 는 PostgreSQL 공식 저장소(PGDG)에서 제공합니다.

인터넷 연결이 가능한 환경에서는 저장소를 등록한 뒤 버전을 지정하여 설치합니다.

```bash
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

dnf install -y rsync
dnf --enablerepo=pgdg-common install -y barman-cli-3.11.1
```

인터넷 연결이 불가한 환경에서는 동일한 버전의 rpm 파일을 미리 확보하여 설치합니다.

```bash
dnf install -y ./barman-cli-3.11.1-*.rpm
```

{% hint style="warning" %}
**주의**

버전을 지정하지 않고 `dnf install barman-cli` 또는 `pip install barman-cli` 로 설치하면 최신 버전이 설치되어 Barman 서버와 버전이 어긋납니다. 반드시 `barman-cli-3.11.1` 처럼 버전을 지정해주십시오.
{% endhint %}

### 2. OpenBackup 서버 설치 결과 확인

OpenBackup 서버에서 아래 명령으로 점검합니다.

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

# barman 계정의 NOPASSWD sudo
$ su - barman -c 'sudo -n true' && echo OK
OK
```

<table data-search="true"><thead><tr><th>확인 항목</th><th>정상 결과</th></tr></thead><tbody><tr><td><code>barman --version</code></td><td><code>3.11.1</code></td></tr><tr><td><code>/etc/barman.conf</code></td><td><code>path_prefix</code>, <code>configuration_files_directory</code> 설정됨</td></tr><tr><td><code>barman-agent@</code> 유닛</td><td>유닛 존재, <code>ExecStart</code> 가 <code>/var/lib/barman/agent/%i.config.yml</code> 참조</td></tr><tr><td><code>/etc/cron.d/barman-cron</code></td><td>1분 주기 <code>barman cron</code> 등록</td></tr><tr><td><code>owldb-barman-agent.service</code> / <code>.timer</code></td><td>모두 <code>active</code></td></tr><tr><td><code>sshd</code>, <code>crond</code></td><td>모두 <code>active</code></td></tr><tr><td>NOPASSWD sudo</td><td><code>OK</code> 출력</td></tr></tbody></table>

{% hint style="warning" %}
**주의**

NOPASSWD sudo 확인을 빠뜨리지 마십시오. 이 설정이 없으면 설치는 끝난 것처럼 보이지만, 연동할 때 OwlDB가 Barman Agent를 기동하는 단계에서 실패합니다.
{% endhint %}

### 3. OwlDB 연동

OwlDB 웹 UI에서 아래 순서로 연동합니다.

1. 데이터베이스의 **OpenBackup 설정** 화면으로 이동합니다.
2. **백업 서버** 목록에 설치한 Barman 서버가 표시되는지 확인합니다. 표시되면 연동 가능한 상태입니다.
3. **백업 서버**, **OpenBackup Agent Port**, **WAL 보관방식**, **백업 방식** 을 선택하여 연동합니다.
4. 연동 후 **Health** 가 **연결됨** 으로 표시되는지 확인합니다.

**Health** 가 **연결됨** 이면 설치와 연동이 완료된 상태입니다.

***

#### 참고: 연동 시 OwlDB가 자동 수행하는 작업

아래 항목은 OwlDB가 연동 시점에 자동으로 생성하거나 설정하므로 직접 작성하지 않습니다.

| 대상           | 위치                                                       | 내용                                                       |
| ------------ | -------------------------------------------------------- | -------------------------------------------------------- |
| Barman 설정 파일 | Barman 서버 `/etc/barman.d/<데이터베이스 ID>.conf`               | 데이터베이스 접속 정보, WAL 보관방식, 백업 방식, `backup_directory`        |
| Agent 설정 파일  | Barman 서버 `/var/lib/barman/agent/<데이터베이스 ID>.config.yml` | 클러스터 이름, Patroni 접속 정보, listen 포트                        |
| Agent 서비스 기동 | Barman 서버 `barman-agent@<데이터베이스 ID>`                     | `sudo systemctl restart` 로 기동                            |
| SSH 접속 설정    | 데이터베이스 서버 `~/.ssh/config`                                | Barman 서버 접속 시 노드 등록 때 입력한 SSH 키를 사용하도록 지정               |
| 접속 허용 규칙     | 데이터베이스 서버 `patroni.yml`                                  | Barman 서버 IP의 접속 허용 규칙 추가                                |
| 복제 슬롯        | 데이터베이스                                                   | WAL 보관방식이 `streaming` 또는 `archiver+streaming` 인 경우 자동 생성 |
| 리더 변경 연동     | 데이터베이스 서버 `patroni.yml`                                  | 리더 변경 시 Barman 설정을 갱신하도록 설정                              |

{% hint style="info" %}
참고

OpenBackup 서버 설치 및 운영에 문제가 있는 경우, [참고 자료 > OpenBackup 조치 가이드](../../../references/openbackup-troubleshooting.md)를 참고 바랍니다.
{% endhint %}
