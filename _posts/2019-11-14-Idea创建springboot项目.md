---
layout: article
title: Idea创建springboot项目
mathjax: true
---

# Idea创建springboot项目
## 系统环境：
win10+idea企业版+jdk1.8   
$\color{red}{学生可以使用学校邮箱申请idea企业版}$
## 新建spring项目
1. 打开idea，创建新项目，选择Spring Initializr，选择默认的初始化服务URL即可：   


![image](https://user-images.githubusercontent.com/54270073/68842429-532c5f00-0701-11ea-9797-6fc6403a3faf.png)
1. 填写组和签名，点击next    


![image](https://user-images.githubusercontent.com/54270073/68842684-c03ff480-0701-11ea-8bfa-ff07b6d85484.png)
1. 选择Spring Web即可，点击next   


![image](https://user-images.githubusercontent.com/54270073/68842754-e36aa400-0701-11ea-9f3f-7c41a1d37c60.png)
1. 填写项目名，点击finish   


![image](https://user-images.githubusercontent.com/54270073/68842915-29c00300-0702-11ea-8ef9-598affd7eb34.png)

1. 完成上一步项目创建完成，但是你会发现项目结构中没有，webapp，WEB-INF，web.xml，classes和lib


![image](https://user-images.githubusercontent.com/54270073/68843040-668bfa00-0702-11ea-87cd-42a84a114296.png)

## 添加web.xml
本部会顺便创建webapp和WEB-INF文件夹   
1. 打开工具栏’file'，并点击‘project structure’，   

![image](https://user-images.githubusercontent.com/54270073/68843381-f92c9900-0702-11ea-8abd-53969726a85b.png)
2. 在弹出的弹窗中，选择facets，并点击中框中的‘+’号，选择下拉框中的“Web”选项   

![image](https://user-images.githubusercontent.com/54270073/68843568-4f014100-0703-11ea-94b9-a41a82f89f05.png)

3. 双击弹窗中的项目名

![image](https://user-images.githubusercontent.com/54270073/68843732-92f44600-0703-11ea-834d-512002082a34.png)
4. 修改红框中的路径，上：d:\test\src\main\webapp\WEB-INF\web.xml，下：d:\test\src\main\webapp\WEB-INF   

![image](https://user-images.githubusercontent.com/54270073/68843815-b61ef580-0703-11ea-9385-15b6c0d04a1a.png)
5. 创建成功

## 创建classes和lib目录
1. 在WEB-INF目录下，分别创建以上两个目录，然后到“project structure”中，    
![image](https://user-images.githubusercontent.com/54270073/68844121-40675980-0704-11ea-8912-568f75e92210.png)
2. 点击Modules，并点击右侧的paths项，将红框中的路径修改为对应的classes路径，即可   
![image](https://user-images.githubusercontent.com/54270073/68844208-64c33600-0704-11ea-8fa9-9619b040398a.png)
3.接着，点击‘Dependencies’项，

![image](https://user-images.githubusercontent.com/54270073/68844455-c84d6380-0704-11ea-9874-e776965fd5fd.png)
![image](https://user-images.githubusercontent.com/54270073/68844504-e1561480-0704-11ea-91d4-777b4217c26c.png)
![image](https://user-images.githubusercontent.com/54270073/68844551-f29f2100-0704-11ea-86ca-1f5ae1fd3f66.png)
4. 最后点击‘ok’即可