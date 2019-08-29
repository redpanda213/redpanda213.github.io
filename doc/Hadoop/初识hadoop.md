# Hadoop

> **分布式：**
>
> 由分布在不同主机上的进程（程序）协同在一起才能构成整个应用

HDFS：Hadoop Distributed File System

> **大数据4V特征**
>
> Volumn：体量大
>
> Velocity：速度快
>
> Variaty：样式多
>
> Value：价值密度低

hadoop

一款可靠的、而可伸缩的、分布式计算的开源软件

是要给框架、允许跨越计算机集群的大数据集处理，使用简单的编程模型（MapReduce）。

可从单个服务器扩展到几千台主机，每个节点体懂了计算和储存的功能。而不是以来高可用性的机器，

依赖于应用层面上的实现

## hadoop模块

## hadoop安装

1、jdk（建议使用jdk1.8.11）

2、Tar hadoop.tar.gz（建议使用Hadoop 2.7.3）

3、配置JAVA环境变量

/opt/inst/jdk180

4、hadoop5.14.2

解压hadoop

```
#我打算把hadoop放在/opt/bigdata下，所以在opt在先创建一个bigdata目录
cd /opt
tar -zxvf 
mv hadpopd-       /opt/bigdata/hadoop260   #将
mkdir bigdata

```



```shell
cd /hadoop260/etc/hadoop
vi hadoop-env.sh
export JAVA_HOME=/opt/inst/jdk180
vi core-site.xml
```

```html
<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://elk-2:9000</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/opt/hadoopdata</value>
        </property>
        <property>
                <name>hadoop.proxyuser.root.users</name>
                <value>*</value>
        </property>
        <property>
                <name>hadoop.prosyuser.root.groups</name>
                <value>*</value>
        </property>
</configuration>
              
```



```shell
vi hdfs-site.xml
```



```html
<configuration>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>
</configuration>

```



设置映射化简模型框架为yarn

```shell
cp mapred-site.xml.template mapred-site.xml
vi mapred-site.xml。
```

```html
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
</configuration>

```



站点名称

辅助节点管理

```
vi yarn-site.xml
<configuration>
        <property>
                <name>yarn.resourcemanager.lcoalhost</name>
                <value>localhost</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
</configuration>

```



```shell
vi /etc/profile

export HADOOP_HOME=/opt/bigdata/hadoop260
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_INSTALL=$HADOOP_HOME
```



## 启动hadoop

```shell
hdfs namenode -format #格式化
```

```shell
start-all.sh #启动

3090 ResourceManager
2948 SecondaryNameNode
3370 NodeManager
2684 NameNode
2799 DataNode

```

```shell
stop-all.sh #停止
```





```
hdfs dfs -ls /
```

