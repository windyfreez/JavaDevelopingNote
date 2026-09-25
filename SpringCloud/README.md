# 📚 SpringCloud微服务架构

## 前言

- Spring Cloud 作为 Java 生态中最具代表性的微服务一站式解决方案，将复杂的分布式系统开发转化为模块化的组件组合。学习它不仅是掌握一套具体的框架，更是建立现代企业级分布式架构思维的关键。

- 微服务治理与分布式架构经验是当前中大型互联网企业对 Java 工程师的核心硬性要求之一。

- Spring Cloud 是连接传统 Java 开发与云原生技术（如 Docker、Kubernetes）的重要桥梁，能大幅降低向云端容器化迁移的门槛。

- 在处理分布式锁、网络延迟、数据一致性等问题的过程中，显著提升复杂线上故障的定位与调优能力。

## 一、分布式基础
### 1.单体架构（ALL IN ONE）
所有功能模块都在一个项目里
- 优点：开发部署简单容易
- 缺点：无法应对高并发
![alt text](image.png)


### 2.集群架构
将单体架构创建多个副本，部署在不同的服务器当中，并且将同一个域名指向一个部署公网IP的服务器（网关），网关处理用户请求，并通过负载均衡策略将请求均摊给服务器集群。
- 优点：解决高并发问题
- 缺点1：模块化升级：订单功能经常升级：v1.0、v2.0
- 缺点2：多语言团队：C++直播模块、Java业务模块
![alt text](image-1.png)


### 3.分布式架构
一个大型应用被拆分成很多小应用分布部署在各个机器中（数据库同样也可以拆分出多个微服务模块，对应微服务与数据库模块相联），拆分的每一个模块就叫做**微服务**，微服务的特点是：**独立部署、数据隔离、语言无关**
- 需要处理的问题：单点故障问题：一个微服务只在一台服务器上部署，该服务器故障整个微服务宕机
- 注册中心：维持多个列表数据，用来保存各个微服务都在哪些服务器当中，注册中心可进行服务发现（该服务在哪些服务器）、服务注册（服务上/下线的消息、心跳机制、负载均衡）
- 配置中心；统一管理所有配置：负责保存配置、配置修改、推送配置变更
- 服务熔断：解决服务雪崩的问题、**快速失败机制**，大部分请求失败需要提前拦截更多请求到达服务器
![alt text](image-2.png)

![alt text](image-3.png)

**技术点：**

- **微服务**：SpringBoot
- **注册中心/配置中心**：Spring Cloud Alibaba Nacos
- **网关**：Spring Cloud Gateway
- **远程调用**：Spring Cloud OpenFeign
- **服务熔断**：Spring Cloud Alibaba Sentinel
- **分布式事务（数据库集群数据一致性）**：Spring Cloud Alibaba Seata
![alt text](image-4.png)



## Nacos（注册中心、配置中心）

### 注册中心

- 功能：**服务注册、服务发现**

- 服务注册具体流程：
  1. 启动微服务：SpringBoot微服务web项目启动
  2. 引入服务发现依赖：`spring-cloud-starter-alibaba-nacos-discovery`
  
  ```xml
  <dependency>
  	<groupId>com.alibaba.cloud</groupId>
  	<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
  </dependency>
  ```
  
  3. 配置Nacos地址：`spring.cloud.nacos.server-addr=127.0.0.1:8848`
  
  ```pro
  # 服务名
  spring.application.name=service-product
  # 端口号
  server.port=8001
  
  # 服务地址
  spring.cloud.nacos.server-addr=127.0.0.1:8848
  ```
  
  4. 查看注册中心效果：访问 http://localhost:8848/nacos
  
  ![1790059283023](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1790059283023.png)
  
  5. 集群模式启动测试：单机情况下通过改变端口模拟微服务集群
  
- 服务发现具体流程：
  1. 开启服务发现功能：`@EnableDiscoveryClient`
  2. 测试服务发现API：`DiscoveryClient`
  
  ```java
  @SpringBootTest
  public class DiscoveryTest {
  
      @Autowired
      DiscoveryClient discoveryClient;
  
      @Test
      void discoveryClientTest() {
          for (String service : discoveryClient.getServices()) {
              System.out.println("services = "+service);
              //获取ip+port
              List<ServiceInstance> instances = discoveryClient.getInstances(service);
              for (ServiceInstance instance : instances) {
                  System.out.println("ip: "+instance.getHost()+"; port: "+instance.getPort());
              }
          }
      }
  }
  ```
  
  3. 测试服务发现API：`NacosServiceDiscovery`
  
  ```java
  @SpringBootTest
  public class DiscoveryTest {
  
      @Autowired
      NacosServiceDiscovery nacosServiceDiscovery;
  
      @Test
      void nacosServiceDiscoveryTest() throws NacosException {
          for (String service : nacosServiceDiscovery.getServices()) {
              System.out.println("services = "+service);
              //获取ip+port
              List<ServiceInstance> instances = nacosServiceDiscovery.getInstances(service);
              for (ServiceInstance instance : instances) {
                  System.out.println("ip: "+instance.getHost()+"; port: "+instance.getPort());
              }
          }
      }
  }
  
  ```
  
  ### 远程调用
  
  **下单场景：**
  
  ![1790072084524](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1790072084524.png)

![1790072134869](C:\Users\zhaos\AppData\Roaming\Typora\typora-user-images\1790072134869.png)



