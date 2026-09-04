### 배포 파일 준비 및 배치

이 페이지에서는 OpenBackup 서버 환경을 준비하기 위한 배포 파일 배치와 owlagent 설치 절차를 설명합니다.

#### 1. owlagent 설치

1. agent 바이너리를 압축 해제 합니다.

   ```bash
   tar -zxvf owlagent_dist_latest.tar.gz
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
2. `owlagent.env`에 설정 값을 입력합니다.

| 항목 | 설명 | 입력 규칙 |
|------|------|-----------|
| `AGENT_TYPE` | barman | |
| `IP` | OwlDB CP의 IP | |
| `PORT` | OwlDB CP의 port | |
| `USERNAME` | opensql을 실행할 user의 이름 | |
| `OPENSQL_HOME` | | - [2. 파일 배치](https://outline.tibero.com/doc/db-6OChcUO1K6#h-2-%ED%8C%8C%EC%9D%BC-%EB%B0%B0%EC%B9%98)단계에서 이미 환경 변수가 설정되어 있으면 미입력<br>- 환경 변수가 없으면 위에서 사용한 `OPENSQL_HOME` 입력 |
| `BARMAN_NAME` | barman 서버의 이름 | |
| `BARMAN_SSH_USER` | SSH 접속 계정 | |
| `BARMAN_SSH_KEYPATH` | SSH 접속에 사용할 개인 키 파일 경로 | |
| `BARMAN_SSH_IP` | barman host에 SSH 접속할 IP 주소 | |
| `BARMAN_SSH_PORT` | barman host에 SSH 접속할 Port 번호 | |
| `BARMAN_CONF_DIR` | barman host의 conf 디렉토리 경로<br>e.g.)`/etc/barman.d/` | |

3. owlagent를 실행합니다.

   ```bash
   sh owlagent_start.sh
   ```

{% hint style="info" %}
**참고**

`owlagent_start`는 Agent를 systemd 서비스 및 타이머로 등록하며 이 과정에서 sudo 권한이 사용됩니다.
{% endhint %}
