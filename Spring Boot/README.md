# 📚 Springboot开发框架
## 前言

### 学习说明
SpringBoot基于Spring，核心是简化配置、自动装配；
### 面试重点
IOC/DI、Bean生命周期、AOP、事务、配置绑定、定时任务、全局异常、REST接口；不要死记代码，要能口述原理、说出坑点。
### 以下是本文整理出的常见八股问题清单，对照复习效率翻倍

> 🎯【⭐⭐⭐⭐⭐ 必须掌握】
1. 什么是三层架构？每一层职责？为什么要分层？禁止跨层调用指什么？
2. `@RestController` 和 `@Controller` 的区别？
3. `@RequestParam`、`@PathVariable`、`@RequestBody` 三者区别与使用场景？
4. 什么是IOC控制反转、DI依赖注入？两者关系？什么是Bean？
5. `@SpringBootApplication` 由哪三个注解组成？各自作用？SpringBoot自动装配原理？
6. SpringBoot Starter启动器的作用是什么？
7. `@Component`、`@Service`、`@Controller`、`@Repository` 区别？
8. `@Configuration` 和 `@Component` 的区别？`@Bean`注解用途？
9. Bean的作用域有哪些？默认singleton单例特点？`@Lazy`懒加载作用？
10. Spring依赖注入三种方式？官方推荐哪一种？各自优缺点？
11. `@Autowired` 和 `@Resource` 的区别？
12. 同一个类型多个Bean，如何解决注入冲突？`@Primary`、`@Qualifier`用法？
13. 什么是循环依赖？Spring如何解决循环依赖？哪种注入方式无法解决循环依赖？
14. Spring Bean完整生命周期？
15. Spring Schedule定时任务，`@EnableScheduling`、`@Scheduled`使用要求？
16. `fixedRate` 和 `fixedDelay` 的区别？任务耗时超过间隔会发生什么？
17. Cron表达式语法，日和周字段注意什么？
18. Spring Schedule默认线程池有什么坑？集群部署定时任务会出现什么问题？生产怎么解决？
19. 什么是AOP？底层原理？JDK动态代理与CGLIB代理区别？
20. AOP五大核心概念：连接点、通知、切入点、切面、目标对象？
21. AOP五种通知类型以及执行顺序？`@Around`环绕通知用法？
22. 配置绑定：`@Value` 和 `@ConfigurationProperties` 的区别？
23. `@RestControllerAdvice + @ExceptionHandler` 全局异常处理，有哪些拦截边界限制？
24. `@Transactional`声明式事务，默认什么异常才回滚？`rollbackFor`作用？
25. `@Transactional`事务失效常见场景？
26. Filter过滤器和Interceptor拦截器区别？完整执行顺序？

> 🎯【⭐⭐⭐⭐ 建议掌握】
27. Lombok常用注解`@Data`、`@NoArgsConstructor`、`@AllArgsConstructor`注意事项？
28. 切入点execution表达式语法规则？
29. 什么是RESTful接口设计思想？
30. SpringBoot条件注解`@ConditionalOnClass`、`@ConditionalOnMissingBean`作用？
31. 定时任务实际业务使用场景？
32. AOP适合哪些业务场景？有什么优势？
33. yml与properties配置文件对比？
34. 什么场景推荐使用XXL‑Job替代Spring自带@Scheduled？

---
### 📌 SpringBoot高频面试陷阱速记
1. `@RestControllerAdvice` 拦截不到Filter、定时任务、异步线程抛出的异常。
2. `@Transactional` 同类内直接调用方法事务会失效，必须走代理对象。
3. Spring Schedule默认单线程，耗时任务会阻塞其余定时任务；集群多实例会重复执行。
4. `@RequestBody` 不能接收GET的query参数，只能读取请求体JSON。
5. 构造器注入会触发循环依赖报错，Spring无法解决构造器的循环依赖。
6. Cron表达式中`日`和`周`不能同时指定具体值，其中一个必须写`?`。
7. `@Data`搭配`@AllArgsConstructor`会丢失默认无参构造，框架反射实例化会报错。
8. 切面类、定时任务类不加`@Component`交给IOC容器，注解完全不生效。

## 一、三层架构
### 1.三层架构：
Controller层即控制层，负责接收前端发送的请求，完成请求参数接收、调用业务层、向前端响应数据；
Service层即业务逻辑层，处理项目当中具体的业务逻辑，业务校验、业务组装、业务规则都写在此层；
dao层也叫mapper层（MyBatis场景）即数据访问层，专门负责和数据库交互，实现数据的增加、删除、修改、查询操作。

