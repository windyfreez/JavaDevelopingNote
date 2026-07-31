<div align="center">

# Java 后端学习笔记

**系统整理 · 面试导向 · 持续更新**

[![Stars](https://img.shields.io/badge/Java-17-orange)](https://www.java.com/)
[![SpringBoot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen)](https://spring.io/projects/spring-boot)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)](https://www.mysql.com/)
[![Redis](https://img.shields.io/badge/Redis-7.x-red)](https://redis.io/)
[![MyBatis](https://img.shields.io/badge/MyBatis-3.x-lightgrey)](https://mybatis.org/)

*涵盖 Java 基础、MySQL、Redis、Spring Boot、组件配置、实战案例*

</div>

---

## 关于本仓库

本仓库整理了 Java 后端开发相关的学习笔记以及组件配置指南，内容覆盖核心语法、框架原理、中间件使用、项目实战与面试高频考点。

> 目标读者：准备校招/实习的学生、初中级后端开发者

---


## 目录总览


| 分类 | 说明 |
|------|------|
| [Java/]( #java-语言基础 ) | Java 语言基础核心知识 |
| [MySQL/]( #mysql-数据库 ) | MySQL 数据库核心知识 |
| [Redis/]( #redis-缓存中间件 ) | Redis 缓存中间件 |
| [Spring Boot/]( #spring-boot-框架 ) | Spring Boot 开发框架 |
| [杂项/]( #杂项-组件配置 ) | Spring Boot 常用组件集成配置 |


**推荐阅读顺序：** `Java/` → `MySQL/` → `Redis/` → `Spring Boot/` → `杂项/` 各组件


---


## 文档结构


```
JavaNote/
├── README.md                    ← 本文件（总目录）
├── Java/                        ← Java 语言基础
│   └── README.md                ← Java 核心语法与特性
├── MySQL/                       ← MySQL 数据库
│   └── README.md                ← MySQL 核心知识
├── Redis/                       ← Redis 缓存中间件
│   └── README.md                ← Redis 基础与命令
├── Spring Boot/                 ← Spring Boot 框架
│   └── README.md                ← Spring Boot 开发框架
└── 杂项/                        ← 组件配置
    ├── Logback.md               ← Logback 日志技术详解
    ├── config-AliyunOSS.md      ← 阿里云 OSS 文件上传
    ├── config-JWTInterceptor.md ← JWT 登录校验
    └── config-Swagger.md        ← Swagger / Knife4j
```


---


## Java/ 语言基础


### [README.md]( ./Java/README.md ) — Java 语言基础


> Java 基础语法、面向对象、集合框架、异常处理、多线程与 I/O 流等核心知识点。


| 章节 | 跳转 |
|------|------|
| 一、Java 基础语法 | [→ 基础语法]( ./Java/README.md#一、-java-基础语法 ) |
| 二、面向对象基础 | [→ 面向对象基础]( ./Java/README.md#二、-面向对象基础 ) |
| 三、面向对象高级特性与多态 | [→ 高级特性与多态]( ./Java/README.md#三、-面向对象高级特性与多态 ) |
| 四、泛型、集合与异常处理 | [→ 泛型、集合与异常]( ./Java/README.md#四、-泛型、集合与异常处理 ) |
| 五、输入输出文件流 (I/O) | [→ I/O 流]( ./Java/README.md#五、-输入输出文件流-io ) |
| 六、枚举与程序初始化顺序 | [→ 枚举与初始化]( ./Java/README.md#六、-枚举与程序初始化顺序 ) |
| 七、多线程与图形用户界面 (GUI) | [→ 多线程与 GUI]( ./Java/README.md#七、-多线程与图形用户界面-gui ) |


**重点子章节：**
- [封装、继承、多态定义]( ./Java/README.md#1-封装、继承、多态定义 )
- [方法重载与方法重写]( ./Java/README.md#3-方法重载是什么？规则？ )
- [访问控制四种修饰符]( ./Java/README.md#6-访问控制四种修饰符，作用范围 )
- [编译时多态与运行时多态]( ./Java/README.md#1-编译时多态？运行时多态？ )
- [集合类 (Java Collections Framework)]( ./Java/README.md#2-集合类-java-collections-framework )
- [异常处理两种方法]( ./Java/README.md#3-异常处理两种方法 )
- [线程生命周期与状态]( ./Java/README.md#3-线程的生命周期与状态 )


---


## MySQL/ 数据库


### [README.md]( ./MySQL/README.md ) — MySQL 数据库


> 多表关系、动态 SQL、事务 ACID、锁机制、索引优化与 EXPLAIN 执行计划。


| 章节 | 跳转 |
|------|------|
| 一、多表关系 | [→ 多表关系]( ./MySQL/README.md#一、多表关系 ) |
| 二、多表查询 | [→ 多表查询]( ./MySQL/README.md#二、多表查询 ) |
| 三、动态 SQL | [→ 动态 SQL]( ./MySQL/README.md#三、动态sql ) |
| 四、事务、ACID | [→ 事务 ACID]( ./MySQL/README.md#四、事务acid ) |
| 五、锁（Lock） | [→ 锁]( ./MySQL/README.md#五、锁lock ) |
| 六、索引（Index） | [→ 索引]( ./MySQL/README.md#六、索引index ) |
| 七、MySQL 优化总结（面试高频） | [→ 优化总结]( ./MySQL/README.md#七、mysql优化总结 ) |


**重点子章节：**
- [一对多 / 多对多 / 一对一]( ./MySQL/README.md#1一对多 )
- [内连接 / 外连接]( ./MySQL/README.md#1内连接 )
- [事务四大特性 & 隔离级别]( ./MySQL/README.md#1事务四大特性 )
- [行级锁（InnoDB）]( ./MySQL/README.md#4行级锁innodb )
- [最左前缀法则 & 索引失效场景]( ./MySQL/README.md#5最左前缀法则 )
- [EXPLAIN 执行计划]( ./MySQL/README.md#8explain执行计划 )


---


## Redis/ 缓存中间件


### [README.md]( ./Redis/README.md ) — Redis 指南


> Redis 五种数据类型、常用命令、Spring Data Redis 编程式调用与缓存注解。


| 章节 | 跳转 |
|------|------|
| 一、Redis 简介 | [→ 简介]( ./Redis/README.md#一、redis简介 ) |
| 二、5 种常用数据类型 | [→ 数据类型]( ./Redis/README.md#二、5种常用数据类型 ) |
| 三、Redis 常用命令 | [→ 常用命令]( ./Redis/README.md#三、redis常用命令 ) |
| 四、Spring Data Redis | [→ Spring Data Redis]( ./Redis/README.md#四、spring-data-redis ) |
| 五、Redis 常用注解 | [→ 常用注解]( ./Redis/README.md#五、redis常用注解 ) |
| Redis 开发与集成指南 | [→ 集成指南]( ./Redis/README.md#redis开发与集成指南 ) |
| Redis 实战应用 | [→ 实战应用]( ./Redis/README.md#redis实战应用 ) |


**命令速查：**
- [字符串命令]( ./Redis/README.md#1字符串 ) · [哈希命令]( ./Redis/README.md#2哈希 ) · [列表命令]( ./Redis/README.md#3列表 ) · [集合命令]( ./Redis/README.md#4集合 ) · [有序集合命令]( ./Redis/README.md#5有序集合 )


**实战重点（面试高频）：**
- [基于 Session 实现登录功能]( ./Redis/README.md#1基于session实现登录功能 )
- [基于 Redis 实现共享 Session 登录]( ./Redis/README.md#13基于redis实现共享session登录 )
- [缓存更新策略与主动更新]( ./Redis/README.md#2缓存的基本原理 )
- [缓存穿透、雪崩、击穿]( ./Redis/README.md#23缓存穿透 )
- [互斥锁解决缓存击穿]( ./Redis/README.md#3利用互斥锁解决缓存击穿 )


---


## Spring Boot/ 框架


### [README.md]( ./Spring%20Boot/README.md ) — Spring Boot 开发框架


> 三层架构、IOC/DI、AOP、统一异常处理、事务管理等核心知识点，含面试高频考点。


| 章节 | 跳转 |
|------|------|
| 一、三层架构 | [→ 三层架构]( ./Spring%20Boot/README.md#一、三层架构 ) |
| 二、分层解耦 | [→ 分层解耦]( ./Spring%20Boot/README.md#二、分层解耦 ) |
| 三、IOC 和 DI 常用注解 | [→ IOC 和 DI 常用注解]( ./Spring%20Boot/README.md#三、ioc和di常用注解 ) |
| 四、功能实现详解 | [→ 功能实现详解]( ./Spring%20Boot/README.md#四、功能实现详解 ) |
| 五、最终功能实现 | [→ 最终功能实现]( ./Spring%20Boot/README.md#五、最终功能实现 ) |
| 六、SpringBoot 高频面试知识点补充 | [→ 面试知识点补充]( ./Spring%20Boot/README.md#六、springboot高频面试知识点 ) |
| 七、Spring Schedule 定时任务 | [→ Spring Schedule]( ./Spring%20Boot/README.md#七、spring-schedule-定时任务 ) |


**重点子章节：**
- [Controller 层常用注解速记（面试）]( ./Spring%20Boot/README.md#5面试必考controller层常用注解速记 )
- [AOP 面向切面编程详解（面试高频）]( ./Spring%20Boot/README.md#aop面向切面编程详解面试高频 )
- [@Autowired 与 @Resource 的区别（必背）]( ./Spring%20Boot/README.md#面试重点autowired-与-resource-的区别必背 )
- [统一异常处理]( ./Spring%20Boot/README.md#2统一异常处理面试手写代码常考 )
- [事务管理]( ./Spring%20Boot/README.md#3事务管理 )


---


## 杂项/ 组件配置


> 面向 Spring Boot 项目的「可直接落地」配置指南，含完整代码示例与避坑说明。


### [Logback.md]( ./杂项/Logback.md ) — Logback 日志技术详解


> SLF4J + Logback 架构、日志级别、Appender 配置、异步日志、MDC 与最佳实践。


| 章节 | 跳转 |
|------|------|
| 一、Logback 简介 | [→ 简介]( ./杂项/Logback.md#一、logback-简介 ) |
| 二、依赖引入 | [→ 依赖引入]( ./杂项/Logback.md#二、依赖引入 ) |
| 三、日志级别（Level） | [→ 日志级别]( ./杂项/Logback.md#三、日志级别level ) |
| 四、核心组件详解 | [→ 核心组件]( ./杂项/Logback.md#四、核心组件详解 ) |
| 五、完整配置示例 | [→ 配置示例]( ./杂项/Logback.md#五、完整配置示例 ) |
| 六、过滤器（Filter） | [→ 过滤器]( ./杂项/Logback.md#六、过滤器filter ) |
| 七、高级特性 | [→ 高级特性]( ./杂项/Logback.md#七、高级特性 ) |
| 八、最佳实践 | [→ 最佳实践]( ./杂项/Logback.md#八、最佳实践 ) |
| 九、常见问题排查 | [→ 问题排查]( ./杂项/Logback.md#九、常见问题排查 ) |
| 十、附录：与 Log4j2 的对比 | [→ Log4j2 对比]( ./杂项/Logback.md#十、附录与-log4j2-的对比 ) |


**重点子章节：**
- [Logger 的继承体系]( ./杂项/Logback.md#logger-的继承体系 )
- [常用 Appender 类型]( ./杂项/Logback.md#常用-appender-类型 )
- [Pattern 常用占位符]( ./杂项/Logback.md#pattern-常用占位符 )
- [异步日志（AsyncAppender）]( ./杂项/Logback.md#71-异步日志asyncappender )
- [MDC（Mapped Diagnostic Context）]( ./杂项/Logback.md#72-mdcmapped-diagnostic-context )


---


### [config-AliyunOSS.md]( ./杂项/config-AliyunOSS.md ) — 阿里云 OSS 文件上传


| 章节 | 跳转 |
|------|------|
| 1. 引入依赖 | [→ 依赖]( ./杂项/config-AliyunOSS.md#1-引入依赖 ) |
| 2. 配置 OSS 参数 | [→ 配置]( ./杂项/config-AliyunOSS.md#2-配置-oss-参数 ) |
| 3. OSS 工具类封装 | [→ 工具类]( ./杂项/config-AliyunOSS.md#3-oss-工具类封装 ) |
| 4. 文件上传接口 | [→ 上传接口]( ./杂项/config-AliyunOSS.md#4-文件上传接口 ) |


---


### [config-JWTInterceptor.md]( ./杂项/config-JWTInterceptor.md ) — JWT 登录校验


| 章节 | 跳转 |
|------|------|
| 1. 令牌技术 | [→ JWT 令牌]( ./杂项/config-JWTInterceptor.md#1令牌技术 ) |
| 2. Filter 过滤器 | [→ Filter]( ./杂项/config-JWTInterceptor.md#2filter过滤器 ) |
| 3. Interceptor 拦截器 | [→ Interceptor]( ./杂项/config-JWTInterceptor.md#3interceptor拦截器 ) |
| 4. ThreadLocal 传递当前登录用户 ID | [→ ThreadLocal]( ./杂项/config-JWTInterceptor.md#4threadlocal传递当前登录用户id实战高频 ) |
| 5. 登录功能开发完整流程速记 | [→ 完整流程]( ./杂项/config-JWTInterceptor.md#5登录功能开发完整流程速记 ) |


**面试重点：**
- [JWT 优缺点]( ./杂项/config-JWTInterceptor.md#4jwt优缺点面试常问 )
- [Filter 生命周期]( ./杂项/config-JWTInterceptor.md#4filter生命周期面试常问 )
- [Filter 与 Interceptor 区别（必背）]( ./杂项/config-JWTInterceptor.md#4filter-与-interceptor-区别面试必背 )


---


### [config-Swagger.md]( ./杂项/config-Swagger.md ) — Swagger (Knife4j)


| 章节 | 跳转 |
|------|------|
| 1. 引入依赖 | [→ 依赖]( ./杂项/config-Swagger.md#1-引入依赖 ) |
| 2. 配置 Knife4j | [→ 配置]( ./杂项/config-Swagger.md#2-配置-knife4j ) |
| 3. 设置静态资源映射 | [→ 静态资源]( ./杂项/config-Swagger.md#3-设置静态资源映射 ) |


> 访问地址：`http://localhost:8080/doc.html`


---


## 说明


- 本仓库为个人学习笔记，主要记录 Java 后端开发相关知识
- 笔记包含理论知识、代码示例和面试考点
- 持续更新中，欢迎补充和指正


---

<div align="center">

**如果这份笔记对你有帮助，欢迎 Star ⭐ 支持！**

有任何问题或建议，欢迎提 Issue 交流。

</div>
