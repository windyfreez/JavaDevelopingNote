# Java学习笔记

## 前言

欢迎来到我的 Java 后端学习仓库！这里记录了我从 2025 年初开始系统学习 Java 后端技术栈以来的全部笔记沉淀：从语言基础到框架实战、从数据库底层原理到中间件应用、从面试八股到算法手撕，再到可以直接落地的 Linux 部署与组件配置指南。每一篇笔记都经过*"**学习 → 实践 → 面试复盘**"*的打磨，既是我的复习资料，也希望成为你学习路上的参考。

这个仓库始终坚持一个原则：**不只记录"怎么用"，更讲清"为什么"**。面试考的是底层原理，开发考的是避坑能力，因此笔记中大量采用"**面试题 + 参考答案 + 陷阱速记**"的形式，帮助大家把知识讲出来、写出来、用起来。

### 关于内容
本仓库整理了本人 **Java 后端开发**相关的学习笔记、面试八股、算法手撕以及组件配置指南。
本人正在求职**java后端 + AI应用开发职位**，该仓库包含本人所有复习资料，其中重要的内容包括：
- **java语言基础**：基本语法、面向对象、集合、异常处理、多线程、JVM虚拟机等基础知识；
- **MySQL数据库**：多表关系、事务ACID、锁、索引、MVCC、SQL慢查询、MyBatis框架等相关知识
- **Spring Boot框架**：三层架构、IOC/DI/AOP、统一异常处理，声明式事务管理、Spring Scheduled定时任务、RESTful接口设计思想等核心知识
- **Redis缓存中间件**：缓存相关知识、Redis常用数据结构/基本操作、Spring Redis基本用法、缓存穿透/击穿/雪崩、实际高并发应用场景详解等核心知识
- **RabbitMQ消息队列**：交换机、消息队列、消费者/生产者等相关概念、Java操作RabbitMQ相关方法等
- **Linux相关知识**：Linux常用命令、CentOS7安装方法、VMWare虚拟机安装、Linux安装JDK/MySQL/nginx/Redis/RabbitMQ、Linux部署Spring Boot项目等相关方法
- 其他：包括**算法题思路（本人的理解）与答案**、**Spring Boot相关组件配置方法**


笔记实时动态更新，博主将不断总结更新Java后端开发学习的相关知识，**版权归作者所有，请勿商用**

**推荐阅读顺序：** `Java/` → `MySQL/` → `Spring Boot/` → `Redis/` → `RabbitMQ/` → `Linux/` → `杂项/`
该顺序与作者的学习顺序相同，我认为这对于知识连贯性是相对合理的 


### 仓库亮点
- **🎯 面试八股清单 + 参考答案对照**：每篇笔记开头都有按星级标注的常见面试题清单，正文给出可直接口述的参考答案，复习效率翻倍；
- **⚠️ 高频陷阱速记**：把最容易踩坑、最容易答错的知识点单独提炼成"陷阱速记"，面试/上线前快速过一遍即可查漏补缺；
- **🔍 底层原理深入**：不满足于会用，深入讲解 MySQL MVCC/锁/索引、JVM 内存与 GC、并发编程、Spring 自动装配等底层机制，能够连贯口述原理；
- **💻 完整代码示例**：所有知识点都配有可直接运行的代码/命令示例，动手实践才能真正内化；
- **🚀 可直接落地的部署与配置指南**：Linux 环境搭建、Spring Boot 项目部署、Logback 日志、阿里云 OSS、JWT 登录校验、Swagger 等"拿来即用"的实战内容；
- **🧮 算法手撕**：收录 Leetcode 题目的个人理解与解题思路，面试算法环节不慌。

### 学习建议
1. **先看清单，再学正文**：每篇笔记开头的八股清单就是学习地图，先自测哪些会、哪些不会，再针对性学习；
2. **口述代替背诵**：面试考察的是表达能力，学完一个知识点尝试自己讲一遍（对着镜子或录音），讲不通就是没懂；
3. **动手实践**：命令、代码、配置一定要自己敲一遍，尤其是 Linux 部署和中间件安装，纸上得来终觉浅；
4. **建立知识网络**：Java 基础 → 数据库 → 框架 → 中间件 → 部署是层层递进的关系，按照推荐的阅读顺序学习更连贯；
5. **面试前快速复盘**：重点复习各篇的"陷阱速记"和"面试重点"，用最少的时间覆盖最高频的考点。

### 关于本人
本人是一个**西安电子科技大学的计算机科学与技术专业本科在读学生**，自**2025年初**开始学习Java后端开发相关技术栈，**2026年6月**正式开始求职后端开发的实习岗位，毕业时间为**2028年夏季**，目前本人的博客不仅包含学习JavaWeb技术栈的相关知识点总结，同时包括了个人在学习过程中自己开发维护的项目以及平时Vibe Coding的一些小工具，如若您对我的项目感兴趣，可以为我的项目点个Star，这将成为我维护这个博客的最大动力

