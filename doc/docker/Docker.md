# Docker
docker容器

端口转发
通过虚拟机中的端口映射到服务器中的端口，宿主机连接虚拟机中的端口即连接到docker容器中的各应用服务器

## 一、安装docker

```shell
#1、安装需要的软件包
yum install -y yum-utils device-mapper-persistent-data lvm2
#2、设置yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
#3、安装docker
yum install docker-ce -y
```

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://nysyuqyh.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

二、docker 安装Tomcat



```shell
docker
	docker search tomcat    #查询版本
	docker images			#查看目录
	docker pull tomcat:8	#下载
	docker ps	#查看运行中的容器
	docker run -d --name tomcat1 -p 8080:8080 (IMAGE ID)
	# run => 安装并运行容器
	# --name tomcat =>  命名
	# -p 8080:8080 =>前面一个代表虚拟机上映射端口：原始端口，前面的端口可以改变，原始端口不能改变
    # -v => 映射容器内的文件夹：虚拟机中的文件夹
	# -e MYSQL_ROOT_PASSWORD=123456 =>初始化MYSQL密码
    # -d => 代表容器在后台运行不占领窗口
    
    
    docker ps	#查看运行中的容器
    docker ps -a #查看所有的容器，包括未运行的容器
    docker stop #停止容器
    docker start #运行容器
    docker restart #重启容器
    docker rm #删除容器
    docker rmi #删除镜像
	
	
```

```
	docker search tomcat    #查询版本
	docker images			#查看目录
	docker pull tomcat:8	#下载
	docker images
	docker ps	#查看运行中的容器
	docker run -d --name tomcat1 -p 8080:8080 [tomcat IMAGE ID]
```

安装mysql

```
docker pull mysql:5.6
docker images
docker run -d --name mysql1 -p 3360:3306 -e MYSQL_ROOT_PASSWORD=ok [mysql IMAGES ID]
```

账号：maria_dev
密码：maria_dev

