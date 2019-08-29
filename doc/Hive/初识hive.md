```java
public class MyDemo{
    public static void main(String[] args) throws Exception{
		Map<String, Integer> words = new HashMap<String,Integer>();
        BufferedReader br = new BufferedReader(new FileReader("d:/a.txt"));
        String line = br.readline();
        while(line!=null){
            Stringp[] wds = line.split(" ");
            for(String wds : wds){
                if(words.containKey(wd)){
                    words.put(wds,words.get(wd)+1);
                }else{
                    words.put(wds,1);
                }
            }
            line=br.readline();
        }
        br.close();
        words.forEach((k,v)->{
            System.out.println(k+":"+v);
        });
    
    } 
}
```

# **hive**

自身不存储数据，相当于一个空壳子

将表的结构信息记录到=>mysql(结构信息)中

文件=>database=>table

hive可以将文件中数据，加载到mysql中存放表结构的空表中

## 一、下载

## 二、安装

1、解压缩，移动到一个熟悉的目录，并重命名hive110（110为版本）

```
tar -zxvf hive-1.1.0-cdh5.14.2.tar.gz
mv hive-1.1.0-cdh5.14.2.tar.gz  /opt/bigdata/hive110
```

2、导入mysql-connector-java-5.1.38.jar 放在`路径为/hive110/lib/下`

3、修改配置文件

1>>>>>>>>>>>>>>>>

```shell
cd conf
cp hive-env.sh.template hive-env.sh
echo $HADOOP_HOME /opt/bigdata/hadoop260
vi hive-env.sh

HADOOP_HOME=/opt/bigdata/hadoop260
export HIVE_CONF_DIR=/opt/bigdata/hive110/conf
```

2>>>>>>>>>>>>>>

```
vi hive-site.xml
```

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
	<property>
		<name>javax.jdo.option.ConnectionURL</name>
		<value>jdbc:mysql://192.168.56.110:3306/hive?createDatabaseIfNotExist=true</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionDriverName</name>
		<value>com.mysql.jdbc.Driver</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionUsername</name>
		<value>root</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionPassword</name>
		<value>ok</value>
	</property>
</configuration>
```





4、配置环境变量

```shell
vi /etc/profile

export HIVE_HOME=/opt/bigdata/hive110
export PATH=$PATH:$HIVE_HOME/bin

source /etc/profile
```

## 三、运行

1、启动mysql

```
mysql应在后台运行
mysql>show  variables like '%char%'; 查看mysql字符设置
```



2、启动hadoop

```
start-all.sh
```



3、启动hive

```
hive
```

4、使用sql语句建库建表

成功后应该在mysql库中多了一个hive数据库

其中TBL_COL_PRIVS表为空表不放数据

```mysql
hive>create database mydemo;
hive>use mydemo;
hive>create table userinfos(userid int);
hive>insert into userinfos(userid) values(1);
```

5、通过浏览器打开Hadoop【192.168.56.110:50070】(192.168.56.110为当虚拟机的ip)

```shell
#可以在虚拟机内命令行输入如下命令，查看数据
hdfs dfs -text /user/hive/warehouse/mydemo.db/userinfos/000000_0
```





> schematool -initSchema -dbType mysql #初始化mysql



在一张表中可能可以创建的列不是无限的，

将一张表可以分裂成三张表，创建表则可以创建很多。

用无数张表代替一张表，列簇

按照需求寻找特定列簇，返回放在一个什么地方