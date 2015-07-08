# hadoopTrial
* on centos 7

## java install##
* https://github.com/draculavlad/JavaDevEnv

## /etc/profile##
```
export JAVA_HOME=/usr/java/jdk1.7.0_55
export JRE_HOME=$JAVA_HOME/jre
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
```

## master node##
```shell
groupadd hadoop
useradd -g hadoop hdfs
su hdfs
ssh-keygen -t rsa
cd /home/hdfs/.ssh/ && cat id_rsa.pub > ./authorized_keys
exit
```
* go to http://www.apache.org/dyn/closer.cgi/hadoop/common/ find a downloadable link
```
cd /opt && wget http://mirror.olnevhost.net/pub/apache/hadoop/common/stable2/hadoop-2.7.1.tar.gz

```

