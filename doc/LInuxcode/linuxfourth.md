# linux基础（4）

## 更改mysql编码格式

### 1、查看当前编码

登录数据库：

````
mysql -u用户名 -p密码
````

查看编码：

````
show variables like 'char%'
````

### 2、修改my.cnf文件

````
vi /etc/my.cnf
````

添加

````
[clinet]
default-character-set=utf8

[mysqld]
character-set-server=utf8
````

### 3、重启数据库

````
service mysql restart
````

### 4、查看是否更改成功

````
mysql -u用户名 -p密码

mysql>status
````

## 建库、建表

````
mysql>create database mydemo;

mysql>use mydemo;

mysql>create table userinfos(userid int primary key not null auto_increment,username varchar(20) not null,birthday date not null);

mysql>show databases;

mysql>desc userinfos;
````



热备份

````
mysqldump -u用户名 -p密码 库名 > /root/mydemo.sql
````

````
source /root.mydemo.sql
````

※注意一般情况下用户会填入错误属性，错误格式，或者不填，需要先进行数据清洗，但是前提是源数据不能被改动！！！切记！！！

※空白数据      可能位空字符串' '（\0）   可能为null 

