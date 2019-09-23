# HBase概述

- 领先
  - 面向列存储的数据库
  - 分布式的hash map
  - 基于google big table 论文
  - 使用hdfs作为存储并利用其可靠性
- 特点
  - 数据访问速度快，响应时间约2-20毫秒
  - 支持随机读写，每个节点20k~100k+ ops/s
  - 可扩展性，可扩展到20,000+节点

## 发展历史

## 用户群体

## 应用场景

## HBase生态圈

## 逻辑架构

```
Table					(HBase table)
	Region
		Store
			MemStore
			StoreFile
				Block
```



### 存储

0、Zookeeper

Ⅰ、存储元数据信息

Ⅱ、

Ⅲ、

Ⅳ、

1、HMaster管理者

HBase高可用指的是HMaster的高可用

HBase可以将高可用的信息、元数据信息存到Zookeeper，实际上是HMaster干这个事。

HMaster管理整个集群，掌握着整个集群的元数据信息，将元数据信息存到Zookeeper。

HMaster随机分派任务，

2、HRegionServer

客户端的读写请求，由这个家伙处理HRegionServer

业务层面上的扩展性（可扩展）

HRegionServer中是怎么管理这些数据的呢？很复杂。

一个RegionServer中维护了多个Region

3、HLog



先将你进行的操作写到一个文件中保存下来，防止在内存中丢失

一台服务器中有一个，一一对应

如果有数据丢失，可从HLog中恢复



4、HRegion

一个Region对应一张表

一张表对应一个Region或多个Region，在数据量很大（HBase往往存储的数据、表量都很大）是，会对表进行切分。

HRegionServer

---------------------------------------------------------------------------------------------

H	|	HRegion、                                                           Region、Region、Region、Region、……

L	 |	Store					

o	 |	Mem Store、StoreFile[Hfile]

g	 |	  ↓

↓			  ↓

|HDFS Client|↓

---------------------------------------------------------------------------------------------------------------------------------------------

HDFS ---> |DataNode| |DataNode| |DataNode| |DataNode|

5、block和mem store之间有个通道，复制有mem store 上的地址，新共呢个给

表太大了，会切成多个region来进行存放，

如1-2000的数据量太大

1-1000可以存在region1、1001-2000可以存在region2

store呢？也会随着region的切分而切分吗？在切分触发之前，即region还没切分，一个store对应一个列族，但是一旦发生了切分，store也被切分，假设被切成两部分S1、S2，S1依旧在region上，S2被分到region2上进行维护。

每个Store中都有自己的Mem Store，当Mem Store满了之后，会将数据刷写到磁盘（HDFS上）

刷写到HDFS上之后叫StoreFile，以Hfile的形式存储。    Mem Store由一定条件触发书写，如数据大小或者时间

## HBase Shell

- HBase Shell是一种操作HBase的交互模式

| **命令类别** | **命令**                                                     |
| ------------ | ------------------------------------------------------------ |
| General      | version, status, whoami, help                                |
| DDL          | alter, create, describe, disable, drop, enable, exists, is_disabled, is_enabled, list |
| DML          | count, delete, deleteall, get, get_counter, incr, put, scan, truncate |
| Tools        | assign, balance_switch, balancer, close_region, compact, flush, major_compact, move, split, unassign, zk_dump |
| Replication  | add_peer, disable_peer, enable_peer, remove_peer, start_replication, stop_replication |

## HBase操作

### 启动

```shell
start-all.sh #启动hadoop
zkserver2.sh start #启动zookeeper
hbase/bin/start-hbase.sh #启动hbase
hbase shell
```

### 基本操作

```
hbase(main):001:0> version     --查看版本  
1.2.0-cdh5.14.2, rUnknown, Tue Mar 27 13:31:54 PDT 2018
---------------------------------------------------------------
hbase(main):002:0> whoami      --查看集群状态
root (auth:SIMPLE)
    groups: root
---------------------------------------------------------------
hbase(main):003:0> status      --查看
1 active master, 0 backup masters, 1 servers, 0 dead, 2.0000 average load
---------------------------------------------------------------
hbase(main):001:0> list        --查看表
TABLE               
student                      
1 row(s) in 0.2650 seconds
=> ["student"]

```

**put语法：**

t1:table  

r1:rowkey

c1:column

value:value

ts1:timestamp

```
  hbase> put 'ns1:t1', 'r1', 'c1', 'value'
  hbase> put 't1', 'r1', 'c1', 'value'
  hbase> put 't1', 'r1', 'c1', 'value', ts1
  hbase> put 't1', 'r1', 'c1', 'value', {ATTRIBUTES=>{'mykey'=>'myvalue'}}
  hbase> put 't1', 'r1', 'c1', 'value', ts1, {ATTRIBUTES=>{'mykey'=>'myvalue'}}
  hbase> put 't1', 'r1', 'c1', 'value', ts1, {VISIBILITY=>'PRIVATE|SECRET'}

```

**create语法：**

```
hbase> create 't1', {NAME => 'f1'}, {NAME => 'f2'}, {NAME => 'f3'}
hbase> create 't1', 'f1', 'f2', 'f3'
```



```
create 'customer', {NAME =>'addr'}, {NAME =>'order'}	-- 创建一个表
list					                            -- 列出HBase所有的表
desc 'customer' 				                    -- 查看表的详细信息

put 'customer', 'jsmith', 'addr:city', 'montreal'   -- 添加数据
put 'customer', 'jsmith', 'addr:start', 'ON'
put 'customer','jsmith','addr:date','2015-12-19'
put 'customer','jsmith','addr:numb','123456'
#指定列的时候必须指定到列族#
```

```
get 'customer', 'jsmith'			                -- 获取数据
-----------------------------------------------------------------
结果:
COLUMN                        CELL                                          addr:city                    timestamp=1568689341406, value=montreal
addr:state                   timestamp=1568689940335, value=ON 
addr:numb                    timestamp=1568690302971, value=123456         addr:state                   timestamp=1568689940335, value=ON 
------------------------------------------------------------------
```

```
desc 'customer'
-----------------------------------------------------------------
结果：
COLUMN FAMILIES DESCRIPTION                                               {NAME => 'addr', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA
_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BL
OCKSIZE => '65536', REPLICATION_SCOPE => '0'}                                                                    
{NAME => 'order', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DAT
A_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', B
LOCKSIZE => '65536', REPLICATION_SCOPE => '0'}   
------------------------------------------------------------------

alter 'customer',NAME=>'order',VERSIONS=>5    --改、更新操作
desc ‘customer’
--------------------------------------------------
结果：
COLUMN FAMILIES DESCRIPTION                                                {NAME => 'order', BLOOMFILTER => 'ROW', VERSIONS => '5', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DAT
A_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', B
LOCKSIZE => '65536', REPLICATION_SCOPE => '0'}                             
--------------------------------------------------

```