> 三层分工明确，各层之间通过接口进行调用，不允许跨层直接访问，Controller不能直接操作DAO，DAO不能写业务逻辑。

![alt text](image-9.png)

### 2.**为什么要对代码进行拆分？**
> 🎯【面试题】项目为什么要做三层架构分层？
> 参考答案：核心遵循单一职责原则，每一层只做自己分内的事情。分层之后代码模块职责清晰，代码复用性提升；后期业务迭代维护的时候，修改某一层逻辑不会波及其他层，便于单元测试，也方便团队多人协同开发。如果全部写在一个类里面，代码臃肿，后期改bug和迭代会非常痛苦。

### 3.Lombok注解
在Java实体类当中可使用Lombok注解简化实体类模板代码，减少getter、setter、构造方法手写工作量。
- `@Data`：组合注解，自动生成私有化成员变量的getter、setter、toString、equals、hashCode方法，极大简化实体类代码；
- `@NoArgsConstructor`：生成无参构造方法；很多框架反射实例化对象依赖无参构造；
- `@AllArgsConstructor`：生成全参构造方法；
> 注意：使用`@Data`不代表可以完全不写构造；如果同时写`@AllArgsConstructor`，默认无参构造会消失，建议实体类同时加上`@NoArgsConstructor`。

### 4.两个Controller层当中的注解
```java
@RestController//表示当前类是一个请求处理类
public class HelloController {

    @RequestMapping("/hello")
    public String hello(String name){
        System.out.println("name :"+name);
        return "Hello "+name+ "~";
    }

}
```

`@RestController`是组合注解，等价于`@Controller + @ResponseBody`；如果只使用`@Controller`，返回字符串会被解析成视图页面路径，做页面跳转；加上`@RestController`才会把返回值直接作为JSON或者文本数据返回给前端接口。

`@RequestMapping("/请求地址")`用来定义接口请求路径，默认支持GET、POST、PUT、DELETE全部请求方式。
`@RequestBody`注解用于接收前端传过来的JSON格式请求体，把JSON数据直接封装到Java实体对象；**注意：@RequestBody只能读取请求体，不能接收url拼接的query参数**。

### 5.面试必考：Controller层常用注解速记

| 注解 | 作用 | 面试考点 |
|---|---|---|
| **@GetMapping** | 等价于 `@RequestMapping(method = RequestMethod.GET)` | 专门处理GET查询请求 |
| **@PostMapping** | 等价于 `@RequestMapping(method = RequestMethod.POST)` | 专门处理POST新增请求 |
| **@PutMapping** | 等价于 `@RequestMapping(method = RequestMethod.PUT)` | 专门处理PUT更新请求 |
| **@DeleteMapping** | 等价于 `@RequestMapping(method = RequestMethod.DELETE)` | 专门处理DELETE删除请求 |
| **@RequestParam** | 接收URL当中`?name=xxx`这种query查询参数 | 可设置 `required = false` 非必传，`defaultValue` 设置参数默认值 |
| **@PathVariable** | 接收URL路径上的变量，例如 `/user/{id}` | RESTful风格接口必备 |

> 🎯【面试题】@RequestParam、@PathVariable、@RequestBody三者区别？
> 参考答案：
> @RequestParam获取url问号后面的query参数；
> @PathVariable获取url路径占位符`{}`里面的参数；
> @RequestBody读取http请求体中的JSON数据，post/put常用，get请求没有请求体，不能使用@RequestBody。

**记忆口诀**：`@RestController`返回数据，`@Controller`跳转页面；`@GetMapping`查、`@PostMapping`增、`@PutMapping`改、`@DeleteMapping`删。

> 补充知识点：
> 1. 普通表单`application/x‑www‑form‑urlencoded`，不能使用`@RequestBody`，直接用实体接收或者`@RequestParam`；
> 2. `@RestControllerAdvice` + `@ExceptionHandler`可以统一处理Controller抛出的异常，后面统一异常处理章节讲解。

---

## 二、分层解耦
三层架构实现业务需求的时候，controller调用service，service调用dao；原始写法直接new实现类完成对象创建，代码耦合严重。
```java
private UserDao userDao = new UserDaoImpl();
//后续代码直接调用UserDao类当中的方法和return值
```
这种硬编码new对象，一旦要替换实现类，就需要修改全部调用处代码，违反开闭原则。

