# 1 开发规划

## 1.1 开发人员

后端开发：田嘉禾

## 1.2 开发计划

于2020年3月至2020年6月期间完成开发

## 1.3 开发环境和工具

需要Java 1.8版本，后端框架为SpringBoot+Hibernate JPA，数据库为MySQL。

使用Intellij IDEA协助开发， Maven进行依赖管理，git进行版本管理。

# 2 总体设计

## 2.1 概念术语描述

### 2.1.1 细胞

* 每个用户对应一个细胞
* 每个细胞有属于自己的模型
* 细胞间合作收集信息，进行游戏
### 2.1.2 NPC

* 以病毒的形象出现
* 单击后能够进行交互，获得问题或知识点或细胞信息
### 2.1.3 细胞信息

* 任意一条细胞的信息介绍
### 2.1.4 问题

* 有四个选项，其中一个选项为正确的生物领域选择题
### 2.1.5 知识点

* 任意一条生物领域的知识点
### 2.1.6 背包

* 用户间互相合作，进行收集信息的背包
* 每次游戏开始时会创建新的背包
* 背包中存储信息，知识点及答对的问题
* 当次游戏中每个用户收集到的知识（含以上三种）都将加入背包
* 背包中存储的知识（含以上三种）达到一定数额时游戏结束
## 2.2 基本设计描述

* 使用REST风格的接口进行前后端数据交互
* 使用jwt token进行权限判定
* SpringBoot通过stomp协议实现双向通信功能
### 2.2.1 系统的可复用性

系统模块间具有隔离性，用户信息及websocket游戏部分互不干扰；

需要增加功能时只需增加对应的Entitiy-Controller-Service，逻辑明确；

整个系统的架构为分层次的。

Entitiy代表数据库中的实体，Repository类负责与数据库进行交互；

Service类为主要逻辑实现的类，注入Repository的实体，实现功能；

Controller类为接口实现的类，负责进行请求的映射，在Controller类中注入Service实体并调用Service实体的方法可以实现接口的功能；

系统的分层模块化使debug变得简洁，容易定位问题。

## 2.3 基本功能描述

### 2.3.1 登录，注册及个人信息

* 用户注册时需要填写用户名，密码，邮箱，选择的细胞模型
* 用户登录时需要填写用户名及密码
* 用户可以设置头像
* 用户可以在登陆后编辑个人信息，包括如下项目
  * 用户名
  * 地区
  * 性别
  * 全名
  * 邮箱
  * 年龄
  * 密码
* 用户可以在个人信息界面查看以往在游戏中获得的知识点
### 2.3.2 游戏过程

* 用户加入游戏，连接到后端服务器，达到一定人数后，后端服务器向所有客户端广播游戏开始
* 所有用户开始进行界面加载，加载完毕后，后端服务器向所有客户端广播加载完毕
* 加载完毕后，游戏正式开始，前后端通过stomp协议进行通信
* 游戏过程中，用户可以通过单击NPC向后端发送请求
* 后端接受到NPC被单击的请求后，向前端随机发送一个知识点，一条细胞信息或一个问题
* 知识点及细胞信息会被直接加入当场游戏的背包，问题则会点对点回传给前端进行回答
* 回答后后端将点对点通知前端回答是否正确，若正确则将回答的问题加入背包
* 背包满后游戏结束，背包信息被保存，以便收集到的知识点能够被用户查看
## 2.4 数据库设计

数据表由JPA自动生成，共有如下实体表：

* cell_info               
* choice_question          
* game_record              
* knowledge                
* pack                        
* user                       
* virus

UML图如下：

![图片](https://uploader.shimo.im/f/hTllk1AZwBvlcSK9.png!thumbnail)

## 2.5 结构设计

遵循MVC架构及SpringBoot的一般设计模式，共有如下的几个包

* config：用于配置跨域，websocket，上传文件地址映射
* controller：接受请求，交给service包中的类处理
* domain：各实体类
* repository：数据库接口，继承CrudRepository接口，运行时由SpringBoot自行注入
* security：进行访问控制，对请求进行过滤及权限判断
* service：对请求的实际处理类
* exception：异常类

总体结构如下：

![图片](https://uploader.shimo.im/f/kq4DBu3SPW6Bxn0c.png!thumbnail)

主要各包的具体结构如下

### 2.5.1 config

![图片](https://uploader.shimo.im/f/s6lb6L0jbsk6rb45.png!thumbnail)

### 2.5.2 controller

![图片](https://uploader.shimo.im/f/kyr18IPXyMYYy0WC.png!thumbnail)

### 2.5.3 service

![图片](https://uploader.shimo.im/f/IvXQMA0N1oV6wIk7.png!thumbnail)

### 2.5.4 repository

![图片](https://uploader.shimo.im/f/8sOFBNfFeAkNBMCa.png!thumbnail)

### 2.5.5 domain

![图片](https://uploader.shimo.im/f/EpAbN13wdpthFfDK.png!thumbnail)

### 2.5.6 security

![图片](https://uploader.shimo.im/f/ymZsOW7OBdbSQq6B.png!thumbnail)

# 3 接口规范

采用Restful风格的接口，Controller使用@RestController注解且在前后端间利用JSON格式进行交互，URI代表资源。

具体见接口文档：[https://shimo.im/docs/XQjVhxxT8kCrgwpP](https://shimo.im/docs/XQjVhxxT8kCrgwpP)

# 4 服务器配置

采用Docker进行部署，将项目打包为jar后上传到服务器，在tomcat镜像的基础上新建项目镜像。

使用的Dockerfile分别如下：

## 4.1 tomcat基础镜像

```plain
FROM maven:3.6.3-jdk-8
ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME
ENV TOMCAT_VERSION 8.5.54
ENV TOMCAT_TGZ_URL https://www.apache.org/dist/tomcat/tomcat-8/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz
COPY tomcat.tar.gz tomcat.tar.gz
RUN set -x \
&& tar -xvf tomcat.tar.gz --strip-components=1 \
&& rm bin/*.bat \
&& rm tomcat.tar.gz*
EXPOSE 8080
CMD ["catalina.sh", "run"]
```
## 4.2 项目镜像

```plain
FROM java:8
MAINTAINER bingo
COPY pj.jar pj.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","pj.jar"]
```
