This page prepares the server environment for an OpenSQL-based database installation. It covers downloading the distribution files and placing them in the installation directory, installing the required packages and owlagent, and then performing environment validation.

# **1. List of Required Files**

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)
- OpenSQL binary (`Tmax_OpenSQL_*.tar.gz`)
- license file (`license.xml`)

# **2. Creating the Installation Directory**

Create the path where the database will be installed (hereafter `설치 디렉터리`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/opensql
chmod 755 {installation directory}/opensql
```

`{설치 디렉터리}/opensql` This path is used as `$OPENSQL_HOME`in the subsequent procedures.

# **3. File Placement**

Extract the DP binary to `$OPENSQL_HOME`extract into it, then place the OpenSQL binary and license file.

```bash
# Extract the DP binary
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
# Place the OpenSQL binary
mv {OpenSQL binary file} $OPENSQL_HOME/

# Place the license file
mv {license file} $OPENSQL_HOME/license.xml
```

Once preparation is complete, `$OPENSQL_HOME` the structure is as follows.

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*.tar.gz            # OpenSQL binary
 ├── license.xml                      # License file
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# **4. Install required packages**

`owldb_dp_installer` Move to the directory and run the script. Subsequently, perform steps 5 and 6 in the same directory as well.

```bash
cd $OPENSQL_HOME/owldb_dp_installer
sudo bash install_opensql_package.sh
```

This script installs the following packages.

```bash
#!/bin/bash

exec 5> /dev/null
BASH_XTRACEFD=5

V_USER=$(whoami)
BASE_DIR="$(cd "$(dirname "$0")" && pwd)"

set -x

# PostgreSQL official (pgdg) repository
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# EPEL
dnf install -y epel-release

# Enable CodeReady Builder (CRB)
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

# geos-devel: AppStream only has the geos runtime and no devel subpackage. pgdg-common provides the version
# with a name bearing a suffix (geos313-devel), and opensql-installer checks with the geos*-devel pattern,
# so this name also satisfies the required package requirement. Matches 3.13.1, the same as AppStream geos.
dnf --enablerepo=pgdg-common install -y geos313-devel

pip3 install pyyaml etcd3 requests psycopg2-binary 'protobuf<4.0.0' tabulate

#BINARY
set +x
```

# **5. Install owlagent**

1. Extract the owlagent binary. 
 tar -zxvf owlagent_dist_latest.tar.gz -C $OPENSQL_HOME owlagent_dist_latest.tar.gz └── owlagent_dist/ ├── config.json.description ├── manifest ├── owlagent ├── owlagent.env ├── owlagent_start.sh └── owlagent_stop.sh
2. Enter the configuration values in owlagent.env.

| KEY | VALUE |
| --- | --- |
| AGENT_TYPE | pg |
| IP | IP of the OwlDB CP |
| PORT | port of the OwlDB CP |
| USERNAME | Name of the user that will run opensql |
| OPENSQL_HOME | [2. Create installation directory](#h-2-설치-디렉터리-생성) Use $OPENSQL_HOME entered in the step |

3. Run owlagent. sh owlagent_start.sh

{% hint style="warning" %}
**Caution**

`owlagent_start.sh` The script registers owlagent as a systemd service and timer, and sudo privileges are used during this process.
{% endhint %}

# **6. Perform environment validation**

Run the script that validates whether the current server is ready for database installation.

```bash
bash validate_infra.sh --mode DP --db-type opensql
```
