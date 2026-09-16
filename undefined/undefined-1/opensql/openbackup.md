이 페이지에서는 OpenBackup 서버 환경을 준비하기 위한 배포 파일 배치와 owlagent 설치 절차를 설명합니다.

## 1. owlagent 설치

1. agent 바이너리를 압축 해제 합니다. tar -zxvf owlagent_dist_latest.tar.gz owlagent_dist_latest.tar.gz └── owlagent_dist/ ├── config.json.description ├── manifest ├── owlagent ├── owlagent.env ├── owlagent_start └── owlagent_stop
2. `owlagent.env`에 설정 값을 입력합니다.

<table data-full-width="true"><thead><tr><th>항목</th><th>설명</th><th>입력 규칙</th></tr></thead><tbody><tr><td><code>AGENT_TYPE</code></td><td>barman</td><td></td></tr><tr><td><code>IP</code></td><td>OwlDB CP의 IP</td><td></td></tr><tr><td><code>PORT</code></td><td>OwlDB CP의 port</td><td></td></tr><tr><td><code>USERNAME</code></td><td>opensql을 실행할 user의 이름</td><td></td></tr><tr><td><code>OPENSQL_HOME</code></td><td></td><td><ul><li><a href="#h-2-파일-배치">2. 파일 배치</a>단계에서 이미 환경 변수가 설정되어 있으면 미입력</li><li>환경 변수가 없으면 위에서 사용한<code>OPENSQL_HOME</code> 입력</li></ul></td></tr><tr><td><code>BARMAN_NAME</code></td><td>barman 서버의 이름</td><td></td></tr><tr><td><code>BARMAN_SSH_USER</code></td><td>SSH 접속 계정</td><td></td></tr><tr><td><code>BARMAN_SSH_KEYPATH</code></td><td>SSH 접속에 사용할 개인 키 파일 경로</td><td></td></tr><tr><td><code>BARMAN_SSH_IP</code></td><td>barman host에 SSH 접속할 IP 주소</td><td></td></tr><tr><td><code>BARMAN_SSH_PORT</code></td><td>barman host에 SSH 접속할 Port 번호</td><td></td></tr><tr><td><code>BARMAN_CONF_DIR</code></td><td>barman host의 conf 디렉토리 경로<br>e.g.)<code>/etc/barman.d/</code></td><td></td></tr></tbody></table>

3. owlagent를 실행합니다. sh owlagent_start.sh

{% hint style="info" %}
**참고**

`owlagent_start`는 Agent를 systemd 서비스 및 타이머로 등록하며 이 과정에서 sudo 권한이 사용됩니다.
{% endhint %}
