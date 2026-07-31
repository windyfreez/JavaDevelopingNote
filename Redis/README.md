# Redis指南

## 一、Redis简介
基于内存的key-value结构数据库
- 基于内存存储，读写性能高
- 适合存储热点数据（热点商品、咨询、新闻）
- 企业应用广泛

## 二、5种常用数据类型
### 1.数据类型
Redis存储的是key-value结构的数据，其中key是字符串类型，value有五种常用的数据类型：
- 字符串 `string`
- 哈希 `hash`
- 列表 `list`
- 集合 `set`
- 有序集合 `sorted set/zset`
### 2.数据类型的特点
- 字符串 (`string`)：普通字符串，Redis 中最简单的数据类型
- 哈希 (`hash`)：也叫散列，类似于 Java 中的 HashMap 结构
- 列表 (`list`)：按照插入顺序排序，可以有重复元素，类似于 Java 中的 `LinkedList`
- 集合 (`set`)：无序集合，没有重复元素，类似于 Java 中的 HashSet
- 有序集合 (`sorted set /zset`)：集合中每个元素关联一个分数（score），根据分数升序排序，没有重复元素


## 三、Redis常用命令
### 1.字符串
- `SET key value` 设置指定 key 的值
- `GET key` 获取指定 key 的值
- `SETEX key seconds value` 设置指定 key 的值，并将 key 的过期时间设为 seconds 秒
- `SETNX key value` 只有在 key 不存在时设置 key 的值

### 2.哈希
- `HSET key field value` 将哈希表 key 中的字段 field 的值设为 value
- `HGET key field` 获取存储在哈希表中指定字段的值
- `HDEL key field` 删除存储在哈希表中的指定字段
- `HKEYS key` 获取哈希表中所有字段
- `HVALS key` 获取哈希表中所有值

### 3.列表
- `LPUSH key value1 （value2 ...）` 将一个或多个值插入到列表头部
- `LRANGE key start stop` 获取列表指定范围内的元素
- `RPOP key` 移除并获取列表最后一个元素
- `LLEN key` 获取列表长度

### 4.集合
- `SADD key member1 （member2）` 向集合添加一个或多个成员
- `SMEMBERS key` 返回集合中的所有成员
- `SCARD key` 获取集合的成员数
- `SINTER key1 （key2）` 返回给定所有集合的交集
- `SUNION key1 （key2）` 返回所有给定集合的并集
- `SREM key member1 （member2）` 删除集合中一个或多个成员

### 5.有序集合
- `ZADD key score1 member1 （score2 member2）` 向有序集合添加一个或多个成员
- `ZRANGE key start stop （WITHSCORES）` 通过索引区间返回有序集合中指定区间内的成员
- `ZINCRBY key increment member` 有序集合中对指定成员的分数加上增量 increment
- `ZREM key member （member ...）` 移除有序集合中的一个或多个成员

