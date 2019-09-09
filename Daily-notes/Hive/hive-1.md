# Hive

> Hive的数据类型	
>
> Hive的数据库和操作||和mysql 关系型数据库的区别
>
> Hive的数据分区
>
> Hive的数据分桶

## 什么是Hive

- 基于Hadoop的数据仓库解决方案
  - 将结构化的数据文件映射为数据库表（需要将文件转化为库表的形式）
  - 提供类sql的出巡语言HQL(Hive Query Language)
  - Hive让后更多的人使用Hadoop

- Hive成为Apache顶级项目
  - 始于2007年的Facebook

## Hive的优势、特点

- HQL类SQL语法，简化MR开发
- 简化的优化模型
- 支持在不同的机选框架上运行
- 支持在HDSF和HBaase上临时查询数据

- 支持用户自定义函数、格式
- 成熟的JDBC和ODBC驱动程序，用于ETL 和BI(Extract Transformation Load和商业智能)
- 稳定可靠(真是生产环境)的批处理
- 有庞大活跃的社区

> Stinger Phase 1 ORC HiveServer2 -----2013.5
>
> Hive 1.0.0 --------2015.2
>
> **Hive 1.2.0** -------2015.05(面试讲这个版本)*
>
> Hive 2.0.0 -------2016.02 (2版本后不支持hadoop 1版本)
>
> Hive 2.1.0 --------2016.06

> Hive和HDFS相比
>
> HDFS代码的执行效率更高，Hive的开发效率更高（Hive代码需要解析）

## Hive 元数据管理

- 记录数据仓库中模型的定义、各层级之间的映射关系

- 存储在关系数据库中
  - 默认Derby**(单线程，但客户，适合本地测试使用)**，轻量级内嵌SQL数据库
  - 存储在.metastore_db中

- 实际生产中一般存储在Mysql中
  - 修改配置文件hive-site.xml

```hive
select * from TBLS; #记录操作的表
select * from DBS;  #DB_LOCATION_URI  数据库的路径
```

## Hive Interface 

> 图形用户界面（Graphical User Interface-----GUI）

### 命令行窗口模式（command-line interfacev -------CLI）

```shell
hiveserver2#启动beeline需要先启动hiveserver2
beeline -u 'jdbc:hive2://localhost:10000/default'

```

- 两种工具：Beeline和Hive命令行（CLI）

- 两种模式：命令行模式和交互模式

- 命令行模式

  ```shell
  hive -e 'show databases'<query in quote>
  hive -f <query file name>
  
  hive --hivevar key=value(Define Variable)
  ```

- 交互模式

### 其他使用环境

- Hive Web Interface(As part of Apache Hive)

- Hue（）----->CPH

- Ambari Hive View(Hortonworks)------>HDP

- JDBC/ODBC
  - Informatica,Taland等（Informatica是操作数据库的，我们使用的是........）
  - Tableau，QlikView，Zeppelin等
  - Oracle SQL Developer，DB Visualizer等

> 面试中问道是否使用过Informatica，要说了解过知道，但是我是用的是.........

## Hive 数据类型

### 原始类型

- **STRING**

- **DECIMAL**

- **DOUBLE**

- **INT**

- DATE

- TIMESTAMP

### 复杂数据类型

- ARRAY：存储相同类型数据  ARRAY< string>
- MAP：具有相同类型的键值对 MAP<String,String>
- STRUACT：封装了一组字段  STRUCT< fruit:string,weight:int>  (对key不关注，只关注value)

## Hive元数据结构

| 数据结构  | 描述   | 逻辑关系               | 物理存储（HDFS） |
| --------- | ------ | ---------------------- | ---------------- |
| Database  | 数据库 | 表的集合               | 文件夹           |
| Table     | 表     | 行数据的集合           | 文件夹           |
| Partition | 分区   | 用于分割数据           | 文件夹           |
| Buckets   | 分桶   | 用于分隔数据           | 文件             |
| Row       | 行     | 行记录                 | 每列中指定的位置 |
| Columns   | 列     | 列记录                 | 每行中指定的位置 |
| Views     | 视图   | 逻辑概念，可跨越多张表 | 不存储数据       |
| Index     | 索引   | 记录统计数据信息       | 文件夹           |

> 分区Partition：在表的基础上进行了优化，例如通过日期时间分区，20190901一个文件夹；20190902一个文件夹
>
> 分桶Buckets：

### 数据库(Databases)

表的集合，HDFS中表现为一个文件夹

  默认为hive.metastore.warehouse.dir

