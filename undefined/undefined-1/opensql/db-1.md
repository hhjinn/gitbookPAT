등록할 DB Service의 배포 파일을 배치하고 owlagent를 설치·기동하기까지의 환경 준비 절차를 설명합니다.

# **1. 필요 파일 목록**

* owldb dp 바이너리 (`owldb_dp_installer_owl_x.x.x.tar.gz`)

# **2. 파일 배치**

DP 바이너리를 `$OPENSQL_HOME`에 압축 해제합니다.

```bash
# DP 바이너리 압축 해제
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
```

준비 완료 후 `$OPENSQL_HOME`구조는 다음과 같습니다

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*                   # OpenSQL 바이너리
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# 3. owlagent 설치

1. agent 바이너리를 압축 해제합니다.

```bash
tar -zxvf owlagent_dist_latest.tar.gz -C $OPENSQL_HOME
```

```bash
owlagent_dist_latest.tar.gz
└── owlagent_dist/
    ├── config.json.description
    ├── manifest
    ├── owlagent
    ├── owlagent.env
    ├── owlagent_start
    └── owlagent_stop
```

2. owlagent.env에 설정 값을 입력 합니다.

<table data-full-width="true"><thead><tr><th>항목</th><th>설명</th><th>입력 규칙</th></tr></thead><tbody><tr><td>AGENT_TYPE*</td><td></td><td><code>pg</code> 입력</td></tr><tr><td>IP*</td><td>OwlDB CP의 IP</td><td></td></tr><tr><td>PORT*</td><td>OwlDB CP의 port</td><td></td></tr><tr><td>USERNAME*</td><td>opensql 실행 user 이름</td><td></td></tr><tr><td>OPENSQL_HOME</td><td></td><td><ul><li>이미 설정 시 입력 불필요</li><li>미설정 시 위에서 사용한 OPENSQL_HOME 입력</li></ul></td></tr><tr><td>DB_LOG_DIR</td><td>PG 로그 경로</td><td>로그 미수집 시 입력 불필요</td></tr><tr><td>DB_LOG_FILE_GLOB</td><td>PG 로그 파일 형식</td><td>예: <code>postgresql*.log</code></td></tr><tr><td>PATRONI_CONFIG</td><td>patroni.yml 경로</td><td></td></tr><tr><td>PATRONI_MEMBER</td><td>patroni 멤버 이름</td><td></td></tr></tbody></table>

\*표기는 필수 입력 항목을 의미합니다.

{% hint style="info" %}
**참고**

DB Service 등록 시점에 `DB_LOG_DIR` 경로에 로그 파일이 존재하지 않아도 무방합니다. 등록 이후 로그 파일이 생성되면 [Syslog](../../../undefined-5/undefined-2/syslog.md) 메뉴에서 조회합니다.
{% endhint %}

3. owlagent 실행

```bash
sh owlagent_start.sh
```

`owlagent_start` 스크립트는 Agent를 systemd 서비스 및 타이머로 등록하며, 이 과정에서 sudo 권한이 사용됩니다.

# 4. patroni 서비스 명 점검

Patroni를 systemd로 기동한 환경에서는 `systemctl start|stop|status patroni` 형태로 Patroni를 제어합니다. 유닛명이 `patroni.service`가 아니면 기동·정지 제어와 상태 수집이 동작하지 않으므로 사전에 유닛명을 확인해야 합니다.

a. systemd 기동 여부 확인

```bash
systemctl is-active --quiet patroni; echo $?
```

<table data-full-width="true"><thead><tr><th>결과</th><th>판정</th></tr></thead><tbody><tr><td>0</td><td><code>patroni.service</code>로 등록·기동됨 → 점검 완료</td></tr><tr><td>0 이외</td><td>아래 두 경우가 구분되지 않아 b 추가 수행 필요<ul><li>다른 유닛명으로 기동된 경우</li><li>systemd 미등록인 경우</li></ul></td></tr></tbody></table>

b. 유닛명 확인

```bash
# 1) Patroni 프로세스 PID 확인
pgrep -af patroni

# 2) 해당 PID를 관리하는 유닛 확인 (PID는 1)의 결과값으로 대체)
cat /proc/<PID>/cgroup
```

| 결과  | 판정  |
|-----|-----|
| 0::/system.slice/xxxxxxxx.service | 유닛명 불일치 → c 추가 수행 필요 |
| 0::/user.slice/user-1000.slice/session-3.scope | systemd 미등록 → 추가 조치 필요 없음<br>owldb 내부에서 중지/시작 처리 |

c. 서비스명 변경 방법

기존 유닛 파일의 `[Install]` 섹션에 Alias를 추가한 뒤 재등록합니다.

```bash
[Install]
WantedBy=multi-user.target
Alias=patroni.service
```

d. 재등록(daemon-reload·reenable)

Alias 추가 후 daemon-reload와 reenable로 서비스를 재등록합니다.

```bash
systemctl daemon-reload
systemctl reenable <기존서비스명>
systemctl is-active --quiet patroni; echo $?   # 0 확인
```