## 四、Spring Data Redis
```java
@SpringBootTest
public class SpringDataRedisTest {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    public void testRedisTemplate(){
        System.out.println(redisTemplate);
        ValueOperations valueOperations = redisTemplate.opsForValue();
        HashOperations hashOperations = redisTemplate.opsForHash();
        ListOperations listOperations = redisTemplate.opsForList();
        SetOperations setOperations = redisTemplate.opsForSet();
        ZSetOperations zSetOperations = redisTemplate.opsForZSet();
    }

    @Test
    public void testString(){
        //set get setex setnx
        redisTemplate.opsForValue().set("name","小明");
        String city = (String) redisTemplate.opsForValue().get("name");
        System.out.println(city);
        redisTemplate.opsForValue().set("code","1234",3, TimeUnit.MINUTES);
        redisTemplate.opsForValue().setIfAbsent("lock","1");
        redisTemplate.opsForValue().setIfAbsent("lock","2");

    }

    /**
     * 操作哈希类型的数据
     */
    @Test
    public void testHash(){
        //hset hget hdel hkeys hvals
        HashOperations hashOperations = redisTemplate.opsForHash();

        hashOperations.put("100","name","Tom");
        hashOperations.put("100","age","20");

        String name = (String) hashOperations.get("100","name");
        System.out.println(name);

        Set keys = hashOperations.keys("100");
        System.out.println(keys);

        List values = hashOperations.values("100");
        System.out.println(values);

        hashOperations.delete("100","age");
    }

    /**
     * 操作列表类型的数据
     */
    @Test
    public void testList(){
        //Lpush Lrange rpop llen
        ListOperations listOperations = redisTemplate.opsForList();

        listOperations.leftPushAll("mylist", "a","b","c");
        listOperations.leftPush("mylist","d");

        List mylist = listOperations.range("mylist", 0, -1);
        System.out.println(mylist);

        listOperations.rightPop("mylist");

        Long size = listOperations.size("mylist");
        System.out.println(size);
    }

    /**
     * 操作集合类型的数据
     */
    @Test
    public void testSet(){
        //sadd smembers scard sinter sunion srem
        SetOperations setOperations = redisTemplate.opsForSet();

        setOperations.add("set1", "a","b","c","d");
        setOperations.add("set2", "a","b","x","y");

        Set members = setOperations.members("set1");
        System.out.println(members);

        Long size = setOperations.size("set1");
        System.out.println(size);

        Set intersect = setOperations.intersect("set1", "set2");
        System.out.println(intersect);

        Set union = setOperations.union("set1", "set2");
        System.out.println(union);

        setOperations.remove("set1", "a","b");
    }

    /**
     * 操作有序集合类型的数据
     */
    @Test
    public void testZset(){
        //zadd zrange zincrby zrem
        ZSetOperations zSetOperations = redisTemplate.opsForZSet();

        zSetOperations.add("zset1", "a", 10);
        zSetOperations.add("zset1", "b", 12);
        zSetOperations.add("zset1", "c", 9);

        Set zset1 = zSetOperations.range("zset1", 0, -1);
        System.out.println(zset1);

        zSetOperations.incrementScore("zset1", "c", 10);

        zSetOperations.remove("zset1", "a","b");
    }

    /**
     * 通用命令操作
     */
    @Test
    public void testCommon(){
        //keys exists type del
        Set keys = redisTemplate.keys("*");
        System.out.println(keys);

        Boolean name = redisTemplate.hasKey("name");
        Boolean set1 = redisTemplate.hasKey("set1");

        for (Object key : keys) {
            DataType type = redisTemplate.type(key);
            System.out.println(type.name());
        }

        redisTemplate.delete("mylist");
    }

```

## 五、Redis常用注解
| 注解         | 核心作用                                  | 核心场景                  | 关键属性/注意点                                                                 |
|--------------|-------------------------------------------|---------------------------|--------------------------------------------------------------------------------|
| @Cacheable   | 先查询缓存，缓存不存在时执行方法，结果自动存入Redis | 查询数据（详情、列表接口）| cacheNames/value（缓存前缀）、key（支持SpEL表达式）、unless（不缓存条件）、sync（防止缓存击穿） |
| @CachePut    | 必须执行方法，执行完成后将结果更新至缓存  | 新增、修改数据            | key需与@Cacheable保持一致，确保缓存能正常覆盖                                   |
| @CacheEvict  | 执行方法后删除指定缓存，支持清空全部缓存  | 删除数据、清空缓存        | allEntries=true（清空该缓存名称下所有缓存）；beforeInvocation=true（方法执行前删除缓存） |
| @Caching     | 组合注解，单个方法可实现多种缓存操作      | 复杂业务（多缓存更新/删除）| 可嵌套@CachePut、@CacheEvict等注解，实现组合缓存逻辑                             |
| @CacheConfig | 类级别注解，统一配置缓存名称（cacheNames） | 类内所有缓存方法同前缀    | 配置后，类中所有缓存方法无需重复编写cacheNames属性                             |


