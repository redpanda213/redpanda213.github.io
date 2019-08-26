## 一、安装gem 

```ejs
yum install gem
```

### 替换为国内源

```
gem source --add http://gems.ruby-china.com --remove http://rubygems.org/
gem source -l
```



## 二、修改Gemfile的数据源地址

```shell
whereis logstashe #查看logstash安装的位置，我的位置：/opt/inst/logstash
cd /opt/inst/logstash
vi Gemfile  #打开文件后，修改source的值为“http://gems.ruby-china.com”
vi Gemfile.jruby-....   #找到 remote 修改它的值为“http://gems.ruby-china.com”
```



## 三、安装logstash-input-jdbc

```shell
cd /opt/inst/logstash/bin
./logstash-plugin install logstash-input-jdbc

#等待安装，如果结果显示success 就说明成功了
```

## 四、导入mysql驱动包

我这里使用的是`mysql-connector-java-5.1.38.jar`，用Xftp导入到虚拟机中

```shell
#同样在logstash目录下新建一个目录
mkdir mysql
#然后可以使用mv命令，将jar放在此文件夹中方便查找
cd mysql
#新建一个jdbc.conf文件
vi jdbc.conf
```

在jdbc.conf中编写如下（此处配置的较为简单，如需更详细的配置设置请查看https://segmentfault.com/a/1190000011784259）：

```shell
input {
 stdin{}
    jdbc {
                jdbc_driver_library => "/opt/inst/logstash/mysql/mysql-connector-java-5.1.38.jar"

                jdbc_connection_string => "jdbc:mysql://192.168.56.110:3306/test"
                jdbc_driver_class => "com.mysql.jdbc.Driver"
                jdbc_user => "root"
                jdbc_password => "ok"

                schedule => "* * * * *"

                jdbc_page_size => 5000
                jdbc_paging_enabled => "true"


                statement => "SELECT * from logs"  #这里使用的表预先已在mysql中创建
                type => "logs"
        }
}
filter{
 json{
  source => "message"
  remove_field => ["message"]
 }
  mutate{
  convert => {
  “watch_from” => "integer"  
  "type" => "integer"
  
  #将这两列类型手动改变为integer，否则mysql中的此列类型es不能识别
  #watch_from tinyint(1) unsigned NOT NULL COMMENT
  #type tinyint(1) unsigned NOT NULL COMMENT
  
}
output {
        elasticsearch {
                 hosts  => ["192.168.56.110:9200"]
                 index  => "test"
                 document_id => "%{id}"

        }
        stdout{
                codec => json_lines
        }
}

```

## 四、启动logstash

````
#在logstash的bin目录下
./logstach -f /opt/inst/logstash/mysql/jdbc.conf
````

## 四、结果使用kibana查看

结果：

```url
http://192.168.56.110:5601/app/kibana#/visualize/create?type=histogram&indexPattern=231c07e0-c7ad-11e9-b21e-1d7dcbaaec1c&_g=(refreshInterval:(display:Off,pause:!f,value:0),time:(from:now%2Fd,mode:quick,to:now%2Fd))&_a=(filters:!(),linked:!f,query:(language:lucene,query:''),uiState:(),vis:(aggs:!((enabled:!t,id:'1',params:(),schema:metric,type:count),(enabled:!t,id:'2',params:(field:duration,missingBucket:!f,missingBucketLabel:Missing,order:desc,orderBy:_term,otherBucket:!f,otherBucketLabel:Other,size:50),schema:segment,type:terms)),params:(addLegend:!t,addTimeMarker:!f,addTooltip:!t,categoryAxes:!((id:CategoryAxis-1,labels:(show:!t,truncate:100),position:bottom,scale:(type:linear),show:!t,style:(),title:(),type:category)),grid:(categoryLines:!f,style:(color:%23eee)),legendPosition:right,seriesParams:!((data:(id:'1',label:Count),drawLinesBetweenPoints:!t,mode:stacked,show:true,showCircles:!t,type:histogram,valueAxis:ValueAxis-1)),times:!(),type:histogram,valueAxes:!((id:ValueAxis-1,labels:(filter:!f,rotate:0,show:!t,truncate:100),name:LeftAxis-1,position:left,scale:(mode:normal,type:linear),show:!t,style:(),title:(text:Count),type:value))),title:'New+Visualization',type:histogram))

```





> 通过wget下载源码
>
> wget https://rubygems.org/rubygems/rubygems-2.6.12.zip
> unzip rubygems-2.6.12.zip
> cd rubygems-2.6.12
> sudo ruby setup.rb
> gem -v