### 1.Spring IOC和DI
> 🎯【面试题】什么是IOC、DI？二者之间的关系？
> 参考答案：IOC即控制反转，把对象创建、对象管理的控制权，从我们业务代码手里反转交给Spring IOC容器。我们代码不再自己new对象。
> DI即依赖注入，IOC容器运行时，自动把业务类所依赖的Bean对象注入进去。
> IOC是思想，DI是实现这个思想的手段；IOC负责把对象交给容器管理，DI负责把对象装配到需要的地方。
> Bean对象：存放在IOC容器当中，由Spring创建、管理的对象统称为Bean。

然而在实际项目开发中，常常遵循“高内聚、低耦合”的软件设计原则，不再手动new对象，依靠IOC与DI完成对象管理与装配。

### 2.分层解耦的思路
1. 将项目中需要使用的业务类交给IOC容器管理，完成控制反转；
2. 业务类运行时所需要依赖的对象，由IOC容器自动注入，完成依赖注入。
实际开发中，DAO层、Service层实现类打上对应注解交给IOC容器，Controller依赖Service、Service依赖DAO，全部通过DI注入。

### 3.SpringBoot启动核心注解（面试常问）
> 🎯【面试题】@SpringBootApplication由哪几个注解组成？分别作用是什么？SpringBoot自动配置原理简单说一下？
> 参考答案：`@SpringBootApplication`是组合注解，包含三个核心注解：
> 1. `@Configuration`：标记当前类是配置类；
> 2. `@EnableAutoConfiguration`：开启SpringBoot自动配置，根据classpath下面引入的jar包，自动装配组件，比如引入tomcat‑starter就自动配置web容器，引入mybatis‑starter自动装配MyBatis相关组件；
> 3. `@ComponentScan`：开启组件扫描，**默认扫描主启动类所在包以及它所有子包下面的类**。
> 注意：如果业务代码放在主启动类包之外，默认扫描不到，需要手动指定@ComponentScan扫描路径。

一句话记住自动配置原理：启动类`@SpringBootApplication` → 组件扫描把带`@Component`及其衍生注解的类加载进IOC容器 → `@EnableAutoConfiguration`读取META‑INF/spring/org.springframework.boot.autoconfigure.imports配置，根据classpath的jar条件自动装配组件。

> 🎯【面试题】SpringBoot的starter是什么？
> 参考答案：starter是SpringBoot的启动器，把依赖jar、自动配置类封装在一起，我们只需要引入一个starter依赖，就自动导入相关依赖并且开启自动装配，不用自己写大量配置。

---

## 三、IOC和DI常用注解

### 常用IOC注解（把类交给Spring容器管理）
1.`@Component`：通用组件注解，将实现类交给IOC容器管理；
**IOC衍生注解，语义化，只是代码可读性区别，功能和@Component完全一样：**
- `@Controller`：标注在控制层类上；
- `@Service`：标注在业务层类上；
- `@Repository`：标注在数据访问层类上；和MyBatis整合的时候mapper接口不需要该注解，使用`@Mapper`。

> 🎯【面试题】@Component、@Service、@Controller、@Repository四者区别？
> 参考答案：底层功能完全等价，都是把类注册进IOC容器；只是为了区分代码分层，增加代码可读性；@Repository在部分持久层场景会做异常转译。

### DI注解：实现依赖注入
`@Autowired`：自动在IOC容器当中查找Bean对象，完成依赖注入。

### 补充：其他IOC/DI高频注解（面试常问）

| 注解 | 作用 |
|---|---|
| **@Configuration** | 标识配置类，通常配合`@Bean`使用 |
| **@Bean** | 写在方法上，把方法返回的对象注册为IOC容器Bean，**主要用于第三方类，我们不能修改源码的类** |
| **@Scope** | 指定Bean的作用域，默认**singleton**单例；常用还有 **prototype**多例，每次获取都会创建全新对象 |
| **@Primary** | 同一个类型存在多个Bean的时候，标记优先选择该Bean |
| **@Qualifier("beanName")** | 和`@Autowired`配合，按Bean名称精确指定注入对象 |
| **@Resource(name="beanName")** | JSR‑250规范提供注解，优先按名称注入，找不到再按类型注入 |

