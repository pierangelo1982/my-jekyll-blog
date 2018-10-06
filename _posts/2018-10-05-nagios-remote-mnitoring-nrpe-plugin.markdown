---
layout: post
title: "Monitoring remote machines with Nagios and NRPE Plugin"
date: 2018-10-05 11:58:38 +0200
categories: nagios
---

NRPE (Nagios Remote Plugin Executor) allows you to monitor remote machine with Nagios.

We have two VPS, the first VPS with installed Nagios Core, and the second to monitoring remotely throight the first.

enter in your second vps:

go in /tmp folder
```
cd /tmp
```
download NRPE plugin

```
wget https://github.com/NagiosEnterprises/nrpe/releases/download/nrpe-3.2.1/nrpe-3.2.1.tar.gz
```

extract:
```
tar -zxvf nrpe-3.2.1.tar.gz
```

go into the extracted folder:
```
cd nrpe-3.2.1
```

compiling:
```
./configure --enable-command-args
```

if all ok you get something:
```
*** Configuration summary for nrpe 3.2.1 2017-09-01 ***:

 General Options:
 -------------------------
 NRPE port:    5666
 NRPE user:    nagios
 NRPE group:   nagios
 Nagios user:  nagios
 Nagios group: nagios

```

if is it,  launch command:
```
make all
```


```
make install-groups-users
```

```
make install
```

```
make install-config
```

Update Services File
The /etc/services file is used by applications to translate human readable service names into port numbers when connecting to a machine across a network.

centos:
```
echo >> /etc/services
echo '# Nagios services' >> /etc/services
echo 'nrpe    5666/tcp' >> /etc/services
```

ubuntu:
```
sudo sh -c "echo >> /etc/services"
sudo sh -c "sudo echo '# Nagios services' >> /etc/services"
sudo sh -c "sudo echo 'nrpe    5666/tcp' >> /etc/services"
```

daemon:
ubuntu:
```
sudo make install-init
sudo systemctl enable nrpe.service
```

centos:
```
make install-init
systemctl enable nrpe.service
```

Firewall

Ubuntu.
```
sudo mkdir -p /etc/ufw/applications.d
sudo sh -c "echo '[NRPE]' > /etc/ufw/applications.d/nagios"
sudo sh -c "echo 'title=Nagios Remote Plugin Executor' >> /etc/ufw/applications.d/nagios"
sudo sh -c "echo 'description=Allows remote execution of Nagios plugins' >> /etc/ufw/applications.d/nagios"
sudo sh -c "echo 'ports=5666/tcp' >> /etc/ufw/applications.d/nagios"
sudo ufw allow NRPE
sudo ufw reload
```
centos:
```
firewall-cmd --zone=public --add-port=5666/tcp
firewall-cmd --zone=public --add-port=5666/tcp --permanent
```
...............................

edit nrcpe.cfg file:
```
vim /usr/local/nagios/etc/nrpe.cfg
```

and insert ip of nagios VPS (the first vps) as server address:

```
server_address=51.75.22.204
```

restart nrpe:
```
service nrpe restart
```


First VPS:
Configuring The Nagios Host on First VPS:




------------------------------------------
If you plan on running nrpe under inetd or xinetd and making use of TCP wrappers you must edit
```
vim /etc/services
```

and add this line:
```
nrpe            5666/tcp    # NRPE
```

and launch:
```
make install-inetd
```

restart inted:
```
/etc/rc.d/init.d/inet restart
```





---------------------------------------------

N.B: if in ubuntu get some error 
```
apt-get install build-essential

sudo apt-get install libssl-dev

```

if centos:
```
yum install -y gcc glibc glibc-common openssl openssl-devel perl wget
```