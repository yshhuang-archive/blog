---
title: springboot集成redis
tags: springboot redis  
key: 20191025
published: false
---

## 关于缓存


## redis安装

+ 在MacOS安装redis(Homebrew方法)
    1. 安装 `brew install redis`
    2. 启动 `brew services start redis`   
    关闭 `brew services stop redis`   
    查看状态 `brew services list|grep redis`
    3. 设置密码   
    安装的时候是没有设置密码的,需要的话可以自行设置.   
    修改配置文件 /usr/local/etc/redis.conf,找到"`#requirepass foobared`"(大概在500行),去掉注释,修改密码,例如改为:`requirepass 1234`,则将密码设为1234.   
    改完配置需要重启redis才能生效   `brew services restart redis`
    4. 允许远程访问   
    除需要开放服务器端口号6379，还需将配置文件中的bind 127.0.0.1注释掉
+ centos7安装redis(yum方法)
  1. 安装EPEL仓库   
  centos7使用yum安装Redis时，可能会有安装源的问题出现。安装epel源，CentOS默认的安装源在官方的centos.org上，而redis在第三方的yum源里，因此无法安装。   
  ```shell
    yum install epel-release
    yum update
  ```
  2. 安装redis  `yum install redis`
  3. 启动   `service redis start`
   关闭 `service redis stop`
  4. 配置文件:`/etc/redis.conf`,修改方法和MacOS一样.

