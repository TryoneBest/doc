# docker部署应用

## 给运行中的容器绑定外部端口

获得容器IP

docker inspect `container_name` | grep IPAddress

iptable转发端口

iptables -t nat -A DOCKER -p tcp --dport 60000 -j DNAT --to-destination IPAddress:port

可用来绑定端口转发，配合ps -aux | grep 60000确认绑定正常

## 巨坑

gunicorn运行端口时ip不能做127.0.0.1，否则即使端口绑定正常，宿主机也访问不到应用

## 步骤

一般是用dockerfile一步搭建，我这是命令行构建

### pull os

下载一个linux系统镜像，我用的是centos相对较小

### 安装必备环境

启动centos容器，安装环境比如我的后端接口是flask，我需要安装python、flask、gunicorn、flask_cors，还有数据库调用的python库

可以将安装完的容器重新创建为本地镜像以作复用

### 上传项目文件

可以先上传到宿主机，然后用docker命令

```shell
docker cp SRC_PATH TARGET_CONTAINER_ID:TARGET_PATH //复制宿主机文件到容器
docker cp TARGET_CONTAINER_ID:TARGET_PATH SRC_PATH //复制容器文件到宿主机
```

### 运行项目

```shell
docker exec -it CONTAINER_ID /bin/bash //进入容器
gunicorn -b 0.0.0.0:8080 app:app //注意-b IP不能是127.0.0.1，否则即使端口没被占用宿主机也访问不到应用
```

