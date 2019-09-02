# Redis

## 一、 安装

```shell
wget http://download.redis.io/releases/redis-2.8.17.tar.gz
tar -zxvf redis-2.8.17.tar.gz
cd redis-2.8.17
make
```



## 二、配置

```shell
vi redis-2.8.17/redis.conf

bind  192.168.56.110#主机ip
```



## 三、运行

```shell
cd redis-2.8.17/src
./redis-server ../redis.conf
#启动了redis服务
```

新打开一个连接虚拟机的终端

```shell
#还是在src目录下
#redis-cli -h ip -p port -a password
./redis-cli -h 192.168.56.110 -p 6379
```

## 四、使用

`DEL key` 该命令用于在key存在时删除key

`PEXPIRE key milliseconds`设置key过期的时间以毫秒计

`KEYS pattern`查找所有符合给定模式( pattern)的 key 。

