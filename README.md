# hadoopTrial
* on centos 6 & vmware env

## preparement##
* clone vm
(setup vmware network)[https://github.com/draculavlad/VmwareVMNetCfgISSUEAfterClone]

* shutdown selinux:
```shell
setenforce 0
```
or
```shell
setenforce Permissive
```
* recommend modify /etc/selinux/config set selinux from enforce to permissive

* shutdown iptable
```shell
service iptables stop
chkconfig iptables off
reboot
```

## setup network##
* (setup dns server and hostname)[https://github.com/draculavlad/DNS_IN_LAN]


##  install & start ambari##
* import packages
```shell
cd /etc/yum.repos.d/ && wget http://public-repo-1.hortonworks.com/ambari/centos6/1.x/updates/1.7.0/ambari.repo
```
* install
```shell
yum install ambari-server
ambari-server setup
```
* littile fix 
* References: http://zorro.blog.51cto.com/2139862/1409468
* vi /usr/lib/python2.6/site-packages/ambari_server/setupAgent.py
* set hostname = $yourhost
* set server_port=8440

* start
```shell
ambari-server start
```
