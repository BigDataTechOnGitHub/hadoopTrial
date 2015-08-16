# hadoopTrial
* CentOS 6.6 x64
* JDK：1.7.0_55 64位
* Hadoop：1.1.2

## config system environment##
* append stuff to /etc/profile
```properties
export JAVA_HOME=${PAHT_TO_YOUR_JAVA_DIR}
export HADOOP_HOME=${PATH_TO_YOUR_HADOOP_DIR}
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

## config network##
* edit /etc/sysconfig/network-scripts/ifcfg-eth0
* exaample:
```properties
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.212.173
GATEWAY=192.168.212.2
BROADCAST=192.168.212.255
NETWORK=192.168.212.0
DNS1=192.168.212.174
```

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

## install jdk7##
(for example)[https://github.com/draculavlad/JavaDevEnv]

## update ssl lib##
```shell
yum update -y openssl
```

## conifg shh without password authentication##
* uncomment below in /etc/ssh/sshd_config
```properties
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```
* restart sshd
```shell
sudo service sshd restart
```
* generate ssh key
```shell
ssh-keygen -t rsa
cd ~/.ssh && cp id_rsa.pub authorized_keyssudo chmod 400 authorized_keys
```

## install hadoop##
* install
```shell
tar -xzf hadoop-1.1.2-bin.tar.gz
rm -rf /opt/hadoop-1.1.2
mv hadoop-1.1.2 /opt
cd /opt/hadoop-1.1.2
mkdir tmp
mkdir hdfs
mkdir hdfs/name
mkdir hdfs/data
chmod 755 hdfs/data
```
* config
* append to /opt/hadoop-1.1.2/conf/hadoop-env.sh
```properties
export JAVA_HOME=${PAHT_TO_YOUR_JAVA_DIR}
export HADOOP_HOME=${PATH_TO_YOUR_HADOOP_DIR}
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin
export HADOOP_CLASSPATH=$HADOOP_CLASSPATH:$HADOOP_HOME/myclass
```
* set /opt/hadoop-1.1.2/conf/core-site.xml
```xml
<configuration>
  <property>
 <name>fs.default.name</name>
  <value>hdfs://hadoop:9000</value>
  </property>
  <property>
 <name>hadoop.tmp.dir</name>
  <value>${PATH_TO_YOUR_HADOOP_HOME}/tmp</value>
  </property>
</configuration>
```
* set /opt/hadoop-1.1.2/conf/hdfs-site.xml
```xml
<configuration>
  <property>
 <name>dfs.replication</name>
  <value>1</value>
  </property>
  <property>
 <name>dfs.name.dir</name>
  <value>${PATH_TO_YOUR_HADOOP_HOME}/hdfs/name</value>
  </property>
  <property>
 <name>dfs.data.dir</name>
  <value>${PATH_TO_YOUR_HADOOP_HOME}/hdfs/data</value>
  </property>
</configuration>
```
* set /opt/hadoop-1.1.2/conf/mapred-site.xml
```xml
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>hadoop:9001</value>
  </property>
</configuration>
```
* set /opt/hadoop-1.1.2/conf/masters
```properties
hadoop
```
* set /opt/hadoop-1.1.2/conf/slaves
```properties
hadoop
```
* format name node
```shell
hadoop namenode -format
```

## start hadoop service##
```shell
cd ${PATH_TO_YOUR_HADOOP_DIR}/bin && ./start-all.sh
```

## using jps to verify hadoop statis##
```
jps
```
