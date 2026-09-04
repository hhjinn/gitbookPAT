### **배포 파일 준비 및 배치**

이 페이지에서는 OpenSQL 기반 데이터베이스 설치를 위한 서버 환경을 준비합니다. 배포 파일을 내려받아 설치 디렉터리에 배치하고, 필수 패키지와 Agent를 설치한 뒤 환경 검증까지 수행합니다.

#### **1. 필요 파일 목록**

* owldb dp 바이너리 (`owldb_dp_installer_owl_x.x.x.tar.gz`)
* OpenSQL 바이너리 (`Tmax_OpenSQL_*.tar.gz`)
* 라이선스 파일 (`license.xml`)

#### **2. 설치 디렉터리 생성**

데이터베이스를 설치할 경로(이하 `설치 디렉터리`)를 생성합니다. (예시: `/home/rocky/owldb`)

```bash
mkdir -p {설치 디렉터리}/opensql
chmod 755 {설치 디렉터리}/opensql
```

`{설치 디렉터리}/opensql` 경로가 이후 절차에서 `$OPENSQL_HOME`으로 사용됩니다.

#### **3. 파일 배치**

DP 바이너리를 `$OPENSQL_HOME`에 압축 해제하고, OpenSQL 바이너리와 라이선스 파일을 배치합니다.

```bash
# DP 바이너리 압축 해제
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
# OpenSQL 바이너리 배치
mv {OpenSQL 바이너리 파일} $OPENSQL_HOME/

# 라이선스 파일 배치
mv {라이선스 파일} $OPENSQL_HOME/license.xml
```

준비 완료 후 `$OPENSQL_HOME` 구조는 다음과 같습니다.

```text
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*.tar.gz            # OpenSQL 바이너리
 ├── license.xml                      # 라이선스 파일
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

#### **4. 필수 패키지 설치**

```bash
sudo bash install_opensql_package.sh 실행
```

이 스크립트는 다음 패키지를 설치합니다.

```bash
# PostgreSQL 공식(pgdg) 저장소
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# EPEL
dnf install -y epel-release

# CodeReady Builder(CRB) 활성화
dnf config-manager --set-enabled crb

dnf install -y \
    bison \
    clang-devel \
    flex \
    gdal \
    gettext \
    jq \
    krb5-devel \
    lz4-devel \
    openssl-devel \
    proj \
    protobuf-c \
    readline-devel \
    zlib-devel \
    python3 \
    python3-pip \
    python3-setuptools \
    python3-psycopg2 \
    python3-tabulate \
    python3-requests \
    python3-pyyaml \
    python3-dateutil \
    python3-six \
    perl-libs \
    rsync \
    barman-cli-3.11.1 \
    systemd \
    sudo \
    iproute \
    procps-ng \
    which \
    tar \
    gzip \
    glibc-langpack-en \
    libicu \
    ncurses-libs \
    lz4-libs \
    readline \
    zlib \
    libgcc \
    libstdc++ \
    openssh-server \
    openssh-clients \
    hostname \
    cronie

dnf --enablerepo=pgdg-common install -y SFCGAL

pip3 install pyyaml etcd3 requests psycopg2-binary 'protobuf<4.0.0' tabulate
```

#### **5. owlagent 설치**

1. agent 바이너리를 압축 해제 합니다.

   ```bash
   tar -zxvf owlagent_dist_latest.tar.gz
   ```

   ```text
   owlagent_dist_latest.tar.gz
   └── owlagent_dist/
       ├── config.json.description
       ├── manifest
       ├── owlagent
       ├── owlagent.env
       ├── owlagent_start.sh
       └── owlagent_stop.sh
   ```
2. owlagent.env에 설정 값을 입력 합니다.

| KEY | VALUE |
|-----|-------|
| AGENT_TYPE | pg    |
| IP  | OwlDB CP의 IP |
| PORT | OwlDB CP의 port |
| USERNAME | opensql을 실행할 user의 이름 |
| OPENSQL_HOME | [2.설치 디렉터리 생성](https://outline.tibero.com/doc/642w7j207ysw67kg7j207iqkioyenouyhcdspidruyqg67cpioyepoy5ma-pPG7Wrsvwd#h-2-%EC%84%A4%EC%B9%98-%EB%94%94%EB%A0%89%ED%84%B0%EB%A6%AC-%EC%83%9D%EC%84%B1) 단계에서 입력한 $OPENSQL_HOME 사용 |

3. owlagent를 실행합니다.

   ```bash
   sh owlagent_start.sh
   ```

{% hint style="warning" %}
**주의**

`owlagent_start.sh` 스크립트는 Agent를 systemd 서비스 및 타이머로 등록하며, 이 과정에서 sudo 권한이 사용됩니다.
{% endhint %}

#### **6. 환경 검증 수행**

현재 서버에 데이터베이스 설치 준비가 되었는지 검증하는 스크립트를 수행합니다.

```bash
bash validate_infra.sh --mode DP --db-type opensql
```