```java
// 先查询缓存，缓存不在时执行方法，再将结果自动存入Redis
@Cacheable(cacheNames = "setmealCache",key = "#categoryId")
public Result<List<Setmeal>> list(Long categoryId) {
    Setmeal setmeal = new Setmeal();
    setmeal.setCategoryId(categoryId);
    setmeal.setStatus(StatusConstant.ENABLE);
    List<Setmeal> list = setmealService.list(setmeal);
    return Result.success(list);
}
```

```java
// 删除单个用户缓存​
@CacheEvict(cacheNames = "user", key = "#id")​
public void deleteUser(Long id) {​
    userMapper.deleteById(id);​
}​

// 清空所有用户缓存​
@CacheEvict(cacheNames = "user", allEntries = true)​
public void clearAllUserCache() {}​
```

```java
// 更新用户缓存 + 清空用户列表缓存
@Caching(
    put = @CachePut(cacheNames = "user", key = "#user.id"),
    evict = @CacheEvict(cacheNames = "userList", allEntries = true)
)
public User updateUserAndClearList(User user) {
    userMapper.updateById(user);
    return user;
}
```

```java
@Service
@CacheConfig(cacheNames = "user") // 统一缓存名称
public class UserServiceImpl implements UserService {
    // 无需再写cacheNames
    @Cacheable(key = "#id")
    public User getUserById(Long id) {
        return userMapper.selectById(id);
    }
}
```

# Redis开发与集成指南

## 1. 简介

Redis 是一款高性能的 key-value 数据库，因其基于内存存储的特性，具备极高的读写性能，广泛应用于缓存、分布式锁、会话共享及排行榜等业务场景。

## 2. 环境准备与依赖

在 Spring Boot 项目中使用 Redis，首先需在 `pom.xml` 中引入 Spring Data Redis 的相关依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

```

## 3. 配置连接信息

在 `application.yml` 中配置 Redis 的连接参数。确保您的 Redis 服务已启动并可连接。

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    # password: your_password # 如有密码请取消注释
    database: 0 # 操作的数据库索引，默认0
    jedis:
      pool:
        max-active: 8 # 连接池最大连接数
        max-wait: 1ms # 连接池最大阻塞等待时间
        max-idle: 8   # 连接池最大空闲连接
        min-idle: 0   # 连接池最小空闲连接

```

## 4. 序列化配置（推荐）

Spring Data Redis 默认使用 JDK 序列化，会导致存储在 Redis 中的数据在客户端界面显示为乱码。建议自定义 RedisTemplate 配置类，使用 `StringRedisSerializer`：

```java
@Configuration
public class RedisConfiguration {

    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        // 设置连接工厂
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        // 设置 key 的序列化方式为 String
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        // 设置 hash key 的序列化方式为 String
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}

```

## 5. 核心使用方式

根据业务需求，Redis 的使用主要分为两类：

* **编程式调用**：通过 `RedisTemplate` 或其提供的 `Ops` 系列接口（如 `opsForValue()`）进行精准的 CRUD 操作，适用于复杂逻辑处理。
* **声明式缓存**：通过 Spring Cache 注解（如 `@Cacheable`、`@CacheEvict`）实现方法级别的自动缓存，适用于“查询缓存-更新-删除”等标准化流程，代码更简洁。
```java
// 先查询缓存，缓存不在时执行方法，再将结果自动存入Redis
@Cacheable(cacheNames = "setmealCache",key = "#categoryId")
public Result<List<Setmeal>> list(Long categoryId) {
    Setmeal setmeal = new Setmeal();
    setmeal.setCategoryId(categoryId);
    setmeal.setStatus(StatusConstant.ENABLE);
    List<Setmeal> list = setmealService.list(setmeal);
    return Result.success(list);
}
```

```java
// 删除单个用户缓存​
@CacheEvict(cacheNames = "user", key = "#id")​
public void deleteUser(Long id) {​
    userMapper.deleteById(id);​
}​

// 清空所有用户缓存​
@CacheEvict(cacheNames = "user", allEntries = true)​
public void clearAllUserCache() {}​
```