链接：
- SwapU云市集 [https://github.com/windyfreez/SwapU](https://github.com/windyfreez/SwapU) 一个面向C2C的物品交易服务平台（作者力推！）

- ProjectPliot：[https://github.com/windyfreez/ProjectPilot](https://github.com/windyfreez/ProjectPilot) Coding工作台，涵盖项目管理、Token用量统计、TodoList记录等功能

---


## 目录总览


| 分类 | 说明 |
|------|------|
| [Java/]( #java-语言基础 ) | Java 语言基础核心知识 |
| [MySQL/]( #mysql-数据库 ) | MySQL 数据库核心知识 |
| [Redis/]( #redis-缓存中间件 ) | Redis 缓存中间件 |
| [Spring Boot/]( #spring-boot-框架 ) | Spring Boot 开发框架 |
| [Linux/]( #linux-运维部署 ) | Linux 运维部署：常用命令、VMWare/CentOS 安装、环境搭建与项目部署 |
| [杂项/]( #杂项-组件配置 ) | Spring Boot 常用组件集成配置 |





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
├── Linux/                       ← Linux 运维部署
│   └── README.md                ← 常用命令 / 环境安装 / 项目部署
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
| 八、JVM | [→ JVM]( ./Java/README.md#八、jvm ) |
| 九、Java 8 新特性 | [→ Java 8]( ./Java/README.md#九、java-8 ) |
| 十、反射与注解 | [→ 反射与注解]( ./Java/README.md#十、反射与注解 ) |


**重点子章节：**
- [封装、继承、多态定义]( ./Java/README.md#1-封装、继承、多态定义 )
- [方法重载与方法重写]( ./Java/README.md#3-方法重载是什么？规则？ )
- [访问控制四种修饰符]( ./Java/README.md#6-访问控制四种修饰符，作用范围 )
- [编译时多态与运行时多态]( ./Java/README.md#1-编译时多态？运行时多态？ )
- [集合类 (Java Collections Framework)]( ./Java/README.md#2-集合类-java-collections-framework )
- [异常处理两种方法]( ./Java/README.md#3-异常处理两种方法 )
- [线程生命周期与状态]( ./Java/README.md#77-线程的生命周期与状态 )
- [内部类详解（四种内部类）]( ./Java/README.md#8-内部类详解 )
- [synchronized 锁升级（偏向锁/轻量级/重量级）]( ./Java/README.md#31-synchronized-锁升级偏向锁轻量级锁重量级锁 )
- [sleep、wait、join 区别]( ./Java/README.md#78-sleepwaitjoin-的区别与线程通信 )
- [BigDecimal 精度问题]( ./Java/README.md#15-bigdecimal-精度问题金额计算必用 )


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
| 八、数据库引擎对比 | [→ 引擎对比]( ./MySQL/README.md#八、数据库引擎对比 ) |
| 九、线上高阶优化方案 | [→ 优化方案]( ./MySQL/README.md#九、线上高阶优化方案 ) |
| 十、MySQL 架构与 SQL 执行流程 | [→ 架构与执行流程]( ./MySQL/README.md#十、mysql-架构与-sql-执行流程 ) |
| 十一、InnoDB 存储引擎架构 | [→ InnoDB 架构]( ./MySQL/README.md#十一、innodb-存储引擎架构buffer-pool ) |
| 十二、主从复制 | [→ 主从复制]( ./MySQL/README.md#十二、主从复制binlog-三种格式 ) |
| 十三、分库分表 | [→ 分库分表]( ./MySQL/README.md#十三、分库分表核心思想与问题 ) |
| 十四、字段类型与主键设计 | [→ 字段类型]( ./MySQL/README.md#十四、常用字段类型与主键设计面试高频 ) |


**重点子章节：**
- [一对多 / 多对多 / 一对一]( ./MySQL/README.md#1一对多 )
- [内连接 / 外连接]( ./MySQL/README.md#1内连接 )
- [事务四大特性 & 隔离级别]( ./MySQL/README.md#1事务四大特性 )
- [行级锁（InnoDB）]( ./MySQL/README.md#4行级锁innodb )
- [最左前缀法则 & 索引失效场景]( ./MySQL/README.md#5最左前缀法则 )
- [EXPLAIN 执行计划]( ./MySQL/README.md#8explain执行计划 )
- [binlog 与 redo log 两阶段提交]( ./MySQL/README.md#5binlog-与-redo-log-两阶段提交2pc )
- [索引下推 ICP / MRR]( ./MySQL/README.md#6索引下推-icp-与-mrr-优化 )
- [主从复制原理]( ./MySQL/README.md#十二、主从复制binlog-三种格式 )


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


**命令速查：**
- [字符串命令]( ./Redis/README.md#1字符串 ) · [哈希命令]( ./Redis/README.md#2哈希 ) · [列表命令]( ./Redis/README.md#3列表 ) · [集合命令]( ./Redis/README.md#4集合 ) · [有序集合命令]( ./Redis/README.md#5有序集合 )


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
| 六、Spring Schedule 定时任务 | [→ Spring Schedule]( ./Spring%20Boot/README.md#六、spring-schedule-定时任务 ) |
| 七、Spring AOP 面向切面编程 | [→ Spring AOP]( ./Spring%20Boot/README.md#七、spring-aop面向切面编程 ) |
| 八、配置绑定与配置文件 | [→ 配置绑定]( ./Spring%20Boot/README.md#八、配置绑定与配置文件 ) |
| 九、统一异常处理 | [→ 统一异常处理]( ./Spring%20Boot/README.md#九、统一异常处理restcontrolleradvice ) |
| 十、声明式事务管理 | [→ 事务管理]( ./Spring%20Boot/README.md#十、声明式事务管理transactional ) |
| 十一、Spring MVC 请求处理流程与 RESTful | [→ MVC 流程]( ./Spring%20Boot/README.md#十一、spring-mvc-请求处理流程与-restful ) |
| 十二、跨域处理（CORS） | [→ 跨域处理]( ./Spring%20Boot/README.md#十二、跨域处理cors ) |
| 十三、异步任务（@Async） | [→ 异步任务]( ./Spring%20Boot/README.md#十三、异步任务async ) |
| 十四、条件注解与 Spring Boot 启动流程 | [→ 条件注解与启动流程]( ./Spring%20Boot/README.md#十四、条件注解与-spring-boot-启动流程 ) |


**重点子章节：**
- [Controller 层常用注解速记（面试）]( ./Spring%20Boot/README.md#5面试必考controller层常用注解速记 )
- [AOP 面向切面编程详解（面试高频）]( ./Spring%20Boot/README.md#aop面向切面编程详解面试高频 )
- [@Autowired 与 @Resource 的区别（必背）]( ./Spring%20Boot/README.md#面试重点autowired-与-resource-的区别必背 )
- [统一异常处理]( ./Spring%20Boot/README.md#2-统一异常处理面试手写代码常考 )
- [事务管理（传播行为与隔离级别）]( ./Spring%20Boot/README.md#3-事务管理传播行为与隔离级别 )


---


## Linux/ 运维部署


### [README.md]( ./Linux/README.md ) — Linux 运维部署指南


> Linux 常用命令、VMWare 虚拟机与 CentOS7 安装、JDK/MySQL/nginx/Redis/RabbitMQ 环境搭建、Spring Boot 项目部署。


| 章节 | 跳转 |
|------|------|
| 一、Linux 系统基础与常用命令 | [→ 常用命令]( ./Linux/README.md#一、linux-系统基础与常用命令 ) |
| 二、VMWare 虚拟机安装 | [→ VMWare]( ./Linux/README.md#二、vmware-虚拟机安装 ) |
| 三、CentOS7 安装与初始化配置 | [→ CentOS7]( ./Linux/README.md#三、centos7-安装与初始化配置 ) |
| 四、Linux 安装 JDK | [→ JDK]( ./Linux/README.md#四、linux-安装-jdk ) |
| 五、Linux 安装 MySQL | [→ MySQL]( ./Linux/README.md#五、linux-安装-mysql ) |
| 六、Linux 安装 nginx | [→ nginx]( ./Linux/README.md#六、linux-安装-nginx ) |
| 七、Linux 安装 Redis | [→ Redis]( ./Linux/README.md#七、linux-安装-redis ) |
| 八、Linux 安装 RabbitMQ | [→ RabbitMQ]( ./Linux/README.md#八、linux-安装-rabbitmq ) |
| 九、Linux 部署 Spring Boot 项目 | [→ 项目部署]( ./Linux/README.md#九、linux-部署-spring-boot-项目 ) |
| 十、常见问题速查（避坑清单） | [→ 问题速查]( ./Linux/README.md#十、常见问题速查避坑清单 ) |


**重点速查：**
- [文件与目录操作命令]( ./Linux/README.md#2-文件与目录操作 )
- [权限与用户管理]( ./Linux/README.md#4-权限与用户管理 )
- [端口占用排查（部署排错必会）]( ./Linux/README.md#6-网络与防火墙 )
- [Spring Boot 部署（systemd 方式）]( ./Linux/README.md#4-运行方式二systemd-服务生产推荐开机自启-崩溃自动重启 )


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
