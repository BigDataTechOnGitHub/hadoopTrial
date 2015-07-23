# hadoopTrial
* on centos 6

## java install##
* https://github.com/draculavlad/JavaDevEnv

## /etc/profile##
```
export JAVA_HOME=/usr/java/jdk1.7.0_55
export JRE_HOME=$JAVA_HOME/jre
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
```

## preparement##
* shutdown selinux:
```shell
setenforce 0
```
or
```shell
setenforce Permissive
```

* shutdown iptable
```shell
service iptables stop
```

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
* set hostname = yourhost

* start
```shell
ambari-server start
```
