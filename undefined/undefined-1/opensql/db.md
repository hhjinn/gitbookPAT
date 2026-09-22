This page prepares the server environment for an OpenSQL-based database installation. You download the distribution files and place them in the installation directory, install the required packages and owlagent, and then perform environment validation.

# **1. List of Required Files**

- owldb dp binary (`owldb_dp_installer_owl_x.x.x.tar.gz`)
- OpenSQL binary (`Tmax_OpenSQL_*.tar.gz`)
- License file (`license.xml`)

# **2. Create the Installation Directory**

Create the path where the database will be installed (hereinafter `설치 디렉터리`). (Example: `/home/rocky/owldb`)

```bash
mkdir -p {installation directory}/opensql
chmod 755 {installation directory}/opensql
```

`{설치 디렉터리}/opensql` The path is used as `$OPENSQL_HOME`in subsequent procedures.

# **3. Place the Files**

Extract the DP binary into `$OPENSQL_HOME`, and place the OpenSQL binary and the license file.

```bash
# Extract the DP binary
tar -zxvf owldb_dp_installer_owl_x.x.x.tar.gz -C $OPENSQL_HOME
# Place the OpenSQL binary
mv {OpenSQL binary file} $OPENSQL_HOME/

# Place the license file
mv {license file} $OPENSQL_HOME/license.xml
```

After preparation is complete, the `$OPENSQL_HOME` structure is as follows.

```bash
$OPENSQL_HOME/
 ├── Tmax_OpenSQL_*.tar.gz            # OpenSQL binary
 ├── license.xml                      # License file
 └── owldb_dp_installer
     ├── owlagent_dist_latest.tar.gz
     ├── install_opensql_package.sh
     └── validate_infra.sh
```

# **4. Install the Required Packages**

`owldb_dp_installer` Move into the directory and run the script. Steps 5 and 6 that follow are also performed continuously in the same directory.

```bash
cd $OPENSQL_HOME/owldb_dp_installer
sudo bash install_opensql_package.sh
```

This script installs the following packages.

```bash
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
dnf --enablerepo=pgdg-common install -y geos313-devel

pip3 install pyyaml etcd3 requests psycopg2-binary 'protobuf<4.0.0' tabulate
```

# **5. Install owlagent**

1. Extract the owlagent binary. tar -zxvf owlagent_dist_latest.tar.gz -C $OPENSQL_HOME owlagent_dist_latest.tar.gz └── owlagent_dist/ ├── config.json.description ├── manifest ├── owlagent ├── owlagent.env ├── owlagent_start.sh └── owlagent_stop.sh
2. Enter the configuration values in owlagent.env.

| KEY | VALUE |
| --- | --- |
| AGENT_TYPE | pg |
| IP | IP of OwlDB CP |
| PORT | port of OwlDB CP |
| USERNAME | Name of the user that will run opensql |
| OPENSQL_HOME | [2. Create the Installation Directory](#h-2-설치-디렉터리-생성) Use the $OPENSQL_HOME entered in the step |

3. Run owlagent. sh owlagent_start.sh

{% hint style="warning" %}
**Caution**

`owlagent_start.sh` The script registers owlagent as a systemd service and timer, and sudo privileges are used during this process.
{% endhint %}

# **6. Perform Environment Validation**

Run the script that verifies whether the current server is ready for the database installation.

```bash
bash validate_infra.sh --mode DP --db-type opensql
```
