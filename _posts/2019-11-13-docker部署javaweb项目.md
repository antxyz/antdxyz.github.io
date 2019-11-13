---
layout: article
title: docker之制作tomca部署javaweb项目镜像
mathjax: true
---

# 制作tomca部署javaweb项目镜像所需的Dockerfile以及相关文件

1. 首先，在本地任意目录下创建文件夹calorpro，在该文件夹下创建Dockerfile文件、run.sh文件和create_tomcat_admin_user.sh文件
2. 在官网下载tomacat和jdk压缩文件，并解压到当前文件夹，版本根据项目子轩，本文使用的tomcat版本为8.0.48，jdk版本为1.8.0_181
3. 将自己的javaweb项目打成war包也放到当前目录下
4. 最终的目录结构如下：![image](https://user-images.githubusercontent.com/54270073/68737499-95c93b00-061e-11ea-96ab-319020cc2626.png)
5. 准备好以上工具和项目包后，准备编辑Dockerfile，run.sh和create_tomcat_admin_user.sh文件

   - Dockerfile内容如下：
```
//dockerfile中不支持apt ，必须用apt-get 指令，可能是基础镜像的原因
FROM ubuntu:18.04

MAINTAINER NSLab

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y tzdata
RUN echo "Asia/Shanghai" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

RUN apt-get install -yq --no-install-recommends wget pwgen ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV CATALINA_HOME /tomcat
ENV JAVA_HOME /jdk

ADD apache-tomcat-8.0.48 /tomcat
ADD jdk1.8.0_181 /jdk

COPY atlas-controller.war /tomcat/webapps


ADD create_tomcat_admin_user.sh /create_tomcat_admin_user.sh
ADD run.sh /run.sh
RUN chmod +x /*.sh
RUN chmod +x /tomcat/bin/*.sh

EXPOSE 8080
CMD ["/run.sh"]   
```


   -  run.sh内容如下：   
  
```
#!/bin/bash

if [ ! -f /.tomcat_admin_created ]; then
    /create_tomcat_admin_user.sh
fi

# /user/sbin/sshd -D &
exec ${CATALINA_HOME}/bin/catalina.sh run

```
   - create_tomcat_admin_user.sh内容如下：
```
#!bin/bash
if [ -f /.tomcat_admin_created ]; then
    echo "Tomcat 'admin' user already created"
    exit 0
fi 

PASS=${TOMCAT_PASS:-$(pwgen -s 12 1)}
_word=$( [${TOMCAT_PASS} ] &&echo "preset" || echo "random" )

echo "=> Creating and admin user with a ${_word} password in Tomcat"
sed -i -r 's/<\/tomcat-users>//' ${CATALINA_HOME}/conf/tomcat-users.xml
echo '<role rolename="manager-gui"/>' >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo '<role rolename="manager-script"/>' >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo '<role rolename="manager-jmx"/>' >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo '<role rolename="admin-gui"/>' >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo '<role rolename="admin-script"/>' >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo "<user username=\"admin\" password=\"${PASS}\" roles=\"manager-gui,manager-script,manager-jmx,admin-gui,admin-script\"/>" >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo '</tomcat-users>' >> ${CATALINA_HOME}/conf/tomcat-users.xml
echo "=>Done!"

touch /.tomcat_admin_created

echo "============================================================"
echo "You can now configure to this Tomcat server using:"
echo ""
echo "      admin:${PASS}"
echo ""
echo "============================================================"
```

6. 完成以上工作后，就可以创建自己的镜像了

# 
# Dockerfile使用说明

1. 首先，在dockerfile目录下，执行如下命令:
  ```
  docker build -t 镜像名：标签 .
  
  //例：docker build -t tomcat:1.0 .
  //注意：命令最后的 '.' 不要忘记添加

  ```
2. 执行完第一步后，在docker中将构建出一个镜像：tomcat：1.0，接下来可以使用这个镜像进行创建自己的容器
  

3. 创建容器指令：
  ```
  docker run -it --name ant -p ip:port:8080 tomcat:1.0
  
  //ip:port填写形式：0.0.0.0:32774（例），建议将端口号填写上

  //此命令使用了镜像：tomcat:1.0，创建了一个名为ant的容器，其8080端口映射到了ip:端口

  ```

4. 如容器创建成功，则打开浏览器在地址栏中输入：ip:port/atlas-controller/combine.jsp ，即可访问本系统。
  
5. 如果本子网的机器无法访问 ip:port/atlas-controller/combine.jsp 地址，请检查防火墙是否开放了该端口。

# 关于本文中工作出现的问题描述与解决

- 端口占用问题   
  主要是出现在创建容器阶段时出现的问题
- 端口开放问题   
其他机器访问不了相应的ip：port，检查防火墙是否开放该端口
- javaweb项目中路径分隔符的问题   
编程是尽量不要用"\",应使用"/",主要是为了考虑在linux生产环境下"\"不被识别为路径分隔符
- tomcat和jdk版本问题
当出现500问题是，可以检查是否tomcat版本过高的问题，jdk最好和项目设置一样的版本
