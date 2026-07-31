# Logback 日志技术详解

## 一、Logback 简介

### 1.1 什么是 Logback
Logback 是由 Log4j 创始人设计的另一个开源日志组件，是 **SLF4J（Simple Logging Facade for Java）** 的原生实现。它被认为是 Log4j 的继任者，在性能和功能上都有显著提升。

### 1.2 Logback 的核心优势
- **执行速度快**：初始化、运行时内存占用和日志输出速度都优于 Log4j
- **配置灵活**：支持 XML、Groovy 配置方式，且支持自动重载配置
- **自动清除旧日志**：通过 `TimeBasedRollingPolicy` 可以自动删除归档日志
- **自动压缩归档日志**：自动将历史日志压缩为 `.gz` 或 `.zip` 格式
- **谨慎模式（Prudent Mode）**：可在多 JVM 环境下安全写入同一日志文件
- **丰富的过滤能力**：提供比 Log4j 更强大的日志过滤机制

### 1.3 Logback 的三层架构
```
┌─────────────────────────────────────┐
│         logback-core（核心）         │  ← 为前两者提供基础功能
├─────────────────────────────────────┤
│      logback-classic（经典实现）      │  ← 兼容 SLF4J，核心模块
├─────────────────────────────────────┤
│      logback-access（访问模块）       │  ← 与 Servlet 容器集成，记录 HTTP 访问日志
└─────────────────────────────────────┘
```

---

## 二、依赖引入

### 2.1 Maven 依赖
Spring Boot 项目默认已集成，无需额外引入。普通 Maven 项目需要手动添加：

```xml
<!-- SLF4J API -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.9</version>
</dependency>

<!-- Logback 核心 + 经典实现 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.4.14</version>
</dependency>

<!-- Logback 核心（classic 会传递依赖，可选） -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
    <version>1.4.14</version>
</dependency>
```

### 2.2 依赖关系说明
- `logback-classic` 依赖 `logback-core` 和 `slf4j-api`
- 项目只需显式引入 `logback-classic` 即可，其余依赖会自动传递

---

## 三、日志级别（Level）

### 3.1 级别定义（由低到高）

| 级别 | 说明 | 使用场景 |
|------|------|----------|
| `TRACE` | 追踪，最详细的运行轨迹 | 方法入参、执行路径追踪（极少使用） |
| `DEBUG` | 调试信息 | 开发阶段调试，查看变量值、流程分支 |
| `INFO` | 一般信息 | 记录程序运行的关键节点、业务操作 |
| `WARN` | 警告信息 | 潜在问题、非预期但可恢复的情况 |
| `ERROR` | 错误信息 | 程序异常、功能不可用、需要人工介入 |

### 3.2 级别使用示例
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class UserService {
    private static final Logger log = LoggerFactory.getLogger(UserService.class);

    public void queryUserById(Long id) {
        log.trace("进入 queryUserById 方法，参数 id={}", id);
        log.debug("开始查询用户信息，id={}", id);

        User user = userMapper.selectById(id);
        log.info("查询用户信息成功，id={}", id);

        if (user == null) {
            log.warn("用户不存在，id={}", id);
        }

        try {
            // 业务逻辑
        } catch (Exception e) {
            log.error("查询用户信息异常，id={}", id, e);
        }
    }
}
```

### 3.3 占位符语法
日志中若要输出变量，需使用占位符 `{}`，后面用逗号分隔变量：
```java
// ✅ 正确：使用占位符，日志框架会在需要输出时才拼接字符串
log.info("查询用户信息成功，id={}，name={}", id, name);

// ❌ 错误：字符串拼接，无论是否输出都会执行拼接操作
log.info("查询用户信息成功，id=" + id + "，name=" + name);
```

> **性能提示**：使用占位符 `{}` 可以避免不必要的字符串拼接开销，当日志级别未达到时，参数不会被解析和拼接。

### 3.4 级别控制规则
**大于等于配置级别的日志才会输出。**

例如配置 `root level="INFO"`，则只输出 `INFO`、`WARN`、`ERROR`，不输出 `DEBUG` 和 `TRACE`。

```xml
<!-- 控制台将会输出 info、warn、error 日志 -->
<root level="INFO">
    <appender-ref ref="STDOUT" />
    <appender-ref ref="FILE" />
