# Maven的安装配置及创建IDEA项目

##  1、Maven下载
- ***这里使用win10系统*** 
- https://maven.apache.org/download.cgi  下载（选择.zip格式）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809164501917.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_16,color_FFFFFF,t_70)

## 2、在系统环境变量中配置maven
- 新建：MAVEN_HOME     和JAVA_HOME类似，地址写到bin目录上一层
- 编辑修改：在PATH中添加 __%MAVEN_HOME%\bin__
- 完成后，可在cmd中使用mvn -version查看是否安装成功，如下图所示即表示配置成功

__*MAVEN_HOME变量：*__
![MAVEN_HOME环境变量](https://img-blog.csdnimg.cn/20190809164818727.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_16,color_FFFFFF,t_70)__*PATH变量：*__![PATH环境变量](https://img-blog.csdnimg.cn/20190809165129176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_16,color_FFFFFF,t_70)
![cmd中查看maven版本，出现框中内容即环境变量配置完成](https://img-blog.csdnimg.cn/20190809165720707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_16,color_FFFFFF,t_70)
## 3、创建本地仓库repository
- 创建一个本地仓库，自定义命名，我这里命名为maven-repository
- 打开**settings.xml**文件，文件一般在apache-maven-3.6.1中的**conf**文件夹中
- 将仓库地址在<settings>中配置  添加 **\<localRepository>仓库路径\</localRepository>**
![maven仓库](https://img-blog.csdnimg.cn/2019080917154283.png)


## 4、设置公共仓库提供我们下载需要的包
- 同样的，在settings.xml文件中添加阿里云的公共仓库（可用阿里云、腾讯或其他的公共仓库）

        <mirror> 
        <id>nexus-aliyun</id>   
        <mirrorOf>*</mirrorOf>   
        <name>Nexusaliyun</name>  
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>  
        </mirror>

![配置公共仓库](https://img-blog.csdnimg.cn/20190809173045475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_16,color_FFFFFF,t_70)
## 5、idea中创建maven
- 新建项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809173814802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_10,color_FFFFFF,t_70)
- 选择Maven，勾选“Create from archetye”，再选择webapp，完成
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080917392277.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_10,color_FFFFFF,t_70)
- 设置包名和模块名
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809173947971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_10,color_FFFFFF,t_70)
- 选择maven安装的路径，勾选Override
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809174001774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_10,color_FFFFFF,t_70)
- 这一步不用设置，直接Next即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809174014957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_10,color_FFFFFF,t_70)
- 安装好后的界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190809174034465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlZHBhbmRhMjEz,size_10,color_FFFFFF,t_70)

<center>[返回主页](README.md)</center>











































