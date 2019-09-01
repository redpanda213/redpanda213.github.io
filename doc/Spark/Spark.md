# Spark

内存操作，计算引擎框架

内存不稳定容易出问题，spark无法替代mapreduce

mapreduce存到硬盘里

## 一、安装

`spark.apache.org`

## 二、配置

1、

```shell
cd spark243/conf
cp slaves.template slaves
vi slaves
```

```shell
#从机名
bigdata1
```

2、

```shell
cp spark-env.sh.temple spark-env.sh
export SPARK_MASTER_HOST=bigdata#主机名
export SPARK_MASTER_PORT=7077#端口号
export SPARK_WORKER_CORES=2#cpu
export SPARK_DAEMON_MEMORY=3g#给spark分配的内存大小
export SPARK_MASTER_WEBUI_PORT=8888#显示管理界面
```

3、

```shell
cd spark243/sbin
vi spark-config.sh
```

```shell
export JAVA_HOME=/opt/inst/jdk180
```

## 三、运行

```shell
[sbin]   ./start-all.sh
```



> 