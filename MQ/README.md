# RabbitMQ 

## 1. 同步调用 vs 异步调用

### 1.1. 同步调用

* **核心特点**：时效性强，调用方必须等待服务提供方返回结果后，才能继续向下执行。
* **存在的问题**：
* **拓展性差**：代码耦合度高。例如在“余额支付”场景中，若后续新增“短信通知”需求，必须修改原有主流程代码，无法做到即插即用。
* **性能下降**：所有关联业务串行执行，耗时为所有操作的总和，整体响应变慢。
* **级联失败**：只要链路中任意一个下游服务（如更新订单状态失败）抛出异常，整个事务便会触发回滚，系统脆弱性高。

> **经典案例（余额支付流程）**：
> 包含“余额扣减 -> 更新支付状态 -> 更新订单状态”。若采用同步调用，任何一环卡顿或报错都会导致整个支付流程阻塞或失败。
> ![alt text](image.png)

### 1.2. 异步调用

* **核心特点**：基于**事件驱动/消息通知**机制，调用方发送完消息后即可立即返回，由下游异步处理。
* **三大核心角色**：
* **消息发送者 (Publisher)**：原调用方，负责将业务事件封装为消息并投递出去。
* **消息代理 (Broker)**：MQ 服务端，负责消息的暂存、管理与路由转发。
* **消息接收者 (Consumer)**：原服务提供方，负责从 Broker 拉取或接收消息并完成本地业务。

![alt text](image-1.png)

* **优缺点对比**：
* **优势**：
* **解耦**：解除系统间的强依赖，极大提升拓展性。
* **高性能**：无需等待下游响应，缩短主流程耗时。
* **故障隔离**：下游服务宕机不影响主业务（如支付成功后，积分服务延迟不影响用户支付）。
* **流量削峰填谷**：应对秒杀等高并发场景时，通过队列缓存瞬时大流量。

* **劣势与挑战**：
* **时效性降低**：属于最终一致性，无法立即得到下游执行结果。
* **复杂度提升**：需解决消息丢失、重复消费、分布式事务等问题。
* **依赖 Broker 可靠性**：一旦 Broker 宕机，整个异步链路将受影响。
![alt text](image-2.png)

---

## 2. MQ 技术选型（Broker）

**MQ (Message Queue，消息队列)** 是存放消息的容器，也就是异步调用架构中的 Broker。目前主流的 MQ 技术选型对比见下表：

![alt text](image-3.png)

---

## 3. RabbitMQ 整体架构与核心概念

RabbitMQ 的核心组件和运行流程主要围绕以下几个关键角色展开：

* **Publisher (生产者)**：负责生产消息并发送到 RabbitMQ 的 Exchange 中。
* **Consumer (消费者)**：连接到 RabbitMQ Queue，专门监听并消费消息的服务。
* **Exchange (交换机)**：生产者不直接把消息发给队列，而是发给交换机。**交换机负责根据路由规则（Routing Key、Binding）将消息路由/分发到一个或多个 Queue 中**。
* **Queue (队列)**：真正存储消息的物理容器。消息会在队列中排队，直到被 Consumer 消费。

![alt text](image-4.png)

## 4.在Linux上安装RabbitMQ

> RabbitMQ 版本：3.8.16
> Erlang 版本：23.3.4.11
> 系统：CentOS 7

### 4.1. 安装 Erlang

#### 添加 Erlang RPM 源

```bash
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
```

#### 安装 Erlang

```bash
sudo yum install erlang-23.3.4.11-1.el7.x86_64
```

#### 检查 Erlang

```bash
erl -v
```

退出 Erlang：

```erlang
halt().
```

---

### 2. 安装 RabbitMQ

#### 添加 RabbitMQ RPM 源

```bash
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
```

#### 安装 RabbitMQ

```bash
sudo yum install rabbitmq-server-3.8.16-1.el7.noarch
```

---

### 3. 配置 RabbitMQ

如果启动时出现：

```text
epmd error for host 10: badarg (unknown POSIX error)
```

可以配置 RabbitMQ 节点名称。

#### 创建配置文件

```bash
vi /etc/rabbitmq/rabbitmq-env.conf
```

添加：

```bash
NODENAME=rabbit@localhost
```

---

### 4. 启动 RabbitMQ

```bash
rabbitmq-server start
```

---

### 5. 开启 Web 管理界面

```bash
rabbitmq-plugins enable rabbitmq_management
```

Web 管理端口：

```text
15672
```

---

### 6. 设置 RabbitMQ 开机启动

```bash
chkconfig rabbitmq-server on
```

---

### 7. 查看 RabbitMQ 是否正常运行

```bash
netstat -ntlp
```

如果看到：

```text
15672
25672
```

说明 RabbitMQ 已启动，并且 Web 管理功能已经开启。

---

### 8. 创建 RabbitMQ 管理员用户

#### 创建用户

```bash
rabbitmqctl add_user admin admin
```

#### 设置管理员权限

```bash
rabbitmqctl set_user_tags admin administrator
```

#### 查看用户

```bash
rabbitmqctl list_users
```

---

### 9. 常用 RabbitMQ 命令

#### 启动

```bash
rabbitmq-server start
```

#### 开启管理插件

```bash
rabbitmq-plugins enable rabbitmq_management
```

#### 查看用户

```bash
rabbitmqctl list_users
```

#### 查看端口

```bash
netstat -ntlp
```