> 🎯【面试题】@Configuration和@Component的区别？
> 参考答案：@Configuration修饰的类会被CGLIB动态代理，类内部调用本类`@Bean`方法，返回的是容器里面的单例Bean；@Component不会做代理，内部调用@Bean方法会多次new新对象。

**Bean作用域速记**：
- **singleton**：单例（默认），整个Spring容器全局只有一个实例，容器初始化的时候就创建（默认饿汉；可以设置懒加载`@Lazy`）；
- **prototype**：多例，每次从容器获取该Bean，都会创建全新对象；
- **request**：web环境，一次http请求生命周期；
- **session**：web环境，一次用户会话session生命周期；
- **application**：ServletContext全局生命周期。

> 🎯【面试题】@Lazy懒加载注解作用？
> 参考答案：单例Bean默认容器启动就实例化；打上`@Lazy`之后，第一次使用该Bean的时候才创建实例。

---

## 四、功能实现详解

### DI基于@Autowired进行依赖注入的三种常见方式
> 🎯【面试题】Spring依赖注入有几种方式？官方推荐哪一种？各自优缺点？
> 参考答案：一共三种注入方式：属性注入、setter注入、构造器注入；Spring官方推荐**构造器注入**。

①属性注入(字段注入)
```java
//方式一：属性注入
    @Autowired
    private UserService userService;
```
优点：代码非常简洁，开发速度快；
缺点：依赖关系隐藏，外部无法直观看到类需要哪些依赖；容易循环依赖；单元测试难以mock对象。

②构造函数注入（官方推荐）
```java
//方式二：构造器注入，如果类只有一个构造函数，@Autowired注解可以省略
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }
```
优点：所有依赖写在构造方法参数，依赖关系一目了然；可以把成员变量定义为`final`保证对象初始化完成后依赖不可修改；避免循环依赖问题；方便单元测试mock。
缺点：代码模板较多；依赖数量很多的时候，构造函数参数列表会臃肿，提示类职责过多，需要拆分类。

> 注意：如果当前类仅有一个构造函数，Spring可以自动推断注入，`@Autowired`可以省略。

③setter注入
```java
//方式三：setter注入
    private UserService userService;
    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    }
```
优点：依赖关系清晰；适合依赖可选的场景，可以灵活修改依赖对象。
缺点：需要手写setter方法，代码量增加；对象实例化完成之后还可以修改依赖，无法保证依赖不为null。

### DI详解
`@Autowired`注解**默认按照类型byType完成注入**；
如果IOC容器里面同一个接口存在多个实现类Bean，按照类型查找会找到多个，抛出`NoUniqueBeanDefinitionException`异常。

解决同一个类型多个Bean冲突的三种手段：
* `@Primary`：标记某一个Bean，提升优先级，优先注入这个；
* `@Qualifier("bean名称")`：配合`@Autowired`，通过Bean的名字精确指定注入对象；
* `@Resource(name="bean名称")`：JDK标准注解，优先按照名字注入。

### 面试重点：@Autowired 与 @Resource 的区别（必背）

| 对比项 | @Autowired | @Resource |
|---|---|---|
| **来源** | Spring框架提供 | JDK JSR‑250标准提供 |
| **注入规则** | 默认**按类型（byType）**注入 | 默认**按名称（byName）**注入，名称找不到回退按类型 |
| **指定名称** | 需要配合`@Qualifier("xxx")` | 直接写`name="xxx"`属性 |
| **required属性** | 支持`required = false`，找不到bean不报错 | 不支持required属性 |

**记忆口诀**：`Autowired`先找类型，`Resource`先找名字。

> 🎯【面试题】什么是循环依赖？Spring怎么解决循环依赖？哪些情况无法解决？
> 参考答案：A依赖B，B又依赖A，形成循环依赖。Spring单例场景下，通过三级缓存解决构造器以外的循环依赖；**构造器注入的循环依赖Spring无法解决，直接抛出异常**。这也是官方不推荐大量使用构造器注入的一个现实原因。

> 🎯【面试题】Bean的完整生命周期？
> 参考答案：实例化 → 依赖注入 → 初始化（InitializingBean、@PostConstruct、init‑method） → 业务使用 → 销毁（DisposableBean、@PreDestroy、destroy‑method）。

---

## 五、功能实现：分层代码示例
> 说明：这一组示例模拟读取txt文件数据，展示三层之间调用；实际项目替换为MyBatis访问数据库。

