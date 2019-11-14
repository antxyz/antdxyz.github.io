---
layout: article
title: docker在windows环境下搭建私有仓库
mathjax: true
---

# windows环境下，搭建docker本地仓库

```
系统环境：
    windows10
    docker1.40
```

## 为何要搭建本地私有仓库？
- 动机1：随着docker的使用，本地的镜像数量逐渐增多，通过共有仓库管理不方便，需要一个仓库集中存放
- 动机2：存在一些镜像，用户并不想将它们分享出去，则需要一个私有仓库来存放

## 如何搭建私有仓库？
- 方法1：基于容器安装运行，简单方便
- 方法2：本地源码安装运行，可以在本地运行仓库服务

### 基于容器安装运行
1. 首先，创建该容器：
```
docker run -d -p 5000:5000 \
    --restart=always \
    --name register \
    register:2

//默认端口为5000，也可以通过标签'REGISTRY_HTTP_ADDR'自定义仓库的端口,如下：
docker run -d -e REGISTRY_HTTP_ADDR=0.0.0.0:5001\
    -p 5000:5001 \
    --restart=always \
    --name registerx \
    register:2

```
2.如何将镜像上传到本地仓库
```

//先拉取一个镜像做实验
docker pull ubuntu:18.04

//为需要上传的镜像打上特定标签：ip(域名):port/自定义名
docker tag ubuntu:18.04 127.0.0.1:5000/test
```
![image](https://user-images.githubusercontent.com/54270073/68830238-14d57680-06e6-11ea-9bce-784557e6e5ec.png)
```
//通过docker  images指令可以查看到标签已标注，接下来可以将此镜像上传至仓库
docker push 127.0.0.1:5000/test

//上传到本地仓库后，在本地删除该镜像
docker image remove 127.0.0.1:5000/test

//从本地仓库拉取镜像
docker pull 127.0.0.1:5000/test

```
### 本地源码安装运行