</root>
```

---

## 四、核心组件详解

### 4.1 Logger（日志记录器）
Logger 是日志记录的入口，负责按级别分发日志事件。

#### Logger 的继承体系
- `ROOT Logger`：根记录器，所有 Logger 的祖先，名称为 `""`（空字符串）
- 包/类 Logger：通过 `LoggerFactory.getLogger(Xxx.class)` 获取，名称通常是全限定类名
- **继承规则**：子 Logger 会继承父 Logger 的级别和 Appender，除非显式覆盖

```
root(级别=INFO, Appender=CONSOLE)
  └── com(继承 root)
       └── example(继承 root)
            ├── controller(级别=WARN, 继承 CONSOLE)
            │    └── UserController(继承 controller)
            └── service(级别=DEBUG, 继承 CONSOLE)
                 └── UserService(继承 service)
```

#### Logger 配置示例
```xml
<!-- root logger：全局默认配置 -->
<root level="INFO">
    <appender-ref ref="CONSOLE" />
</root>

<!-- 指定包/类的日志级别 -->
<logger name="com.example.controller" level="WARN"/>
<logger name="com.example.service" level="DEBUG"/>

<!-- 指定某个类完全独立的配置（additivity=false 不继承 root 的 appender） -->
<logger name="com.example.service.PayService" level="DEBUG" additivity="false">
    <appender-ref ref="PAY_FILE" />
</logger>
```

> `additivity="false"`：阻止日志向父 Logger 传递，避免重复输出。

---

### 4.2 Appender（输出目的地）
Appender 定义日志输出的目标位置，一个 Logger 可以绑定多个 Appender。

#### 常用 Appender 类型

| Appender | 类名 | 说明 |
|----------|------|------|
| 控制台输出 | `ConsoleAppender` | 输出到 System.out 或 System.err |
| 文件输出 | `FileAppender` | 输出到指定文件 |
| 滚动文件 | `RollingFileAppender` | 按规则滚动生成新日志文件 |
| 数据库 | `DBAppender` | 输出到数据库 |
| 远程 Socket | `SocketAppender` | 通过网络发送到远程服务器 |
| SMTP 邮件 | `SMTPAppender` | 发送日志邮件 |

#### ConsoleAppender 配置
```xml
<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <target>System.out</target>
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
    </encoder>
</appender>
```

#### FileAppender 配置
```xml
<appender name="FILE" class="ch.qos.logback.core.FileAppender">
    <file>logs/app.log</file>
    <append>true</append>  <!-- true 追加，false 覆盖 -->
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```

#### RollingFileAppender 配置（推荐生产环境使用）
```xml
<appender name="ROLLING_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <!-- 当前日志文件路径 -->
    <file>logs/app.log</file>

    <!-- 滚动策略：按时间 + 文件大小 -->
    <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
        <!-- 归档日志文件名格式：每天一个文件夹，%i 用于区分超出大小后的文件序号 -->
        <fileNamePattern>logs/archive/%d{yyyy-MM-dd}/app-%i.log.gz</fileNamePattern>

        <!-- 单个归档文件最大 100MB -->
        <maxFileSize>100MB</maxFileSize>

        <!-- 保留 30 天的归档日志 -->
        <maxHistory>30</maxHistory>

        <!-- 所有归档日志总大小不超过 10GB -->
        <totalSizeCap>10GB</totalSizeCap>
    </rollingPolicy>

    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
    </encoder>