### 1.Controller层（补充，原文缺失controller示例）
```java
@RestController
@RequestMapping("/user")
public class UserController {
    //构造器注入，官方推荐
    private final UserService userService;
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/list")
    public List<User> list(){
        return userService.findAll();
    }
}
```

### 2.Service层实现类（ServiceImpl）
```java
@Service //交给IOC容器管理
public class UserServiceImpl implements UserService {

    private final UserDao userDao;
    //构造注入
    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public List<User> findAll() {
        //1.调用DAO，获取数据
        List<String> lines = userDao.findAll();

        //2.解析用户信息，封装为User对象->list集合
        List<User> userList = lines.stream().map(line -> {
            //将txt中的每一行数据解析并封装到对象当中
            String[] parts = line.split(",");
            Integer id = Integer.parseInt(parts[0]);
            String username = parts[1];
            String password = parts[2];
            String name = parts[3];
            Integer age = Integer.parseInt(parts[4]);
            LocalDateTime updateTime = LocalDateTime.parse(parts[5], DateTimeFormatter.ofPattern("yyyy‑MM‑dd HH:mm:ss"));
            return new User(id+200, username, password, name, age, updateTime);
        }).toList();

        return userList;
    }
}
```

### 3.Dao层数据处理层
```java
@Repository
public class UserDaoImpl implements UserDao {

    @Override
    public List<String> findAll() {
        //1.加载并读取user.txt来获取用户数据
        InputStream in = this.getClass().getClassLoader().getResourceAsStream("user.txt");
        ArrayList<String> lines = IoUtil.readLines(in, StandardCharsets.UTF_8, new ArrayList<>());
        return lines;
    }
}
```

---

## 六、Spring Schedule 定时任务

### 1.@EnableScheduling 注解开启定时任务
在 Spring Boot 主启动类上添加 `@EnableScheduling` 注解，启用定时任务功能：

```java
@SpringBootApplication
@EnableScheduling // 开启定时任务
public class SpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

### 2.@Scheduled 注解的使用
> 🎯【面试题】@Scheduled标记的方法有什么要求？
> 参考答案：方法返回值必须是void，方法不能带入参；所在类必须交给Spring IOC容器管理（@Component/@Service）。

```java
@Component
public class ScheduledTask {

    // 方式一：使用 Cron 表达式（业务最常用）
    @Scheduled(cron = "0 0 2 * * ?") // 每天凌晨2点执行
    public void dailyTask() {
        System.out.println("每日定时任务执行：" + LocalDateTime.now());
    }

    // 方式二：fixedRate（固定速率执行）
    @Scheduled(fixedRate = 5000) // 每隔5秒执行一次
    public void fixedRateTask() {
        System.out.println("固定速率任务：" + LocalDateTime.now());
    }

    // 方式三：fixedDelay（固定延迟执行）
    @Scheduled(fixedDelay = 5000) // 上次执行完成后隔5秒执行
    public void fixedDelayTask() {
        System.out.println("固定延迟任务：" + LocalDateTime.now());
    }

    // 方式四：initialDelay（项目启动后初始延迟）
    @Scheduled(initialDelay = 3000, fixedDelay = 5000) // 启动后延迟3秒开始，之后每隔5秒执行
    public void initialDelayTask() {
        System.out.println("初始延迟任务：" + LocalDateTime.now());
    }
}
```

### 3.Cron 表达式详解
Cron 表达式是一个字符串，由 6 或 7 个空格分隔的字段组成，格式为：
`秒 分 时 日 月 周 [年]`

| 字段 | 允许值 | 允许的特殊字符 |
|---|---|---|
| 秒 | 0‑59 | `,` `-` `*` `/` |
| 分 | 0‑59 | `,` `-` `*` `/` |
| 时 | 0‑23 | `,` `-` `*` `/` |
| 日 | 1‑31 | `,` `-` `*` `/` `?` `L` `W` |
| 月 | 1‑12 或 JAN‑DEC | `,` `-` `*` `/` |
| 周 | 1‑7 或 SUN‑SAT | `,` `-` `*` `/` `?` `L` `#` |

**特殊字符说明：**