```java
// 更新用户缓存 + 清空用户列表缓存
@Caching(
    put = @CachePut(cacheNames = "user", key = "#user.id"),
    evict = @CacheEvict(cacheNames = "userList", allEntries = true)
)
public User updateUserAndClearList(User user) {
    userMapper.updateById(user);
    return user;
}
```

```java
@Service
@CacheConfig(cacheNames = "user") // 统一缓存名称
public class UserServiceImpl implements UserService {
    // 无需再写cacheNames
    @Cacheable(key = "#id")
    public User getUserById(Long id) {
        return userMapper.selectById(id);
    }
}
```

# Redis实战应用
## 1.基于session实现登录功能

### 1.1.流程
- **发送短信验证码**
- **短信验证码登录、注册**
- **校验登录状态**

#### 1） 发送短信验证码
- 校验手机号
- 如果手机号不符合条件，返回错误信息
- 符合条件，生成验证码
- 保存验证码到session
- 发送验证码
- 返回success
```java
    @Override
    public Result sendCode(String phone, HttpSession session) {
        //1.校验手机号
        if (RegexUtils.isPhoneInvalid(phone)) {
            //2.如果不符合，返回错误信息
            return Result.fail("手机号格式错误！");
        }

        //3.符合，生成验证码
        String code = RandomUtil.randomNumbers(6);

        //4.保存验证码到session
        session.setAttribute("code", code);
        //5.发送验证码
        log.debug("发送验证码成功，验证码：{}", code);
        //返回ok
        return Result.ok();
    }
```


#### 2） 短信验证码登录、注册
- 校验手机号
- 校验验证码
- 如果不一致，报错
- 如果一致，根据手机号查询用户
- 判断用户是否存在
- 不存在，创建新的用户并保存
- 保存用户信息到session中
```java
    @Override
    public Result login(LoginFormDTO loginForm, HttpSession session) {
        //1.校验手机号
        String phone = loginForm.getPhone();
        if (RegexUtils.isPhoneInvalid(phone)) {
            return Result.fail("手机号格式错误！");
        }
        //2.校验验证码
        Object cacheCode = session.getAttribute("code");
        String code = loginForm.getCode();
        if(cacheCode == null || !cacheCode.toString().equals(code)){
            //3.不一致，报错
            return Result.fail("验证码错误！");
        }
        //4.一致，根据手机号查询用户 select * from tb_user where phone = ?
        User user = query().eq("phone", phone).one();
        //5.判断用户是否存在
        if(user == null){
            //6.不存在，创建新用户并保存
            user = createUserWithPhone(phone);
        }

        //7.保存用户信息到session中
        session.setAttribute("user", BeanUtil.copyProperties(user, UserDTO.class));
        return Result.ok();
    }
```


#### 3） 校验登录状态
**采用Interceptor拦截器拦截用户信息，用户信息保存进ThreadLocal中，各个Controller接受到用户请求需要先拦截校验，再从ThreadLocal中取出用户信息**
- 获取session
- 获取session中的用户
- 判断用户是否存在
- 不存在，拦截
- 存在，保存用户信息到ThreadLocal
- 放行
```java
public class LoginInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //1.获取session
        HttpSession session = request.getSession();
        //2.获取session中的用户
        Object user = session.getAttribute("user");
        //3.判断用户是否存在
        if(user == null){
            //4.不存在，拦截
            response.setStatus(401);
            return false;
        }
        //5.存在，保存用户信息到ThreadLocal
        UserHolder.saveUser((UserDTO) user);
        //6.放行
        return true;
    }
}
```

注意：
1. **拦截器中需要排除某些接口，因为某些接口需要对未登录的用户开放**
2. **不能将所有的用户信息都存入ThreadLocal，否则会引起内存泄漏问题，同时会将用户敏感信息泄漏**

### 1.2.集群的session共享问题
* session共享问题：多台Tomcat并不共享session存储空间，当请求切换到不同tomcat服务时导致数据丢失的问题（**数据不共享**）
* session的替代方案应该满足：数据共享、内存存储、key-value结构

**Redis优化上述1所有流程**

