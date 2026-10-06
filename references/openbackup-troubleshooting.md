# OpenBackup 조치 가이드

## 설치 중 `Error: Environment variable 'OPENSQL_HOME' is not set.` 가 출력되는 경우 <a href="#opensql-home-not-set" id="opensql-home-not-set"></a>

설치 스크립트가 요구하는 환경 변수가 설정되지 않았습니다. `OPENSQL_HOME`, `PG_HOME`, `PG_DATA_DIR` 세 개를 설정한 뒤 다시 실행합니다.

```bash
export OPENSQL_HOME=/opt/postgresql
export PG_HOME=/opt/postgresql
export PG_DATA_DIR=/opt/postgresql/data
```

## 백업 서버 목록에 서버가 표시되지 않는 경우 <a href="#server-missing-from-backup-list" id="server-missing-from-backup-list"></a>

OwlDB Agent가 OwlDB 서버에 등록되지 않은 상태입니다.

```bash
systemctl is-active owldb-barman-agent.service owldb-barman-agent.timer
cat /var/lib/barman/owlagent_dist/owlagent.env
```

`AGENT_TYPE` 이 `barman` 인지, `IP` 와 `PORT` 가 OwlDB 서버 주소와 일치하는지 확인합니다. 수정한 경우 `owlagent_stop.sh` 로 중지한 뒤 `owlagent_start.sh` 를 다시 실행합니다.

## 연동 시 Barman Agent 기동 단계에서 실패하는 경우 <a href="#barman-agent-fails-during-integration" id="barman-agent-fails-during-integration"></a>

`barman` 계정의 NOPASSWD sudo 설정이 없습니다.

```bash
su - barman -c 'sudo -n true' && echo OK
```

`OK` 가 출력되지 않으면 `sudo` 패키지 설치 여부(`rpm -q sudo`)와 `/etc/sudoers.d/barman` 파일 존재를 확인합니다.

## 연동 시 `barman-wal-archive` 가 없다는 오류로 실패하는 경우 <a href="#barman-wal-archive-missing" id="barman-wal-archive-missing"></a>

WAL 보관방식을 `archiver` 또는 `archiver+streaming` 으로 선택했으나 데이터베이스 서버에 `barman-cli` 가 설치되지 않았습니다. 1-2를 참고하여 설치합니다.

## Barman Agent가 기동되지 않는 경우 <a href="#barman-agent-not-starting" id="barman-agent-not-starting"></a>

systemd 템플릿 유닛의 이름 또는 설정 파일 경로가 OwlDB의 고정 값과 다를 수 있습니다.

```bash
systemctl cat barman-agent@
```

유닛 이름이 `barman-agent@.service` 인지, `ExecStart` 가 `/var/lib/barman/agent/%i.config.yml` 을 읽는지 확인합니다. `barman_home` 을 다른 경로로 설정했더라도 이 경로는 고정입니다.

유닛 파일 작성 후 `systemctl daemon-reload` 를 실행하지 않았을 수 있습니다. `systemctl cat` 은 파일을 직접 읽으므로 이 경우에도 정상 출력됩니다.

```bash
systemctl daemon-reload
```

## Health가 연결안됨으로 표시되는 경우 <a href="#health-not-connected" id="health-not-connected"></a>

`barman check` 항목 중 **WAL archive** 와 **continuous archiving** 이 실패하면 Health가 연결안됨으로 표시됩니다. 데이터베이스 서버에서 Barman 서버로 WAL이 전송되지 않는 상태입니다.

OpenBackup 서버에서 실패 항목을 확인합니다.

```bash
su - barman
barman check <데이터베이스 ID>
```

`WAL archive: FAILED` 또는 `continuous archiving: FAILED` 가 출력되면, 데이터베이스 서버에서 OpenBackup 서버로 접속되는지 확인합니다. **데이터베이스 서버의 OpenSQL 계정으로** 실행합니다.

```bash
ssh -o BatchMode=yes -i <노드 등록 키> barman@<Barman 서버 IP> hostname
```

<table><thead><tr><th width="290">출력</th><th>원인과 조치</th></tr></thead><tbody><tr><td><code>Permission denied (publickey)</code></td><td>OpenBackup 서버가 이 키를 허용하지 않습니다. <strong>OpenBackup</strong> <strong>서버 설치</strong> 문서 7-1의 <code>authorized_keys</code> 등록을 확인합니다</td></tr><tr><td><code>REMOTE HOST IDENTIFICATION HAS CHANGED</code></td><td>데이터베이스 서버가 이전 OpenBackup 서버의 호스트 키를 기억하고 있습니다. 아래 항목을 참고합니다</td></tr><tr><td>OpenBackup 서버 hostname</td><td>이 방향은 정상입니다. 아래 반대 방향도 확인합니다</td></tr></tbody></table>

반대 방향(OpenBackup 서버 → 데이터베이스 서버)도 확인합니다. 이 방향은 `barman check` 의 `ssh` 항목에 해당합니다.

```bash
su - barman
ssh -o BatchMode=yes <OpenSQL 계정>@<데이터베이스 서버 IP> hostname
```

접속되지 않으면 개인키 파일 경로가 `owlagent.env` 의 `BARMAN_SSH_KEYPATH` 와 같은지, 데이터베이스 서버가 해당 키를 허용하는지 확인합니다.

## OpenBackup 서버를 재구축한 뒤 Health가 연결안됨으로 표시되는 경우 <a href="#health-not-connected-after-rebuild" id="health-not-connected-after-rebuild"></a>

데이터베이스 서버가 이전 OpenBackup 서버의 호스트 키를 기억하고 있어 접속을 거부합니다. OpenBackup 서버를 같은 IP로 다시 설치한 경우에 발생합니다.

데이터베이스 서버에서 접속을 시도하면 아래와 같이 출력됩니다.

```bash
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
Host key verification failed.
```

**모든 데이터베이스 노드에서** 해당 항목을 제거한 뒤 다시 연동합니다.

```bash
ssh-keygen -R <Barman 서버 IP>
```

OwlDB가 생성하는 접속 설정은 처음 접속하는 서버의 호스트 키만 자동으로 받아들이므로, 호스트 키가 **변경된** 경우는 위와 같이 직접 정리해야 합니다.

## WAL이 수집되지 않고 로그에 `pg_receivewal not present in $PATH` 가 출력되는 경우 <a href="#pg-receivewal-not-in-path" id="pg-receivewal-not-in-path"></a>

`/etc/barman.conf` 에 `path_prefix` 가 설정되지 않았습니다. PostgreSQL 클라이언트 실행 파일 경로로 설정합니다. cron이 매회 설정 파일을 다시 읽으므로 서비스를 재기동하지 않아도 됩니다.

```bash
[barman]
path_prefix = /opt/postgresql/bin
```

## 백업 시 Barman 버전 관련 오류가 발생하는 경우 <a href="#barman-version-error" id="barman-version-error"></a>

OpenBackup 서버의 `barman` 과 데이터베이스 서버의 `barman-cli` 버전이 다릅니다. 양쪽 모두 `3.11.1` 인지 확인합니다.

```bash
# Barman 서버
barman --version

# 데이터베이스 서버
rpm -q barman-cli
```