</appender>
```

---

### 4.3 Encoder / Layout（格式化）
Encoder 负责将日志事件转换为字节数组，Layout 负责格式化文本。

#### Pattern 常用占位符

| 占位符 | 说明 | 示例 |
|--------|------|------|
| `%d{pattern}` | 日期时间 | `%d{yyyy-MM-dd HH:mm:ss.SSS}` |
| `%thread` | 线程名 | `main`、`http-nio-8080-exec-1` |
| `%-5level` | 日志级别（左对齐5位） | `INFO `、`ERROR` |
| `%logger{len}` | Logger 名称（缩写） | `%logger{36}` |
| `%class` | 输出日志的类名 | `com.example.UserService` |
| `%method` | 输出日志的方法名 | `queryUser` |
| `%line` | 行号 | `42` |
| `%msg` / `%m` | 日志消息 | 用户自定义内容 |
| `%n` | 换行符 | |
| `%ex` / `%exception` | 异常堆栈 | |
| `%X{key}` | MDC 中的变量 | `%X{traceId}` |

> **性能注意**：使用 `%class`、`%method`、`%line` 会显著降低日志性能，因为需要通过运行时反射获取，生产环境建议避免使用。

#### 彩色控制台输出（Spring Boot 风格）
```xml
<appender name="COLOR_STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %highlight(%-5level) %cyan(%logger{50}) - %msg%n</pattern>
    </encoder>
</appender>
```

---

## 五、完整配置示例

### 5.1 Spring Boot 项目（logback-spring.xml）
Spring Boot 推荐命名为 `logback-spring.xml`，以便使用 Spring 扩展特性（如 `<springProfile>`）。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="60 seconds" debug="false">

    <!-- 定义变量 -->
    <property name="LOG_PATH" value="logs"/>
    <property name="LOG_PATTERN" value="%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n"/>

    <!-- 控制台输出 -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>${LOG_PATTERN}</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- INFO 级别文件 -->
    <appender name="INFO_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/info.log</file>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/archive/info-%d{yyyy-MM-dd}.log.gz</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${LOG_PATTERN}</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- ERROR 级别文件 -->
    <appender name="ERROR_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_PATH}/error.log</file>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/archive/error-%d{yyyy-MM-dd}.log.gz</fileNamePattern>
            <maxHistory>60</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>${LOG_PATTERN}</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- 根 Logger -->
    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="INFO_FILE"/>
        <appender-ref ref="ERROR_FILE"/>
    </root>

    <!-- 第三方框架日志级别调整 -->
    <logger name="org.springframework" level="WARN"/>
    <logger name="org.apache.ibatis" level="WARN"/>
    <logger name="com.zaxxer.hikari" level="WARN"/>

</configuration>
```

### 5.2 多环境配置（Spring Boot）
```xml
<!-- logback-spring.xml -->
<configuration>
    <springProfile name="dev">
        <root level="DEBUG">
            <appender-ref ref="CONSOLE"/>
        </root>
    </springProfile>

    <springProfile name="prod">
        <root level="INFO">
            <appender-ref ref="ROLLING_FILE"/>
        </root>
    </springProfile>
</configuration>
```

---

## 六、过滤器（Filter）

过滤器用于精细控制哪些日志事件可以被 Appender 处理。

### 6.1 级别过滤器（LevelFilter）
精确匹配某一级别：
```xml
<filter class="ch.qos.logback.classic.filter.LevelFilter">
    <level>ERROR</level>
    <onMatch>ACCEPT</onMatch>      <!-- 匹配时接受 -->
    <onMismatch>DENY</onMismatch>  <!-- 不匹配时拒绝 -->
</filter>
```

### 6.2 阈值过滤器（ThresholdFilter）
大于等于某级别的日志通过：
```xml
<filter class="ch.qos.logback.classic.filter.ThresholdFilter">
    <level>WARN</level>  <!-- WARN 及以上级别通过 -->
</filter>
```

### 6.3 评估过滤器（EvaluatorFilter）
基于 Groovy/Janino 表达式过滤：
```xml
<filter class="ch.qos.logback.core.filter.EvaluatorFilter">
    <evaluator class="ch.qos.logback.classic.boo.JaninoEventEvaluator">
        <expression>return message.contains("支付宝");</expression>
    </evaluator>
    <onMatch>ACCEPT</onMatch>
    <onMismatch>DENY</onMismatch>
</filter>
```

---

## 七、高级特性

### 7.1 异步日志（AsyncAppender）
将日志事件放入阻塞队列，由后台线程批量写入，减少 I/O 阻塞对业务线程的影响。