### 1.3.基于Redis实现共享session登录
#### 1）校验登录状态
- 获取请求头中的token
- 基于token获取redis中的用户
- 判断用户是否存在
- 将查询到的Hash数据转为UserDTO对象
- 保存用户信息到ThreadLocal
- 刷新token有效期
- 放行
```java
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //1.获取请求头中的token
        String token = request.getHeader("authorization");
        if(StrUtil.isBlank(token)){
            //不存在，拦截，返回401状态码
            response.setStatus(401);
            return false;
        }

        //2.基于token获取Redis中的用户
        String key = RedisConstants.LOGIN_USER_KEY + token;
        Map<Object, Object> userMap = stringRedisTemplate.opsForHash().entries(key);
        //3.判断用户是否存在
        if(userMap.isEmpty()){
            //4.不存在，拦截
            response.setStatus(401);
            return false;
        }
        //5.将查询到的Hash数据转为UserDTO对象
        UserDTO userDTO = BeanUtil.fillBeanWithMap(userMap, new UserDTO(), false);

        //6.存在，保存用户信息到ThreadLocal
        UserHolder.saveUser(userDTO);

        //7.刷新token有效期
        stringRedisTemplate.expire(key, RedisConstants.CACHE_SHOP_TTL, TimeUnit.MINUTES);

        //8.放行
        return true;
    }
```


#### 2）短信验证码登录、注册
- 校验手机号
- 从redis中获取验证码并校验
- 判断用户是否存在
- 保存用户信息到redis中：
①随机生成token，作为登录令牌②将User对象转为Hash存储③存储④设置token有效期
- 返回token
```java
    @Override
    public Result login(LoginFormDTO loginForm, HttpSession session) {
        //1.校验手机号
        String phone = loginForm.getPhone();
        if (RegexUtils.isPhoneInvalid(phone)) {
            return Result.fail("手机号格式错误！");
        }
        //2.从Redis中获取验证码并校验
        Object cacheCode = stringRedisTemplate.opsForValue().get(LOGIN_CODE_KEY + phone);
        String code = loginForm.getCode();
        if(cacheCode == null || !cacheCode.toString().equals(code)){
            //3.不一致，报错
            return Result.fail("验证码错误！");
        }
        //4.一致，根据手机号查询用户 select * from tb_user where phone = ?
        User user = query().eq("phone", phone).one();
        //5.判断用户是否存在
        if(user == null){
            //6.不存在，创建新用户并保存
            user = createUserWithPhone(phone);
        }

        //7.保存用户信息到redis中
        //7.1.随机生成token，作为登录令牌
        String token = UUID.randomUUID().toString(true);

        //7.2.将User对象转为Hash存储
        UserDTO userDTO = BeanUtil.copyProperties(user, UserDTO.class);
        Map<String, Object> userMap = BeanUtil.beanToMap(userDTO, new HashMap<>(),
                CopyOptions
                        .create()
                        .setIgnoreNullValue(true)
                        .setFieldValueEditor((fieldName, fieldValue) -> fieldValue.toString()));

        //7.3.存储
        String tokenKey = LOGIN_USER_KEY + token;
        stringRedisTemplate.opsForHash().putAll(tokenKey, userMap);

        //7.4.设置token有效期
        stringRedisTemplate.expire(tokenKey, LOGIN_USER_TTL, TimeUnit.MINUTES);

        //8.返回token
        return Result.ok(token);
    }
```

**注意：Redis代替session需要考虑的问题**
1. **选择合适的数据结构**
2. **选择合适的key**
3. **选择合适的存储粒度**

### 1.4.登录拦截器的优化
可以创建两个拦截器：
第一个拦截器：拦截一切路径；保存访问用户的信息并存储到ThreadLocal当中
- 获取token
- 查询Redis的用户
- 保存到ThreadLocal
- 刷新token有效期
- 放行

