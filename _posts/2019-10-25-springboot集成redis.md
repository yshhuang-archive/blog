---
title: 关于缓存&springboot集成redis
tags: springboot redis  
key: 20191025
---

## 关于缓存
**缓存是一种保存资源副本并在下次请求时直接使用该副本的技术.**
### 为何使用缓存


## redis安装

+ 在MacOS安装redis(Homebrew方法)
    1. 安装 `brew install redis`
    2. 启动 `brew services start redis`   
    关闭 `brew services stop redis`   
    查看状态 `brew services list|grep redis`
    1. 设置密码   
    安装的时候是没有设置密码的,需要的话可以自行设置.   
    修改配置文件 /usr/local/etc/redis.conf,找到"`#requirepass foobared`"(大概在500行),去掉注释,修改密码,例如改为:`requirepass 1234`,则将密码设为1234.   
    改完配置需要重启redis才能生效   `brew services restart redis`
    1. 允许远程访问   
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

## springboot项目中操作redis(使用Spring Data Redis)
1. 在项目中添加依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```
2. `application.properties`加入redis相关配置:
```properties
# REDIS (RedisProperties)
# Redis数据库索引（默认为0）
spring.redis.database=0
# Redis服务器地址
spring.redis.host=127.0.0.1
# Redis服务器连接端口
spring.redis.port=6379  
# Redis服务器连接密码（默认为空）
spring.redis.password=
# 连接池最大连接数（使用负值表示没有限制）
spring.redis.pool.max-active=8  
# 连接池最大阻塞等待时间（使用负值表示没有限制）
spring.redis.pool.max-wait=-1  
# 连接池中的最大空闲连接
spring.redis.pool.max-idle=8  
# 连接池中的最小空闲连接
spring.redis.pool.min-idle=0  
# 连接超时时间（毫秒）
spring.redis.timeout=0

```
3. 通过注入`StringRedisTemplate`或者`RedisTemplate`来使用
```java
@RestController
public class MyController {
    @Autowired
    private StringRedisTemplate redisTemplate;

    @GetMapping("hello")
    public String hello(@RequestParam String value) {
        ValueOperations<String, String> ops = redisTemplate.opsForValue();
        ops.set("hello",value);
        String result = ops.get("hello");
        return result;
    }
}
```

## 参考资料:
1. [系统性能提升利刃，缓存技术使用的实践与思考](https://www.infoq.cn/article/ORUHqcGub_XQ4Bbp8UKy)
2. [缓存那些事](https://tech.meituan.com/2017/03/17/cache-about.html)
3. [如何优雅的设计和使用缓存？](https://juejin.im/post/5b849878e51d4538c77a974a)
