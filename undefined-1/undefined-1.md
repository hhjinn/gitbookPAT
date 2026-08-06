# **1. 필요 파일 목록**

- owldb dp 바이너리 (`owldb-dp-installer-*.tar.gz`)
- OpenSQL 바이너리 (`Tmax_OpenSQL_*.tar.gz`)
- 라이선스 파일 (`license.xml`)

# **2. 설치 디렉터리 생성**

데이터베이스를 설치할 경로(이하 `설치 디렉터리`)를 생성합니다. (예시: `/home/rocky/owldb`)

```javascript
mkdir -p {설치 디렉터리}/opensql chmod 755 {설치 디렉터리}/opensql
```

`{설치 디렉터리}/opensql`경로가 이후 절차에서 `$OPENSQL_HOME`으로 사용됩니다.

# **3. 파일 배치**

DP 바이너리를 `$OPENSQL_HOME`에 압축 해제하고, OpenSQL 바이너리와 라이선스 파일을 배치합니다.

```javascript
# DP 바이너리 압축 해제
tar -zxvf owldb-dp-installer-%Y%m%d-%H.tar.gz -C $OPENSQL_HOME --strip-components=2

# OpenSQL 바이너리 배치
mv {OpenSQL 바이너리 파일} $OPENSQL_HOME/

# 라이선스 파일 배치
mv {라이선스 파일} $OPENSQL_HOME/license.xml
```

준비 완료 후 `$OPENSQL_HOME`구조는 다음과 같습니다

```javascript
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*.tar.gz            # OpenSQL 바이너리
 ├── license.xml                      # 라이선스 파일
 └── owldb-dp-installer/
      ├──
      └──
```

# 4. 필수 패키지 설치

# 필수 Repository

설치 전에 다음 Repository가 등록/활성화되어야 한다

| Repository | 용도 |
| --- | --- |
| PostgreSQL PGDG | PostgreSQL 관련 패키지(`barman-cli`, `SFCGAL` 등) |
| EPEL | Extra Packages for Enterprise Linux |
| CRB (CodeReady Builder) | 개발(devel) 라이브러리 의존성 제공 |
| pgdg-common | `SFCGAL` 패키지 설치 |

# install_opensql_package.sh

```javascript
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