```java
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //1.判断是否需要拦截（ThreadLocal中是否有用户）
        if(UserHolder.getUser() == null){
            //没有，需要拦截，设置状态码
            response.setStatus(401);
            //拦截
            return false;
        }
        //有用户，则放行
        return true;
    }
```
第二个拦截器：拦截需要登录的路径；用户未登录，拦截，已经登录则不拦截
- 查询ThreadLocal的用户：
- 不存在，则拦截
- 存在，则继续
```java
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //1.获取请求头中的token
        String token = request.getHeader("authorization");
        if(StrUtil.isBlank(token)){
            return true;
        }

        //2.基于token获取Redis中的用户
        String key = RedisConstants.LOGIN_USER_KEY + token;
        Map<Object, Object> userMap = stringRedisTemplate.opsForHash().entries(key);
        //3.判断用户是否存在
        if(userMap.isEmpty()){
            return true;
        }
        //5.将查询到的Hash数据转为UserDTO对象
        UserDTO userDTO = BeanUtil.fillBeanWithMap(userMap, new UserDTO(), false);

        //6.存在，保存用户信息到ThreadLocal
        UserHolder.saveUser(userDTO);

        //7.刷新token有效期
        stringRedisTemplate.expire(key, RedisConstants.CACHE_SHOP_TTL, TimeUnit.MINUTES);

        //8.放行
        return true;
    }
```

## 2.缓存的基本原理

### 2.1.缓存更新策略
| |内存淘汰|超时剔除|主动更新|
|---|---|---|---|
|说明|不用自己维护，利用Redis的内存淘汰机制，当内存不足时自动淘汰部分数据，下次查询时更新缓存|给缓存数据添加TTL时间，到期后自动删除缓存，下次查询时更新缓存|编写业务逻辑，在修改数据库的同时，更新缓存|
|一致性|差|一般|好|
|维护成本|无|低|高|

业务场景：
- 低一致性需求：使用**内存淘汰机制**
- 高一致性需求：**主动更新**，并且以**超时剔除**作为兜底方案

### 2.2.主动更新策略

#### （1）Read/Write Through Pattern
缓存与数据库整合为一个服务，由服务来维护一致性。调用者调用该服务，无需关心缓存一致性问题

#### （2）Write Behind Caching Pattern
调用者只调用缓存，由其他线程异步地将缓存数据持久化到数据库，保存最终一致（e.g.MQ）

#### （3）Cache Aside Pattern（主流）
由缓存的调用者在更新数据库的同时更新缓存
- **删除缓存**：更新数据库时让缓存失效，查询时再更新缓存
- **保证缓存——数据库操作原子性**：将缓存与数据库放在一个事务（单体系统），或者利用TCC等分布式事务方案（分布式系统）

**双写一致性**：
- 读操作：缓存命中直接返回，未命中则查询数据库，写入缓存，设定超时时间
- 写操作：先写**数据库**，然后**删除缓存**，要确保数据库与缓存操作的**原子性**


### 2.3.缓存穿透
#### 1.产生原因
客户端请求的数据在缓存中和数据库中都不存在，缓存永远不会生效，请求会**直接打到数据库**，不断发起这样的请求，会给数据库带来巨大压力
#### 2.解决方案
- **缓存空对象**：缓存null值
- 当客户端请求不存在的数据，先在Redis中缓存一个对应key的null值，当客户端再次请求这个不存在的数据直接命中null值，在代码逻辑中禁止该请求打入数据库
- 优点：实现简单维护方便
- 缺点：额外的内存消耗，可能造成短期的不一致

- **布隆过滤**：在客户端和redis中间加入一个布隆过滤器
- 优点：内存占用较少，没有多余key
- 缺点：实现复杂，存在误判可能
![alt text](image-4.png)

- **增强id的复杂度，避免被猜测id规律**
- **做好数据的基础格式校验**
- **加强用户权限校验**
- **做好热点参数的限流**
![alt text](image-2.png)


