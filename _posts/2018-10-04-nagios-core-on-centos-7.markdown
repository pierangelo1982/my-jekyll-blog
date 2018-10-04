---
layout: post
title:  "nagios core on centos 7"
date:   2018-10-04 19:48:38 +0200
categories: nagios
---

Update your machine:
```
yum update
```

install wget
```
yum install wget
```

install prerequisities packages:
```
yum install -y gcc glibc glibc-common wget unzip httpd php gd gd-devel perl postfix
```

go in /tmp directory
```
cd /tmp
```

download latest releas of nagios core:
```
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
```

extract nagios:
```
tar xzf nagios-4.4.2.tar.gz
```

go in nagios folder

cd /tmp/nagioscore-nagios-4.4.1/

and compile:
```
./configure
```

```
make all
```

create a user and a group for nagios:

```
make install-groups-users
```

and add apache to nagios group:
```
usermod -a -G nagios apache
```

install binaries, CGIs and HTML files.
```
make install
```

Install Service / Daemon
```
make install-daemoninit
```
```
systemctl enable httpd.service

```

Installs and configures the external command file.
```
make install-commandmode
```

Install sample Configuration Files
```
make install-config
```

Install Apache Config Files
```
make install-webconf
```

setting firewall, allow port 80 inbound traffic so you can reach the Nagios web interface.
```
firewall-cmd --zone=public --add-port=80/tcp
```

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

Create nagiosadmin User Account to be able to log into Nagios.
```
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

start apache:
```
systemctl start httpd.service
```

start nagios
```
systemctl start nagios.service
```

connect to:

http://your-ip/nagios

and login with 

username: nagiosadmin

password: yourpassword

---------------------------
Service / Daemon Commands

```
systemctl start nagios.service
systemctl stop nagios.service
systemctl restart nagios.service
systemctl status nagios.service
```