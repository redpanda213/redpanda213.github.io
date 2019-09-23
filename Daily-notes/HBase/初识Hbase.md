# Hbase

## 一、安装

## 二、配置

```shell
vi conf/hbase-env.sh
```

```shell
27  export JAVA_HOME=/opt/inst/jdk180
128 export HBASE_MANAGES_ZK=false  #关闭hbase内置zookeeper，才能让外部的zookeeper管理hbase
```

```shell
vi conf/hbase-site.xml
```

```xml
<property>
	 <name>hbase.rootdir</name>
	 <value>hdfs://192.168.56.110:9000/hbase</value>
</property>
<property>
	 <name>hbase.cluster.distributed</name>
	 <value>true</value>
</property>
<property>
	 <name>hbase.zookeeper.property.dataDir</name>
	 <value>/home/cm/hbase</value>
</property>
<property>     
     <name>hbase.zookeeper.property.clientPort</name>                  
	 <value>2181</value>    
</property>
```

```shell
vi /etc/profile
```

```
export HBASE_HOME=/opt/bigdata/hbase120
export PATH=$PATH:$HBASE_HOME/bin
```



## 三、运行

```shell
start-hbase.sh
hbase shell
#hbase(main):001:0>
jps
#5481 HMaster
#5611 HRegionServer
```

## 四、关闭

```shell
stop-hbase.sh#先关闭hbase
zkServer.sh stop#再关闭zookeeper
stop-all.sh#关闭hadoop
```