指定hive管理的目录

```hive
select current_database() --查看当前库

alter database kb02 set owner user dayongd;

describe database default; --more details than 'show',such as location
#注释中不要使用分号

drop database if exists kb02 cascade;
```

### 数据表(Tables)

- 分为内部表和外部表
- 内部表（管理表）
  - HDFS中为所有数据库目录下的子文件
  - 数据完全由Hive管理，删除表（元数据）会删除数据
- 外部表
  - 数据保存在指定位置的HDFS路径中
  - Hive不完全管理数据，删除表（元数据）不会删除数据

```hvie
Create external table if not exists employee_external(
	name string,
	work_place ARRAY<string>,
	sex_age STRUCT<sex:string,age:int>,
	skills_score MAP<string,int>,
	depart_title MAP<STRING,ARRAY<STRING>>
)
COMMENT ‘This is an external table’
ROW FORMAT DELiMiTED
FIELDS TERMINATED BY '|'
COLLECTION ITEMS TERM TERMINATED BY ','
MAP KEYS TERMINATED BY ':'
STORED AS TEXTFILE
LOCATION '/user/root/employee';  --指定表的存放位置，创建外部表
```

适用场景：内部表的功能多于外部表，可以基于原始数据创建一个内部表

## Hive建表

Storage SerDe

- SerDe： Seializer and Deserializer
- Hive支持不同和类型的Storage SerDe
  - 

```hive
ROW FORMAT SERDE
‘org.apache.hadoop.hive’
```

### 高阶语句

CTAS and WITH

- CTAS - as select

```hive
create table ctas_employee as select * from employee;
```

CTAS ***不能创建***partition，external，bucket table

- CTE（）

```hive
create table cte_employee as
with
r1 as (select name from r2 where name = 'Michael'),
r2 as (select name from employee where sex_age.sex = 'Male'),
r3 as (select name from employee where sex_age.sex = 'Female'),
select * from r1 union all select * from r3;
```

- Like

```hive
CREATE TABLE employee_like LIKE employee;
--只复制表结构
```

### 创建临时表

- 临时表是应用程序自动管理在复杂查询期间生成的中间数据的方法
  - 表只对当前session有效，session退出后自动删除
  - 表空间位于/tmp/hive-<user_name>（安全考虑）
  - 如果创建的临时表表名已存在，实际用的是



```hive
--查看创建表的语句
show create table userinfos;

```

```|                   createtab_stmt                   |
+---------------------------------------------------------------+--+
| CREATE TABLE `userinfos`(                                     |
|   `userid` int)                                               |
| ROW FORMAT SERDE                                              |
|   'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'        |
| STORED AS INPUTFORMAT                                         |
|   'org.apache.hadoop.mapred.TextInputFormat'                  |
| OUTPUTFORMAT                                                  |
|   'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat' |
| LOCATION                                           |
|   'hdfs://elk-2:9000/user/hive/warehouse/mydemo.db/userinfos' |
| TBLPROPERTIES (                                    |
|   'COLUMN_STATS_ACCURATE'='true',                  |
|   'numFiles'='1',                                  |
|   'numRows'='1',                                   |
|   'rawDataSize'='1',                               |
|   'totalSize'='2',                                 |
|   'transient_lastDdlTime'='1566979361')   		 |
+----------------------------------------------------+--+

```

```hive
desc formatted employe;
desc employee;
show create table userinfos;
```

### 表操作

删除

```hive
drop table if exsits employee [with perge]; --with perge直接删除（可选），否则会放到.Trash目录
truncate table employee; --清空表结构
```

修改alter针对元数据

```hive
alter table employee rename to new_employee; --修改表名
alter table c_employee set tblproperties 
```

## Hive分区(Partitions)

### 分区操作

#### 动态分区

1、使用动态分区需要设定属性

```hive
set hive.exec.dynamic.partition=true;
set hvie.exec.dynamic.partition.made=nonstrict;
```





2、动态分区设置方法

```hive
insert into table employee_partitioned partition(year,month)
select name,array('Toronto') as work_place,
named_struct("sex","male","age",30) as sex_age,
map()
map()
year(start_date) as year,month(start_date) as month;
```



#### 静态分区

定义静态分区

```hive

```

分区操作

```hive
--添加分区
alter table employee_partitioned add partition (year=2019,month=7);
alter table employee_partitioned add partition (year=2019,month=8);
--删除分区
alter table employee-partitioned drop partition (year=2019,month=7);
```

