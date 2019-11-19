---
layout: article
title: idea打包部署springboot项目
mathjax: true
---

# 两种方案：maven或打成jar

## maven   
![image](https://user-images.githubusercontent.com/54270073/69002735-d632fc80-092f-11ea-8764-6fe81f104157.png)    
### 遇到的问题
1. 打包时提示：没有setting文件，到设置中，maven去设置maven的setting的目录   
2. 项目发布后，访问页面，告知**.jsp 未找到 ，去查看项目的jar包发现未将jsp目录里的jsp文件打包的jar中       

![image](https://user-images.githubusercontent.com/54270073/69001720-90ba0380-091e-11ea-8fae-7e1018a3a9d2.png)   

解决办法：
- 首先在在pom.xml中<build>下添加：   

```
<resources>
            <resource>
                <directory>src/main/webapp</directory>
                <!--注意此次必须要放在此目录下才能被访问到 -->
                <targetPath>META-INF/resources</targetPath>
                <includes>
                    <include>**/**</include>
                </includes>
            </resource>
        </resources>
```
- 其次在application.jsonznong 添加：   
```
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
```    
- 在pom.xml中<build>下<plugins>标签内加入如下内容   
```   
            <plugin>   
                <groupId>org.springframework.boot</groupId>   
                <artifactId>spring-boot-maven-plugin</artifactId>   
                <!--此版本可解决访问不到jsp页面-->   
                <version>1.4.2.RELEASE</version>   
            </plugin>   
            <plugin>   
```

## 打成jar包