```xml
<appender name="ASYNC" class="ch.qos.logback.classic.AsyncAppender">
    <!-- 队列最大容量，默认 256 -->
    <queueSize>512</queueSize>
    <!-- 队列满时的丢弃阈值（%），默认 20 -->
    <discardingThreshold>20</discardingThreshold>
    <!-- 队列满时是否丢弃 INFO 及以下级别的日志，默认 true -->
    <neverBlock>false</neverBlock>
    <!-- 关联实际的 Appender -->
    <appender-ref ref="ROLLING_FILE"/>
</appender>
```

### 7.2 MDC（Mapped Diagnostic Context）
用于在同一线程的多个日志中共享上下文信息，如 `traceId`、`userId`：

```java
import org.slf4j.MDC;

// 在拦截器中设置
MDC.put("traceId", UUID.randomUUID().toString());
MDC.put("userId", userId);

try {
    // 业务执行...
    log.info("处理请求...");  // 日志中自动包含 traceId 和 userId
} finally {
    MDC.clear();  // 必须清理，防止线程复用时数据污染
}
```

```xml
<!-- 配置中使用 %X{key} 输出 -->
<pattern>%d{HH:mm:ss.SSS} [%thread] [%X{traceId}] [%X{userId}] %-5level %logger - %msg%n</pattern>
```

### 7.3 配置自动重载
```xml
<!-- scan=true 开启自动扫描，scanPeriod 设置扫描间隔 -->
<configuration scan="true" scanPeriod="60 seconds">
    ...
</configuration>
```

---

## 八、最佳实践

### 8.1 使用规范
1. **统一使用 SLF4J API**：`import org.slf4j.Logger; import org.slf4j.LoggerFactory;`
2. **Logger 定义为 `private static final`**：避免序列化问题，减少对象创建
3. **使用占位符 `{}`**：避免不必要的字符串拼接开销
4. **避免在日志中调用方法**：`log.info("data={}", getComplexData())` 中的 `getComplexData()` 总会执行
5. **正确传递异常**：`log.error("操作失败", e)`，而不是 `log.error("操作失败" + e.getMessage())`

### 8.2 级别选择建议
- `TRACE`：方法入参、出参、循环内部细节
- `DEBUG`：外部服务调用、缓存命中/未命中、分支判断
- `INFO`：业务流程节点、用户操作、定时任务开始/结束
- `WARN`：参数校验失败、重复提交、降级处理、资源不足但可恢复
- `ERROR`：异常捕获、数据不一致、核心功能不可用

### 8.3 生产环境建议
- 关闭 `scan="true"`，或设置较长的 `scanPeriod`，避免频繁检查文件变更
- 使用 `RollingFileAppender` 并设置合理的 `maxHistory` 和 `totalSizeCap`
- 异步日志适用于高并发写入场景，但要注意 `queueSize` 和内存占用
- 敏感信息（密码、Token、身份证号）**禁止**输出到日志
- 日志目录应与应用部署目录分离，避免磁盘占满影响应用运行

---

## 九、常见问题排查

| 问题 | 可能原因 | 解决方法 |
|------|----------|----------|
| 日志不输出 | 日志级别设置过高 | 检查 `root` 和 Logger 的 `level` |
| 日志重复 | `additivity="true"` 导致向父 Logger 传递 | 设置 `additivity="false"` |
| 中文乱码 | 未设置 `<charset>UTF-8</charset>` | 在 encoder 中添加 charset |
| 日志文件不滚动 | `fileNamePattern` 格式错误 | 检查日期格式和 `%i` 的使用 |
| 控制台无颜色 | 未使用 `highlight()` 或终端不支持 | 使用支持 ANSI 的终端或 Intellij IDEA |

---

## 十、附录：与 Log4j2 的对比

| 特性 | Logback | Log4j2 |
|------|---------|--------|
| 性能 | 优秀 | 更优（异步基于 LMAX Disruptor） |
| 配置方式 | XML、Groovy | XML、JSON、YAML |
| 自动重载 | ✅ | ✅ |
| 插件扩展 | 一般 | 丰富 |
| Spring Boot 默认 | ✅（1.x/2.x） | ✅（3.x 开始） |
| 垃圾回收 | 较少 | 更少（异步 Logger 几乎零 GC） |

> Spring Boot 3.x 默认使用 Log4j2 的底层实现，但 SLF4J API 保持不变，业务代码无需改动。