| 字符 | 说明 |
|---|---|
| `*` | 匹配该字段全部取值 |
| `?` | 不指定值，**日和周不能同时指定，二者必须其中一个写?消除冲突** |
| `-` | 指定时间范围，例如 1‑5 |
| `,` | 指定多个离散值，例如 1,3,5 |
| `/` | 增量，`0/5`代表每5个单位触发一次 |
| `L` | 代表最后，日字段写L表示当月最后一天 |
| `W` | 最近工作日 |
| `#` | 第几个星期几 |

**常用 Cron 表达式示例：**
```text
0 0 2 * * ?         // 每天凌晨2点执行
0 30 2 * * ?        // 每天凌晨2:30执行
0 0 2 * * MON‑FRI   // 周一到周五凌晨2点执行
0 0 2 1 * ?         // 每月1号凌晨2点执行
0 0 2 * * SUN       // 每周日凌晨2点执行
0 0/5 * * * ?       // 每5分钟执行一次
0 0 0 * * ?         // 每天零点执行
```

### 4.fixedRate 与 fixedDelay 的区别（面试常问）
> 🎯【面试题】fixedRate 和 fixedDelay 区别，如果任务执行时间超过间隔会发生什么？
> 参考答案：
> fixedRate：以上一次**任务开始的时间点**作为基准计算下一次触发时间；如果任务耗时超过间隔时间，任务会直接并发执行。
> fixedDelay：以上一次**任务执行结束的时间点**作为基准；必须等上一轮任务执行完毕，才开始计时等待下一轮，不会并发。

| 对比项 | fixedRate | fixedDelay |
|---|---|---|
| **执行时机** | 以上次**开始时间**为基准计算下次执行时间 | 以上次**结束时间**为基准计算下次执行时间 |
| **执行间隔** | 固定间隔，不受任务耗时影响，会并发 | 受任务执行时长影响，不会并发执行 |
| **适用场景** | 任务耗时很短，要求严格固定频率 | 任务耗时不可控，不允许任务并发 |

示例：
- `fixedRate = 5000`：任务执行耗时2秒，第1次0s开始，第2次5s开始，第3次10s开始；
- `fixedDelay = 5000`：任务执行耗时2秒，第1次0s开始，第2次7s开始（0+2+5），第3次14s开始。

### 5.定时任务配置（可选）
在 `application.yml` 中配置定时任务线程池：
```yaml
spring:
  task:
    scheduling:
      pool:
        size: 10  # 定时任务线程池大小，**默认size=1，单线程**
```

> 🎯【面试题】Spring Schedule默认线程池有什么坑？
> 参考答案：默认只有一个线程。如果有多个定时任务，某一个任务长时间阻塞，其他所有定时任务全部会被卡住，得不到执行。生产环境一定要手动设置pool.size。

### 6.实际应用场景

| 场景 | 说明 |
|---|---|
| **数据统计** | 每天凌晨统计前一天的业务数据 |
| **数据同步** | 定期从第三方外部系统同步业务数据 |
| **缓存刷新** | 定时刷新Redis热点缓存 |
| **日志清理** | 定时清理服务器过期日志文件 |
| **订单处理** | 定时扫描处理超时未支付订单，自动关闭订单 |

### 7.面试重点：@Scheduled 注解的注意事项（必背）
1. **方法要求**：被 `@Scheduled` 注解标记的方法必须是**无返回值（void）、无参数**的方法；
2. **类要求**：该类必须被 Spring IOC容器管理（`@Component`、`@Service`），否则注解不会生效；
3. **线程池**：默认单线程，长耗时任务会阻塞其他定时任务，生产环境配置`spring.task.scheduling.pool.size`；
4. **Cron表达式**：日和周不能同时写具体数值，必须其中一个使用`?`，避免逻辑冲突；
5. **时区问题**：默认取服务器系统时区；可以通过`zone = "Asia/Shanghai"`显式指定时区；
6. **集群环境坑点**：多实例部署的时候，定时任务会每台机器都执行，导致任务重复执行；生产集群需要使用分布式锁（Redis锁、xxl‑job）保证任务只执行一次。

**记忆口诀**：定时任务要开启，方法无参无返回；fixedRate看开始，fixedDelay看结束；默认单线程，长任务要注意；集群要防重复执行。

> 补充：Spring自带Scheduled只适合简单单机定时任务；大型项目推荐使用分布式调度框架XXL‑JOB。

---

