---
description: This chapter describes how to install Kodo Server.
---

# Installation

## Downloading installation packages

To get access to KODO Server installation packages please contact with [Storware Professional Services Team](mailto:ps@storware.eu) or one of our local [partners](https://storware.eu/en/partners/).

## Additional packages

Some additional packages are required to be installed:

* Java 
* MariaDB 
* IBM Spectrum Protect Client 
* lsof
* libunwind
* icu

### Java

At least version 8 is required

You can install OpenJDK Java from yum repositories:  
`yum -y install java`

{% hint style="info" %}
To provide the best security mechanisms Kodo Server **MUST** have Java Cryptography Extension \(JCE\) Unlimited Strength Jurisdiction Policy installed. **OpenJDK Java has this policy installed by default.**
{% endhint %}

### MariaDB

At least version 10.2 is required.

{% hint style="info" %}
You can also use MySQL &gt;= 5.7. MariaDB is preferred.
{% endhint %}

{% hint style="info" %}
CentOS has outdated MariaDB packages. To install newest version add MariaDB repository using official tool: [https://downloads.mariadb.org/mariadb/repositories/](https://downloads.mariadb.org/mariadb/repositories/)
{% endhint %}

#### Installing MariaDB 10.3 on CentOS 7

Create new repository file:  
`touch /etc/yum.repos.d/MariaDB.repo`

Copy and paste this into a MariaDB.repo file:

```text
# MariaDB 10.3 CentOS repository list
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.3/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

After that you can use yum to install MariaDB:  
`yum -y install MariaDB-server MariaDB-client`

### IBM Spectrum Protect Backup-Archive Client

At least version 7.1.6 is required. 

Latest 7.1.8 version of IBM SP BA Client can be found on IBM FTP server:

[ftp://ftp.software.ibm.com/storage/tivoli-storage-management/patches/client/v7r1/Linux/LinuxX86/BA/v718/](ftp://ftp.software.ibm.com/storage/tivoli-storage-management/patches/client/v7r1/Linux/LinuxX86/BA/v718/)

#### Installing Spectrum Protect Backup-Archive Client 7.1.8 on CentOS

Download package with client:  
`wget ftp://ftp.software.ibm.com/storage/tivoli-storage-management/patches/client/v7r1/Linux/LinuxX86/BA/v718/7.1.8.3-TIV-TSMBAC-LinuxX86.tar`

Extract a tarball file:  
`tar -xvf 7.1.8.3-TIV-TSMBAC-LinuxX86.tar`

Install required packages:

* gskcrypt64
* gskssl64
* TIVsm-API64
* TIVsm-BA

`yum -y install gskssl64-*.x86_64.rpm gskcrypt64-*.x86_64.rpm TIVsm-API64.x86_64.rpm TIVsm-BA.x86_64.rpm`

### lsof

You can install lsof from yum repo:  
`yum -y install lsof`

### libunwind

{% hint style="info" %}
libunwind is required by web-admin-ui component
{% endhint %}

You can install libunwind from yum repo:  
`yum -y install libunwind`

### icu

{% hint style="info" %}
icu library is required by web-admin-ui component
{% endhint %}

You can install libunwind from yum repo:  
`yum -y install icu`

## Pre-installation configuration

### Creating user ID and instance directories 

Create the user ID for the KODO server instance and create the directories that the KODO instance needs for installation.

1.  Create the user ID that will own the server instance `useradd -m kodo`
2. Configure password for new user `passwd kodo`
3. Create directory for KODO server installation `mkdir /opt/storware /opt/storware/kodo-server /opt/StorwareData`
4. Change owner of the directories for newly created user `chown -R kodo:kodo /opt/storware /opt/StorwareData`

### Configuring database

#### Changing server configuration

You **MUST** set following parameters in `[mysqld]` section of MySQL/MariaDB configuration \(e.g. `/etc/my.cnf`\):

```text
lower_case_table_names=1
binlog_format = ROW
transaction_isolation = READ-COMMITTED
```

Example `my.cnf` file:

```text
#
# This group is read both both by the client and the server
# use it for options that affect everything
#
[client-server]

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
lower_case_table_names = 1
binlog_format = ROW
transaction_isolation = READ-COMMITTED
```

#### Enable database service at boot

Start the service and enable service at boot:  
`systemctl enable mariadb  
systemctl start mariadb`

#### Initializing and preparing database for KODO

Initialize database:  
`mysql_secure_installation`

This program is a shell script available on Unix systems, and enables you to improve the security of your MariaDB installation in the following ways:

* You can set a password for root accounts.
* You can remove root accounts that are accessible from outside the local host.
* You can remove anonymous-user accounts.
* You can remove the test database, which by default can be accessed by anonymous users.

{% hint style="info" %}
Default root user is using blank password
{% endhint %}

#### Create user and database 

Log in to database instance as a root user:  
`mysql -u root -p`

Create user for KODO:  
`CREATE USER 'kodo'@'localhost' IDENTIFIED BY 'mySecretPassword';`

_Where 'mySecretPassword' is password of your choice._

Create database for KODO:  
`CREATE DATABASE IF NOT EXISTS storware DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_bin;`

{% hint style="warning" %}
You **MUST** prepare `storware` database which uses `utf8mb4` character set and `utf8mb4_bin` default collation.
{% endhint %}

Grant all privileges to newly created storware database for kodo user:  
`GRANT ALL ON storware.* TO 'kodo'@'localhost';`

## Installing api-core component

{% hint style="info" %}
All commands from here should be executed as 'kodo' system user
{% endhint %}

By default all Kodo Server files **SHOULD** be placed in `/opt/storware/kodo-server` directory. We will call it **KODO Server root directory**.

Place KODO api-core installation package to KODO Server root directory and extract it:  
`tar -xvf api-core-<version>.tar.gz`

Change permissions to `/opt/storware/kodo-server/generated/tsm` so only owner can access it:  
`sudo chmod 700 /opt/storware/kodo-server/generated/tsm`

### Preparing KODO Server configuration file

Prepare configuration file. Copy sample file and modify it according to your need:  
`cp /opt/storware/kodo-server/api-core/config/payara.properties.smp /opt/storware/kodo-server/api-core/config/payara.properties`

{% hint style="warning" %}
Changing configuration options other than described in this document is not supported
{% endhint %}

#### Configuring listening ports

Default KODO Server will listen on port 8080 for HTTP connections and 8181 for HTTPS connection. We strongly recommend using HTTPS. To change listening ports modify below values in configuration file:

**eu.storware.kodo.api.http.port=**  
Port number for HTTP sessions. Default: 8080

**eu.storware.kodo.api.https.port=**  
Port number for HTTPS sessions. Default: 8181

```text
eu.storware.kodo.api.http.port=8080
eu.storware.kodo.api.https.port=8181
```

{% hint style="info" %}
If you start Kodo Server by non-root OS user it will be not possible to use ports lower than 1024 \(e.g. 443\). If you want to set lower ports for Kodo Server, use OS level port forwarding.
{% endhint %}

#### Configuring database connection

To provide credentials for database, configured in previous steps, modify following values in configuration file:

**eu.storware.kodo.db.port=**  
Port number for database. Default: 3306

**eu.storware.kodo.db.host=**  
Host where database is installed. Default: localhost

**eu.storware.kodo.db.user=**  
User for database connection**.** Default: -

**eu.storware.kodo.db.password=**  
Password for database connection user. Default: -

```text
eu.storware.kodo.db.port=3306
eu.storware.kodo.db.host=localhost
eu.storware.kodo.db.user=kodo
eu.storware.kodo.db.password=mySecretPassword
```

#### SSL certificate configuration for HTTPS connections

For KODO Server you need to prepare PKCS12 certificate file. Provide certification name to use, path to certificate file and certificate password.

**eu.storware.kodo.ssl.certname=**  
Certificate name in p12 file. Default: s1as

**javax.net.ssl.keyStore=**  
Path to p12 certificate name. Default: -

**javax.net.ssl.keyStorePassword=**  
Password for p12 certificate file**.** Default: -

{% hint style="info" %}
If you don't provide any certificate default one self-signed certificate will be used.
{% endhint %}

### Preparing Spectrum Protect client configuration

Prepare Spectrum Protect client configuration file. Copy sample files:  
`cp /opt/storware/kodo-server/api-core/config/dsm.opt.smp /opt/storware/kodo-server/api-core/config/dsm.opt`

`cp /opt/storware/kodo-server/api-core/config/dsm.sys.smp /opt/storware/kodo-server/api-core/config/dsm.sys`

Create symbolic links for dsm.opt and dsm.sys files:

`sudo ln -s /opt/storware/kodo-server/api-core/config/dsm.opt /opt/tivoli/tsm/client/ba/bin`

`sudo ln -s /opt/storware/kodo-server/api-core/config/dsm.sys /opt/tivoli/tsm/client/ba/bin`

`sudo ln -s /opt/storware/kodo-server/api-core/config/dsm.sys /opt/tivoli/tsm/client/api/bin64`

`sudo ln -s /opt/storware/kodo-server/api-core/config/dsm.opt /opt/tivoli/tsm/client/api/bin64`

`sudo ln -s /opt/storware/kodo-server/api-core/config/dsm.sys /opt/StorwareData`

`sudo ln -s /opt/storware/kodo-server/api-core/config/dsm.opt /opt/StorwareData`

{% hint style="info" %}
Connection with Spectrum Protect server will be configured via administrative portal
{% endhint %}

### Starting server instance

To start KODO Server instance execute following command:  
`/opt/storware/kodo-server/api-core/bin/start.sh`

### Stoping server instance

To stop KODO Server instance execute following command:  
`/opt/storware/kodo-server/api-core/bin/stop.sh`

### Checking server status

To check KODO Server instance status execute following command:  
`/opt/storware/kodo-server/api-core/bin/status.sh`

### Automatically starting server

Copy systemd service sample file:`sudo cp /opt/storware/kodo-server/api-core/config/kodo-server-api-core.service.smp /etc/systemd/system/kodo-server-api-core.service`

Enable kodo-server-api-core service:  
`systemctl enable kodo-server-api-core.service`

{% hint style="info" %}
Default KODO server will be started from 'kodo' user. To change this modify systemd service file
{% endhint %}

## Installing web-admin-ui component

{% hint style="info" %}
All commands from here should be executed as 'kodo' system user
{% endhint %}

By default all Kodo Server Web Admin UI files **SHOULD** be placed in **KODO Server root directory \(prepared in previous steps\)**.

Place KODO web-admin-ui installation package to KODO Server root directory and extract it:  
`tar -xvf web-admin-ui-<version>.tar.gz`

### Listening port

Web admin UI is listening on port 5000.

### Preparing SSL certificate for web admin UI

Administrative portal component require SSL certificate to start properly. 

Here are some requirements for certificate:

* Certificate must be in PKCS12 format
* Certificate must be named _ssl.p12_
* Certificate file must be stored in /opt/storware/kodo-server/web-admin-ui path
* Password for certificate file must be _changeit_

#### Generating self-signed certificate for web admin UI

Use this command go generate self-signed certificate. Answer the questions prompted.:  
`openssl req -x509 -newkey rsa:4096 -keyout privkey.pem -out certificate.pem -days 1095 -nodes`

Export certificate to PKCS12 format. Provide password _changeit_ when asked:  
`openssl pkcs12 -export -in certificate.pem -inkey privkey.pem -out ssl.p12`

### Configuring connection with api-core 

You must configure address \(IP or FQDN\) and port of api-core component to which web admin UI will be connecting.

{% hint style="warning" %}
The address \(IP or FQDN\) that you provide in configuration is used with javascript that is executed on local web browser \(on administrator workstation\). So web browser \(administrator host\) need to be able to connect it.
{% endhint %}

Edit `/opt/storware/kodo-server/web-admin-ui/conf/env.json` file and modify ApiServer line, providing address of api-core component in `https://api-core-addres/api` format.

#### Example of env.json file:

```javascript
{ 
"ApiServer" : "https://192.168.0.100:8181/api", 
    "Logging": { 
        "LogLevel": { 
            "Default": "Warning" 
        } 
    } 
}
```

_In this example administrator workstation from where he will be log in to KODO administrative portal must be able to connect to 192.168.0.100 address_ 

### Starting web admin UI instance

To start KODO web admin UI instance execute following command:  
`/opt/storware/kodo-server/web-admin-ui/bin/start.sh`

### Stoping web admin UI instance

To stop KODO web admin UI  instance execute following command:  
`/opt/storware/kodo-server/web-admin-ui/bin/stop.sh`

### Checking web admin UI status

To check KODO web admin UI  instance status execute following command:  
`/opt/storware/kodo-server/web-admin-ui/bin/status.sh`

### Automatically starting server

Copy systemd service sample file:`sudo cp /opt/storware/kodo-server/web-admin-ui/config/kodo-server-web-admin-ui.service.smp /etc/systemd/system/kodo-server-web-admin-ui.service`

Enable kodo-server-api-core service:  
`systemctl enable kodo-server-web-admin-ui.service`

## Access your KODO system

After successful installation you should be able login your KODO system using web browser and IP address of KODO Server. Open webbrowser and enter:

```text
https://ip_address:5000
```

{% hint style="info" %}
Remember to open correct ports on firewall. In default configuration you will need to have access to ports 8181 and 5000
{% endhint %}

## 

