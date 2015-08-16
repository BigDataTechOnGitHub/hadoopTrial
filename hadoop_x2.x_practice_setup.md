# hadoopTrial for 2.x
* CentOS 6.6 x64
* JDK：1.7.0_55 64位
* Hadoop：2.x

## install java dev environment##
(for example)[https://github.com/draculavlad/JavaDevEnv]

## set your domain name##
* expample
```shell
sed -i 's|HOSTNAME=localhost.localdomain|hadoop|g' /etc/sysconfig/network
sed -i 's|HOSTNAME=localhost.localdomain|hadoop|g' /etc/hosts
hostname hadoop
```
* append to /etc/hosts
```shell
$YOURT_IP hadoop
```

## shutdown firewal & security##
```shell
sudo chkconfig iptables off
sudo service iptables stop
setenforce 0
sed -i 's|SELINUX=enforcing|SELINUX=disabled|g' /etc/selinux/config 
```

## install & config hadoop 2.x
```shell
tar zxf hadoop-2.2.0.tar.gz
rm -rf hadoop-2.2.0.tar.gz
cd /opt/hadoop-2.2.0
mkdir tmp
mkdir hdfs
mkdir hdfs/name
mkdir hdfs/data
chmod 755 hdfs/data
```
* append stuff into /opt/hadoop-2.2.0/etc/hadoop/hadoop-env.sh
```properties
export HADOOP_CONF_DIR=${PATH_TO_YOUR_HADOOP_2.x_DIR}/etc/hadoop
export JAVA_HOME=${{PAHT_TO_YOUR_JAVA_DIR}}
export PATH=$PATH:${PATH_TO_YOUR_HADOOP_2.x_DIR}/bin
```
* make the config work
```shell
source /opt/hadoop-2.2.0/etc/hadoop/hadoop-env.sh
```
* test with shell cmd
```shell
hadoop version
```
* set jdk path in /opt/hadoop-2.2.0/etc/hadoop/yarn-env.sh and make the config work
```
soure /opt/hadoop-2.2.0/etc/hadoop/yarn-env.sh
```
* set /opt/hadoop-2.2.0/etc/hadoop/core-site.xml
```xml
<configuration>
<property>
<name>fs.default.name</name>
<value>hdfs://hadoop:9000</value>
</property>
<property>
<name>fs.defaultFS</name>
<value>hdfs://hadoop:9000</value>
</property>
<property>
<name>io.file.buffer.size</name>
<value>131072</value>
</property>
<property>
<name>hadoop.tmp.dir</name>
<value>file:/app/hadoop-2.2.0/tmp</value>
<description>Abase for other temporary directories.</description>
</property>
<property>
<name>hadoop.proxyuser.hduser.hosts</name>
<value>*</value>
</property>
<property>
<name>hadoop.proxyuser.hduser.groups</name>
<value>*</value>
</property>
</configuration>
```
* set /opt/hadoop-2.2.0/etc/hadoop/hdfs-site.xml
```xml
<configuration>
<property>
<name>dfs.namenode.secondary.http-address</name>
<value>hadoop:9001</value>
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>file:/app/hadoop-2.2.0/hdfs/name</value>
</property>
<property>
<name>dfs.datanode.data.dir</name>
<value>file:/app/hadoop-2.2.0/hdfs/data</value>
</property>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
<property>
<name>dfs.webhdfs.enabled</name>
<value>true</value>
</property>
</configuration>
```
* set /opt/hadoop-2.2.0/etc/hadoop/mapred-site.xml
* sample: /opt/hadoop-2.2.0/etc/hadoop/mapred-site.xml.template
```xml
<configuration>
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
<property>
<name>mapreduce.jobhistory.address</name>
<value>hadoop:10020</value>
</property>
<property>
<name>mapreduce.jobhistory.webapp.address</name>
<value>hadoop:19888</value>
</property>
</configuration>
```
* set /opt/hadoop-2.2.0/etc/hadoop/yarn-site.xml
```xml
<configuration>
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
<property>
<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
<value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
<name>yarn.resourcemanager.address</name>
<value>hadoop:8032</value>
</property>
<property>
<name>yarn.resourcemanager.scheduler.address</name>
<value>hadoop:8030</value>
</property>
<property>
<name>yarn.resourcemanager.resource-tracker.address</name>
<value>hadoop:8031</value>
</property>
<property>
<name>yarn.resourcemanager.admin.address</name>
<value>hadoop:8033</value>
</property>
<property>
<name>yarn.resourcemanager.webapp.address</name>
<value>hadoop:8088</value>
</property>
</configuration>
```
* set /opt/hadoop-2.2.0/etc/hadoop/slaves to hadoop

## config system environment##
* append stuff to /etc/profile
```properties
export JAVA_HOME=${PAHT_TO_YOUR_JAVA_DIR}
export HADOOP_HOME=${PATH_TO_YOUR_HADOOP_DIR}
export MAVEN_HOME=${PATH_TO_YOUR_MAVEN_DIR}
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```
```shell
source /etc/profile
```

## format name node##
```shell
cd /opt/hadoop-2.2.0/bin && ./hdfs namenode -format
```

## start hdfs##
```shell
cd /opt/hadoop-2.2.0/sbin && ./start-dfs.sh
```

## check hadoop servie with jps##
```shell
jps
```
* shall return namenode,secondarynamenode,datanode

## start yarn##
```shell
cd /opt/hadoop-2.2.0/sbin && ./start-yarn.sh
```

## check hadoop servie with jps##
```shell
jps
```
* shall return namenode,secondarynamenode,datanode,resourcemanager,nodemanager

## test hadoop with word count##
```shell
cd /opt/hadoop-2.2.0/bin
./hadoop fs -mkdir -p /class3/input
./hadoop fs -copyFromLocal ../etc/hadoop/* /class3/input
cd /opt/hadoop-2.2.0/bin
./hadoop jar ../share/hadoop/mapreduce/hadoop-mapreduce-examples-2.2.0.jar wordcount /class3/input /class3/output
./hadoop fs -ls /class3/output/
./hadoop fs -cat /class3/output/part-r-00000 | less
```