## 七、Spring AOP面向切面编程
### 1.应用场景
> 🎯【面试题】什么是AOP？底层实现原理？有哪些业务场景？优势是什么？
> 参考答案：AOP面向切面编程，在**不修改目标类源代码的前提下，对目标方法做增强**。底层依靠**动态代理**实现：JDK动态代理（目标实现接口）、CGLIB代理（目标没有实现接口）。
> 典型业务场景：方法耗时统计、统一日志记录、权限校验、事务管理、读写分离数据源切换。
> 优势：业务代码无侵入，公共的重复逻辑抽离到切面类，减少代码重复，便于统一维护。

- 面向切面编程,面向方面编程,面向特定方法编程
- 在不修改目标方法源代码的前提下,Spring AOP对请求链路上的目标方法进行**运行耗时的统计**
- **底层原理:**Spring AOP底层通过**动态代理机制**实现对目标方法的编程,动态代理是目前面向切面编程最主流的实现技术
- **常见应用场景:**给目标方法添加**事务管理**,给目标方法添加**访问权限控制**,对目标方法进行**读写分离**
- **优势:**减少重复代码,代码无侵入,提高开发效率,维护方便

### 2.核心概念
> 🎯【面试题】AOP五大核心概念讲一下？
> 参考答案：
> 1. **连接点JoinPoint**：所有可以被AOP拦截增强的方法；
> 2. **通知Advice**：要插入的公共增强逻辑，就是切面里面写的方法；分为前置、后置、异常、最终、环绕通知；
> 3. **切入点PointCut**：表达式，用来匹配哪些连接点要执行通知；
> 4. **切面Aspect**：切面类，通知Advice + 切入点PointCut的组合；
> 5. **目标对象Target**：被增强的原始对象。

- **连接点JoinPoint**:可以被AOP控制的方法(暗含方法执行时的相关信息)
- **通知Advice**:指那些重复的逻辑,也就是共性功能(最终体现为一个方法)
- **切入点PointCut**:匹配连接点的条件,通知仅会在切入点方法执行时被应用
- **切面Aspect**:描述通知与切入点的对应关系(通知+切入点)
- **目标对象Target**:通知所应用的对象

### 3.使用方法
以AOP统计目标方法运行耗时举例。
1. **导入AOP starter依赖**
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring‑boot‑starter‑aop</artifactId>
    </dependency>
```
2. 编写切面类，写增强逻辑；
3. 使用注解标记切面、通知、切入点。

**常用通知注解：**
* `@Aspect`：标记当前类是切面类；
* `@Before`：前置通知，目标方法执行之前执行；
* `@After`：最终通知，目标方法执行完毕，不管是否异常都执行；
* `@AfterReturning`：返回通知，目标方法正常返回才执行；
* `@AfterThrowing`：异常通知，目标方法抛出异常的时候才执行；
* `@Around`：环绕通知，包裹目标方法，可以在目标方法前后都写逻辑，**功能最强大，性能统计、权限校验常用**。

```java
@Slf4j
@Aspect//标识当前是一个AOP切面类
@Component//切面类必须交给IOC容器管理，否则切面不会生效
public class BookAdvice {

    //统计目标方法运行的耗时时间，execution切入点表达式匹配controller包下所有类所有方法
    @Around("execution(* com.itsean.campus_second_hand.controller..*.*(..))")
    public Object method(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        //1.目标方法运行前记录开始时间
        long start = System.nanoTime();
        //2.执行目标方法，proceed()就是执行原始目标方法
        Object result = proceedingJoinPoint.proceed();
        //3.目标方法执行完成记录结束时间
        long end = System.nanoTime();
        log.info("目标方法{}运行时间：{}纳秒", proceedingJoinPoint.toShortString(),(end‑start));
        return result;
    }
}
```
>注意:AOP的切面类需要加上`@Component`注解交给Spring容器管理，否则AOP不会生效。

> 🎯【面试题】五种通知执行顺序？
> 参考答案：环绕前置 → @Before前置通知 → 执行目标方法 → @AfterReturning返回通知 → @After最终通知 → 环绕后置；
> 如果目标抛出异常：环绕前置 → @Before → 目标异常 → @AfterThrowing异常通知 → @After最终通知；环绕通知proceed不捕获异常不会执行环绕后面代码。

> 🎯【面试题】切入点execution表达式语法？
> `execution(返回值 包.类.方法(参数))`；`*`通配任意返回值、任意类、任意方法；`..`代表任意参数。


# 📋 SpringBoot 高频八股总复习清单
