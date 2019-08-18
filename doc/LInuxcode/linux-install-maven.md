# linux下安装maven

## 一、下载maven

## 二、设置本地仓库

此仓库用于放置maven下载的jar包，文件夹名不要用中文！！以免在以后使用过程中出现未知错误！！

- 选择一个盘新建文件夹

  我这里在E:/maven-repository

## 三、配置国内源及仓库地址

## 四、配置环境变量

## 五、验证是否配置成功

vi  /maven/conf/setting.xml

-53-

````
  <localRepository>/path/to/local/repo</localRepository>
````



-160-

 ````
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

## 配置环境变量

vi  /etc/profile

````
export MAVEN_HOME=/opt/maven354
export PATH=$PATN:$MAVEN_HOME/bin
````

※setting

keymap  >   Eclipse

![1565945958610](C:\Users\王先生\AppData\Roaming\Typora\typora-user-images\1565945958610.png)
