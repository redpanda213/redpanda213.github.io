# linux虚拟机中安装maven

## 一、下载maven

搜索maven官网，点击左侧列表中`Download`

下载`apache-maven-3.6.1-bin.tar.gz`文件(或者其他版本)

```shell
mkdir /opt/inst  //在虚拟机中创建一个文件夹
tar -zxvf apache-maven-3.6.1-bin.tar.gz -C /opt/inst   //将压缩包解压到刚创建的文件夹中
```

使用Xftp导入进虚拟机，

## 二、设置本地仓库

````
mkdir /opt/maven-repo
````

 此仓库用于放置maven下载的jar包

> 在window系统下建立仓库时路径中不要用中文！！以免在以后使用过程中出现未知错误！！



## 三、配置国内源及仓库地址

**解压后的文件名称太长，我重命名为maven354**

```
vi /opt/inst/maven354/conf/settings.xml    //打开setting.xml文件
```

添加以下内容，分别在大约56行`settings`标签，160行`mirrors`标签

````
<localRepository>/opt/maven-repo</localRepository> // 配置本地仓库路径
  
<mirror>
	<id>alimaven</id>
	<mirrorOf>central</mirrorOf>
	<name>aliyun maven</name>
	<url>https://maven.aliyun.com/repository/central</url>
</mirror>
<mirror>
	<id>nexus-aliyun</id>
	<mirrorOf>*</mirrorOf>
	<name>aliyun maven</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>

````



## 四、配置环境变量

```shell
vi /etc/profile      //没错就是当初你配置java环境变量的地方，其实它就是linux的系统变量文件

跳至文本最后
export MAVEN_HOME=/opt/inst/maven354  
//安装的maven的路径,不记得路径可以cd到存放maven的文件夹pwd看一下
export PATH=$MAVEN_HOME/bin:$PATH

```



## 五、验证是否配置成功

````
mvn -version
//出现以下结果说明配置成功
Apache Maven 3.5.4 (1edded0938998edf8bf061f1ceb3cfdeccf443fe; 2018-06-17T14:33:14-04:00)
Maven home: /opt/inst/maven354
Java version: 1.8.0_111, vendor: Oracle Corporation, runtime: /opt/inst/jdk181/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-514.el7.x86_64", arch: "amd64", family: "unix"
````

