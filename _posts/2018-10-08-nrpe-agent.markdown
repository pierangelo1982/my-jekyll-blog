---
layout: post
title: "Nagios NRPE agent"
date: 2018-10-05 11:58:38 +0200
categories: kubernetes
---

apt install nagios-nrpe-server
yum install nagios-nrpe



vim /etc/nagios/nrpe.cfg


Per verificare che Nagios riesca a comunicare con il daemon NRPE nella macchina remota, da Nagios lanciamo il comando:

# /usr/lib/nagios/plugins/check_nrpe -H IP_MacchinaRemota





# nagios server
firewall-cmd --zone=public --add-port=5666/tcp
firewall-cmd --zone=public --add-port=5666/tcp --permanent



wget https://github.com/NagiosEnterprises/nrpe/releases/download/nrpe-3.2.1/nrpe-3.2.1.tar.gz

tar -xzvf nrpe-3.2.1.tar.gz

cd nrpe-3.2.1

./configure
make all



CREA UN FILE IN
CHiAmALO COME VUOI
 vim /usr/local/nagios/etc/objects/

 e poi collegalo qui:
 vim /usr/local/nagios/etc/nagios.cfg

 configure
 cfg_file=/usr/local/nagios/etc/objects/hosts.cfg