### 2.4.缓存雪崩
#### 1.产生原因
同一时段大量的缓存key同时失效或者Redis服务宕机，导致大量请求到达数据库，带来巨大压力
![alt text](image-3.png)
#### 2.解决方案
- 给不同的key的TTL添加**随机值**
- 利用**Redis集群**提高服务的可用性
> Redis“主从 + 哨兵”（Master-Slave + Sentinel）架构能够有效应对主节点宕机带来的单点故障问题，保障系统的高可用性:
> * **主从复制（Master-Slave）：**
> * **架构形态：** 建立主节点（Master）与从节点（Slave）的拓扑结构。
> * **核心功能：** 通过全量同步与增量同步机制实现数据的高效复制。主节点负责处理写请求并将数据同步至从节点，从节点负责读请求及数据备份，从而实现读写分离与数据冗余。
> * **哨兵机制（Sentinel）：**
> * **架构形态：** 由一个或多个哨兵实例组成的独立监控集群，用于对整个Redis架构进行分布式监控。
> * **核心功能：** 具备**实时监控**、**自动故障转移（Failover）**与**配置通知**能力。当主节点发生宕机（客观下线）时，哨兵集群会通过选举机制从从节点中推选出新的主节点，自动完成主备切换并更新客户端路由，确保Redis服务能够持续对外提供高可用保障。
- 给缓存业务添加**降级限流策略**
- 给业务添加多级缓存


### 2.5.缓存击穿

#### 1.产生原因
一个被**高并发访问**并且**缓存重建业务较复杂**的key突然失效，无数的请求访问会在瞬间给数据库带来巨大的冲击
![alt text](image-5.png)

#### 2.解决方案
- 互斥锁
- 逻辑过期
![alt text](image-6.png)
- 优缺点如下

|解决方案|优点|缺点|
|---|---|---|
|**互斥锁**|没有额外的内存消耗，**保证一致性**，实现简单|线程**需要等待**，性能受影响，可能有死锁风险|
|**逻辑过期**|线程**无需等待**，性能较好|**不保证一致性**，有额外内存消耗，实现复杂|

#### 3.利用互斥锁解决缓存击穿
![alt text](image-7.png)
```java
public Result queryById(Long id) {
        // 缓存穿透
        //Shop shop = queryWithPassThrough(id);

        //互斥锁解决缓存击穿
        Shop shop = queryWithMutex(id);
        if (shop == null) {
            return Result.fail("店铺不存在！");
        }
        //返回
        return Result.ok(shop);
    }

    public Shop queryWithMutex(Long id){
        String key = CACHE_SHOP_KEY + id;
        //1.从redis中查询商铺缓存
        String shopJson = stringRedisTemplate.opsForValue().get(key);
        //2.判断是否存在
        if (StrUtil.isNotBlank(shopJson)) {
            //存在，返回
            return JSONUtil.toBean(shopJson, Shop.class);
        }
        //3.判断命中的是否是空值
        if(shopJson != null){
            //返回一个错误信息
            return null;
        }

        //4.实现缓存重建
        //4.1.获取互斥锁
        String lockKey = "lock:shop:" + id;
        Shop shop = null;
        try {
            boolean isLock = tryLock(lockKey);
            //4.2.判断是否获取成功
            if(isLock){
                //4.3.失败，则休眠并重试
                Thread.sleep(50);
                return queryWithMutex(id);
            }
            //4.4.成功，根据id查询数据库
            shop = getById(id);
            //模拟重建的延时
            Thread.sleep(200);
            //5.不存在，返回错误
            if(shop == null){
                //将空值写入redis
                stringRedisTemplate.opsForValue().set(key, "", CACHE_NULL_TTL, TimeUnit.MINUTES);
                return null;
            }
            //存在，写入redis
            stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop), CACHE_SHOP_TTL, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }finally {
            //释放互斥锁
            unLock(lockKey);
        }
        //返回
        return shop;

    }

    /**
     * 加互斥锁
     * @param key
     * @return
     */
    private boolean tryLock(String key){
        Boolean flag = stringRedisTemplate.opsForValue().setIfAbsent(key, "1", 10, TimeUnit.SECONDS);
        return BooleanUtil.isTrue(flag);
    }

    /**
     * 释放锁
     * @param key
     */
    private void unLock(String key) {
        stringRedisTemplate.delete(key);
    }
```