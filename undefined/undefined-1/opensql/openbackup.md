# OpenBackup 서버 환경 준비 가이드

OwlDB는 OpenSQL 데이터베이스의 백업 및 복구를 OpenBackup인 Barman(Backup and Recovery Manager)으로 수행합니다. OpenBackup 서버는 백업 데이터와 WAL(Write-Ahead Log)을 보관하는 서버로, OwlDB와는 별도의 서버에 설치합니다.

## 시스템 요구사항

#### 1. 하드웨어 요구사항

<table><thead><tr><th width="170">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>프로세서</td><td>대상 데이터베이스 규모에 따라 산</td></tr><tr><td>메모리</td><td>대상 데이터베이스 규모에 따라 산정</td></tr><tr><td>디스크</td><td>백업 데이터와 WAL 보관 용량을 합산하여 산정 (아래 참고)</td></tr></tbody></table>

디스크 용량은 아래 항목을 합산하여 산정합니다.

<table><thead><tr><th width="184">용도</th><th>산정 기준</th></tr></thead><tbody><tr><td>백업 데이터</td><td>대상 데이터베이스 크기 × 보관할 백업 개수</td></tr><tr><td>WAL</td><td>백업 보존기간 동안 발생하는 WAL 총량</td></tr><tr><td>로그 및 바이너리</td><td>Barman 로그와 설치 바이너리</td></tr></tbody></table>

여러 데이터베이스를 하나의 OpenBackup 서버에 연동하는 경우 데이터베이스별 용량을 합산합니다.

#### 2. 소프트웨어 요구사항

<table><thead><tr><th width="227">항목</th><th>요구사항</th></tr></thead><tbody><tr><td>OS</td><td>Rocky Linux 9.5 이상</td></tr><tr><td>OpenBackup</td><td><code>3.11.1</code> (OpenSQL 배포본에 포함)</td></tr></tbody></table>

{% hint style="info" %}
**참고**

OpenBackup은 **OpenSQL 배포본으로 설치**하며, 버전은 데이터베이스 서버의 `barman-cli` 와 반드시 같아야 합니다. 자세한 내용은 [OpenBackup 설치](openbackup-1.md)를 참고합니다.
{% endhint %}

#### 3. OS 패키지

OpenBackup 서버에 아래 패키지를 설치합니다.

```bash
dnf install -y systemd sudo cronie rsync openssh-server openssh-clients \
  which procps-ng tar gzip hostname iproute \
  python3 python3-pip python3-setuptools python3-psycopg2 python3-dateutil python3-six \
  glibc-langpack-en libicu ncurses-libs lz4-libs readline zlib libgcc libstdc++
```

<table><thead><tr><th width="208">패키지</th><th>용도</th></tr></thead><tbody><tr><td><code>sudo</code></td><td>OwlDB가 <code>sudo systemctl</code> 로 Barman Agent를 제어합니다. 설치되지 않으면 <code>/etc/sudoers.d</code> 디렉토리가 없어 설치 과정에서 실패합니다</td></tr><tr><td><code>cronie</code></td><td>Barman cron 실행. 설치되지 않으면 WAL이 수집되지 않습니다</td></tr><tr><td><code>rsync</code></td><td>복구 시 데이터 파일 전송</td></tr><tr><td><code>openssh-server</code></td><td>데이터베이스 서버로부터 WAL 아카이브 수신</td></tr><tr><td><code>python3-psycopg2</code></td><td>OpenBackup의 PostgreSQL 접속</td></tr><tr><td><code>tar</code>, <code>gzip</code></td><td>배포본 압축 해제</td></tr><tr><td><code>which</code>, <code>procps-ng</code></td><td>설치 상태 확인</td></tr><tr><td><code>glibc-langpack-en</code> 이하</td><td>PostgreSQL 및 OpenBackup 실행에 필요한 런타임 라이브러리</td></tr></tbody></table>

{% hint style="info" %}
**참고**

이미 설치되어 있는 서버에서는 변경 사항 없이 넘어갑니다.
{% endhint %}

#### 4. 배포 파일

| 항목          | 설명                                                                                      | 예시                            |
| ----------- | --------------------------------------------------------------------------------------- | ----------------------------- |
| OpenSQL 배포본 | OpenBackup, PostgreSQL 클라이언트, Barman Agent가 모두 포함되어 있습니다                                | `Tmax_OpenSQL_*.tar.gz`       |
| OwlDB Agent | OwlDB 서버와 통신하는 Agent                                                                    | `owlagent_dist_latest.tar.gz` |
| SSH 키       | 양방향 접속에 사용할 키. Barman 서버에는 **공개키**(데이터베이스 서버의 접속을 허용)와 **개인키**(데이터베이스 서버로 접속)가 모두 필요합니다 | -                             |

## 네트워크 요구사항

아래 통신이 가능하도록 방화벽을 설정합니다.

| 방향                        | 포트                                | 용도                    |
| ------------------------- | --------------------------------- | --------------------- |
| OpenBackup 서버 → OwlDB 서버  | OwlDB 서버 포트                       | Agent가 OwlDB의 명령을 수신  |
| OwlDB 서버 → OpenBackup 서버  | OpenBackup Agent Port (기본 `8080`) | Barman Agent 제어       |
| OpenBackup 서버 → 데이터베이스 서버 | `5432`                            | 백업 수행 및 WAL 수신        |
| OpenBackup 서버 → 데이터베이스 서버 | `22`                              | 복구 시 데이터 파일 전송(rsync) |
| OpenBackup 서버 → 데이터베이스 서버 | `8008`                            | Patroni 클러스터 상태 조회    |
| 데이터베이스 서버 → OpenBackup 서버 | `22`                              | WAL 아카이브 전송           |

* OpenBackup Agent Port는 연동 시 OwlDB 화면에서 입력하는 값으로, `1024`\~`65535` 범위에서 지정할 수 있습니다. OwlDB 서버 포트와 기본값이 같지만 서로 다른 서버의 포트입니다.
* `22` 번 포트는 **양방향으로 모두** 열려 있어야 합니다. OpenBackup 서버에서 데이터베이스 서버로는 복구 시 데이터 파일을 전송하고, 반대 방향으로는 WAL 아카이브가 전송됩니다. 한쪽만 열려 있으면 백업 또는 복구가 실패합니다.
* 접속 설정은 **양방향 모두 직접 수행**합니다. OwlDB가 자동으로 처리하는 것은 데이터베이스 서버가 _어떤 키로_ OpenBackup 서버에 접속할지 지정하는 부분뿐이며, OpenBackup 서버가 _그 키를 허&#xC6A9;_&#xD558;도록 등록하는 것은 직접 해야 합니다.

{% hint style="info" %}
**참고**

OwlDB 콘솔 화면에서 다음과 같이 표시됩니다.

<table><thead><tr><th width="177">문서 표현</th><th>OwlDB 콘솔 화면 표기</th></tr></thead><tbody><tr><td>OpenBackup 서버</td><td>백업 서버</td></tr><tr><td>Barman Agent 포트</td><td>OpenBackup Agent Port</td></tr><tr><td>연동 상태</td><td>Health (연결됨 / 연결안됨)</td></tr><tr><td>WAL 보관방식</td><td><strong>WAL 보관방식</strong> (<code>archiver</code> / <code>streaming</code> / <code>archiver+streaming</code>)</td></tr></tbody></table>
{% endhint %}

