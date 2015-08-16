# hadoopTrial for 2.x
* CentOS 6.6 x64
* JDK：1.7.0_55 64位
* Hadoop：2.x

## install java dev environment##
(for example)[https://github.com/draculavlad/JavaDevEnv]

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

## install basic lib & tools
```shell
sudo yum install -y svn 
sudo yum install autoconf automake libtool cmake
sudo yum install -y ncurses-devel
sudo yum install -y openssl-devel
sudo yum install -y gcc*
```

## install protobuf##
* (find package from)[https://code.google.com/p/protobuf/downloads/list]
```shell
tar -zxf protobuf-2.5.0.tar.gz
mv  protobuf-2.5.0 /opt
cd /opt/protobuf-2.5.0 
sudo ./configure
sudo make
sudo make check
sudo make install
```
* using shell command to check the installation result
```shell
protoc
```

## download source code of hadoop 2.x##
```shell
cd /opt
mkdir compile
svn checkout http://svn.apache.org/repos/asf/hadoop/common/tags/release-2.2.0
```
* things to do before compile and build
* add dependency to hadoop-common-project/hadoop-auth/pom.xml
```xml
<dependency>
  <groupId>org.mortbay.jetty</groupId>
  <artifactId>jetty-util</artifactId>
  <scope>test</scope>
</dependency>
```
* compile and build
```shell
mvn package -Pdist,native -DskipTests –Dtar(please manual input)
```
* how to verify compilation result
```shell
cd ${PATH_TO+YOUR_HADOOP_SRC_DIR}/hadoop-dist/target/hadoop-2.2.0/lib/native
file ./libhadoop.so.1.0.0
```
* if libhadoop.so.1.0.0 check result shows ELF 64-bit LSB then success
