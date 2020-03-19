---
description: The RPM packages are suitable for installation on Red Hat and CentOS.
---

# Installation with RPM packages

{% hint style="info" %}
The commands described below need to be executed with root user privileges.
{% endhint %}

## Preparation

### Add Storware repository

Create new repository file: `touch /etc/yum.repos.d/storware.repo`

Copy and paste this into a storware.repo file:

```text
[storware]
name=Storware repository
baseurl=https://repo.storware.eu/yum/
enabled=1
gpgcheck=0
```

### Add MariaDB repository

Create new repository file: `touch /etc/yum.repos.d/MariaDB.repo`

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

## Installation

### api-core component

1. Install api-core component

```text
# yum install api-core
```

### web-admin-ui component

1. Install web-admin-ui component

```text
# yum install web-admin-ui
```

### kodo-agent component

{% hint style="info" %}
Install kodo-agent component if you want to protect Office365 or Box service
{% endhint %}

1. Install kodo-agent component

```text
# yum install kodo-agent
```

