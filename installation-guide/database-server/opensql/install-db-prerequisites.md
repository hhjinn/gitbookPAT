On this page, you prepare the server environment for OpenSQL-based database installation. Download the distribution files and place them in the installation directory, install the required packages and owlagent, and then perform environment verification.

# **1. List of required files** <a href="#required-files" id="required-files"></a>

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)
- OpenSQL binary (`Tmax_OpenSQL_*.tar.gz`)
- license file (`license.xml`)

# **2. Creating the installation directory** <a href="#create-install-directory" id="create-install-directory"></a>

Create the path where the database will be installed (hereafter `installation directory`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/opensql
chmod 755 {installation directory}/opensql
```

`{installation directory}/opensql` This path is used in subsequent procedures `$OPENSQL_HOME`is used as.

# **3. File Placement** <a href="#place-files" id="place-files"></a>

The DP binary `$OPENSQL_HOME`Extract into, and place the OpenSQL binary and license file.

```bash
# Decompress DP binary
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
# Place the OpenSQL binary
mv {OpenSQL binary file} $OPENSQL_HOME/

# Place license file
mv {license file} $OPENSQL_HOME/license.xml
```

After preparation is complete `$OPENSQL_HOME` the structure is as follows.

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*.tar.gz            # OpenSQL binary
 ├── license.xml                      # license file
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# **4. Install required packages** <a href="#install-required-packages" id="install-required-packages"></a>

`owldb_dp_installer` Move to the directory and run the script. The subsequent procedures 5 and 6 are also carried out in the same directory.

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

# geos-devel: AppStream only has the geos runtime and does not have the devel subpackage. pgdg-common uses a version
# distributed under a name with a suffix (geos313-devel), and opensql-installer
# checks with the geos*-devel pattern, so this name also satisfies the required package requirement. Match 3.13.1, the same as AppStream geos.
dnf --enablerepo=pgdg-common install -y geos313-devel

pip3 install pyyaml etcd3 requests psycopg2-binary 'protobuf<4.0.0' tabulate

#BINARY
set +x
```

# **5. Install owlagent** <a href="#install-owlagent" id="install-owlagent"></a>

1. Extract the owlagent binary.
 tar -zxvf owlagent_dist_latest.tar.gz -C $OPENSQL_HOME owlagent_dist_latest.tar.gz └── owlagent_dist/ ├── config.json.description ├── manifest ├── owlagent ├── owlagent.env ├── owlagent_start.sh └── owlagent_stop.sh
2. Enter the configuration values in owlagent.env.

| KEY | VALUE |
| --- | --- |
| AGENT_TYPE | pg |
| IP | IP of OwlDB CP |
| PORT | port of OwlDB CP |
| USERNAME | Name of the user that will run opensql |
| OPENSQL_HOME | [2. Create installation directory](#create-install-directory) Use $OPENSQL_HOME entered in the step |

3. Run owlagent. sh owlagent_start.sh

{% hint style="warning" %}
**Caution**

`owlagent_start.sh` The script registers owlagent as a systemd service and timer, and sudo privileges are used during this process.
{% endhint %}

# **6. Perform Environment Verification** <a href="#verify-environment" id="verify-environment"></a>

Runs a script that verifies whether the current server is ready for database installation.

```bash
bash validate_infra.sh --mode DP --db-type opensql
```
