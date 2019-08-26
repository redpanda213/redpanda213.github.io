---
typora-copy-images-to: image
---

# Logstash

## 一、简介

- 具备实时数据传输能力的管道：
- - 支持多种数据源输入

- - 支持多种过滤器
- - 支持多种数据输出目的地

在ELK中作为日志收集器

> Logstash不只是一个input|filter|output的数据流，
>
> 而是一个input|docede|filter|encode|output的数据流

## 二、安装

## 三、运行

````
//进入bin目录下
[root@localhost bin]# ./logstash -e 'input{stdin{}} output{stdout{}}'

````

![1566465760873](D:%5C%E7%AC%94%E8%AE%B0%5CELK%20Stack%5Cimage%5C1566465760873.png)



结果：

![1566466186325](D:%5C%E7%AC%94%E8%AE%B0%5CELK%20Stack%5Cimage%5C1566466186325.png)

## 四、原理

### 如何工作

- logstash对任何事件处理分为三个阶段、
  - 输入（input）：必需，如stdin、file、http、exec……
  - 过滤器（filter）：可选，如grok、mutate……
  - 输出（output）：必需，如stout、elasticsearch、file……

- 编解码器
  - 作为输入或输出插件的一部分，分离传输与序列化过程

可以将命令写成文件

```json
mkdir logfilter
vi myfilter.conf


input{
	stdin{}
}
filter{}
output{
	stdout{
		codec=>rubydebug
	}
}
```

```
input{
	file{
		path =>"/var/log/message"
		start_position =>"beginning"
		sincedb_path =>"/dev/null"
	}
}
filter{}
output{
	stdout{
		codec=>rubydebug
	}
}
```

```json
input{
 file{
  path => '/opt/data/user.csv'   //文件需要使用utf-8
  start_position => "beginning"
 }
}
filter{
 csv{
  separator => ","
  columns => ["userid","username","age","birthday","salary"]
 }
 mutate{
  convert => {
   "userid" => "integer"
   "age" => "integer"
   "salary" => "integer"
  }
 }
}
output{
 stdout{
   codec => rubydebug
 }
}

```

```
=MID($F$1,RANDBETWEEN(0,100),1)
&MID($F$2,RANDBETWEEN(0,100),1)

%{INF:uid}\,%{USERNAME:name}

(?<userid>\d+)\,(?<username>[\u4E00-\u9FA5]+)\,(?<age>\d+)\,(?<birthday>\d+[./-]\d+[./-]\d+)\,(?<salary>\d+)
```

```json
input{
 stdin{
 }
}
filter{
 grok{
  match=>{
    "message" => "(?<userid>\d+)\,(?<username>[\u4E00-\u9FA5]+)\,(?<age>\d+)\,(?<birthday>\d+[./-]\d+[./-]\d+)\,(?<salary>\d+)"
  }
 }
 csv{
  separator => ","
  columns => ["userid","username","age","birthday","salary"]
 }
 mutate{
  convert => {
   "userid" => "integer"
   "age" => "integer"
   "salary" => "integer"
  }
 }
}
output{
 stdout{
   codec => rubydebug
 }
}
```

![1566547995882](D:%5C%E7%AC%94%E8%AE%B0%5CELK%20Stack%5Cimage%5C15665479958812.png)

## 五、Logstash之codec插件

codec就是用来decode(解码)、encode事件的   codec是coder和decoder的缩写

### plain插件

### rubydebug插件

### json插件

### multiline插件