# JWT登录校验开发指南

## 1.令牌技术
### 1）JWT令牌：
   定义了一种简洁的、自包含的格式，用于在通信双方以json数据格式安全的传输信息
### 2）组成：
* Header头，记录令牌类型，签名算法等
* Payload（有效载荷），自定义信息，默认信息等
* Signature签名，防止Token被篡改，保证安全性，融入Header和Payload，加入指定密钥，指定算法计算成的

### 3）JWT工具类（生成与解析）
给每一个登录的用户分配一个JWT Token令牌，每次请求访问接口的时候都校验一次Token，提高系统安全性

**JwtUtil.java**
```java
public class JwtUtil {
    /**
     * 生成jwt
     * 使用Hs256算法, 私匙使用固定秘钥
     *
     * @param secretKey jwt秘钥
     * @param ttlMillis jwt过期时间(毫秒)
     * @param claims    设置的信息
     * @return
     */
    public static String createJWT(String secretKey, long ttlMillis, Map<String, Object> claims) {
        // 指定签名的时候使用的签名算法，也就是header那部分
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;

        // 生成JWT的时间
        long expMillis = System.currentTimeMillis() + ttlMillis;
        Date exp = new Date(expMillis);

        // 设置jwt的body
        JwtBuilder builder = Jwts.builder()
                // 如果有私有声明，一定要先设置这个自己创建的私有的声明，这个是给builder的claim赋值，一旦写在标准的声明赋值之后，就是覆盖了那些标准的声明的
                .setClaims(claims)
                // 设置签名使用的签名算法和签名使用的秘钥
                .signWith(signatureAlgorithm, secretKey.getBytes(StandardCharsets.UTF_8))
                // 设置过期时间
                .setExpiration(exp);

        return builder.compact();
    }

    /**
     * Token解密
     *
     * @param secretKey jwt秘钥 此秘钥一定要保留好在服务端, 不能暴露出去, 否则sign就可以被伪造, 如果对接多个客户端建议改造成多个
     * @param token     加密后的token
     * @return
     */
    public static Claims parseJWT(String secretKey, String token) {
        // 得到DefaultJwtParser
        Claims claims = Jwts.parser()
                // 设置签名的秘钥
                .setSigningKey(secretKey.getBytes(StandardCharsets.UTF_8))
                // 设置需要解析的jwt
                .parseClaimsJws(token).getBody();
        return claims;
    }

}

```

### 4）JWT优缺点（面试常问）

| 优点 | 缺点 |
|---|---|
| 无需在服务端存储，减轻服务器压力 | 令牌一旦签发，无法中途废止（难做退出登录） |
| 支持跨域、分布式系统 | Payload可解码，不要存放敏感信息 |
| 自包含，减少数据库查询 | 令牌过长会增加请求头体积 |

**记忆点**：JWT = Header（类型+算法） + Payload（数据） + Signature（防篡改）

---

## 2.Filter过滤器
![alt text](image-2.png)
### 1）过滤器
Filter过滤器可以把对接口的请求拦截下来，从而进行Token验证，防止未登录就可直接访问数据
过滤器一般完成登录校验、统一编码处理、敏感字符处理

使用方法：
* 定义一个类实现Filter接口，实现重写（override）所有方法
* Filter类上加@WebFilter注解配置拦截路径。引导类上加@ServletComponentScan注解开启Servlet组件支持
* 其中重写的doFilter方法就是拦截操作，在其下写出JWT校验业务逻辑即可在每次请求操作前都校验JWT令牌

### 2）登录校验Filter
![alt text](image-1.png)
**注意：1.除了登录请求，其他请求都需要拦截进行令牌校验2.拦截到请求后，有令牌、并且令牌校验通过，否则都返回未登录错误结果（401）**

```Java
@Slf4j
@WebFilter(urlPatterns = "/*")//“/*"表示所有请求都进行拦截
public class TokenFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //1.获取到请求路径
        String path = request.getRequestURI();// /employee/login

        //2.判断是否是登录请求，如果路径中包含/login，说明是登陆操作，放行
        if(path.contains("login")){
            log.info("登录操作，放行...");
            filterChain.doFilter(request,response);//放行操作
            return;
        }

        //3.获取请求头当中的token
        String token = request.getHeader("token");

        //4.判断token是否存在，不存在，说明用户没有登录，返回错误信息（响应401状态码）
        if(token == null || token.isEmpty()){
            log.info("令牌为空，响应401");
            response.setStatus(401);
            return;
        }
        //5.如果token存在，校验令牌，如果校验失败，返回错误信息（响应401状态码）
        try {
            JwtUtils.parseToken(token);
        } catch (Exception e) {
            log.info("令牌非法，响应401");
            response.setStatus(401);
            return;
        }

        //6.校验通过，放行
        log.info("令牌合法，放行");
        filterChain.doFilter(request,response);//放行操作
    }
}
```

### 3）拦截范围
```Java
@WebFilter(urlPatterns = "/*")
public class TokenFilter implements Filter
```

Filter可以根据需求，配置不同的拦截资源路径
1.拦截具体路径：eg./login ，只有访问/login路径才可被拦截
2.目录拦截：eg./emps/*，访问/emps下的所有资源都会被拦截
3.拦截所有：eg./*，访问所有资源都会被拦截

### 4）Filter生命周期（面试常问）

Filter接口有三个方法，代表生命周期的三个阶段：

* **init(FilterConfig config)**：Filter初始化时执行，只执行**一次**
* **doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)**：每次请求拦截时执行，可执行**多次**
* **destroy()**：Filter销毁时执行，只执行**一次**

**执行流程**：请求 → init（一次） → doFilter（多次拦截） → destroy（一次）

## 3.Interceptor拦截器
### 令牌校验——拦截器
1. 获取请求url
2. 判断是否是登录接口，若是，放行
3. 若不是，获取请求头中的token
4. 判断token是否存在，不存在则响应401
5. 校验token，校验失败则响应401
6. 校验通过，放行

**JwtClaimsConstant.java**(JWT常量类)
```java
public class JwtClaimsConstant {

    public static final String EMP_ID = "empId";
    public static final String USER_ID = "userId";
    public static final String PHONE = "phone";
    public static final String USERNAME = "username";
    public static final String NAME = "name";

}
```

**JwtTokenUserInterceptor.java**
```java
/**
 * jwt令牌校验的拦截器
 */
@Component
@Slf4j
public class JwtTokenUserInterceptor implements HandlerInterceptor {

    @Autowired
    private JwtProperties jwtProperties;

    /**
     * 校验jwt
     *
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        log.info("当前线程的ID:"+Thread.currentThread().getId());

        //判断当前拦截到的是Controller的方法还是其他资源
        if (!(handler instanceof HandlerMethod)) {
            //当前拦截到的不是动态方法，直接放行
            return true;
        }

        //1、从请求头中获取令牌
        String token = request.getHeader(jwtProperties.getUserTokenName());

        //2、校验令牌
        try {
            log.info("jwt校验:{}", token);
            Claims claims = JwtUtil.parseJWT(jwtProperties.getUserSecretKey(), token);
            Long userId = Long.valueOf(claims.get(JwtClaimsConstant.USER_ID).toString());
            log.info("当前用户id：{}", userId);
            BaseContext.setCurrentId(userId);
            //3、通过，放行
            return true;
        } catch (Exception ex) {
            //4、不通过，响应401状态码
            response.setStatus(401);
            return false;
        }
    }
}

```


### 2）拦截器的三个方法（面试常问）

HandlerInterceptor接口有三个方法：

* **preHandle**：在目标方法执行前执行（最常用，登录校验写这里）
  * 返回 true：放行
  * 返回 false：拦截
* **postHandle**：在目标方法执行后，视图渲染前执行（可修改ModelAndView）
* **afterCompletion**：在视图渲染完成后执行（常用于资源清理、异常记录）

**执行顺序**：preHandle → Controller方法 → postHandle → 视图渲染 → afterCompletion

### 3）拦截器配置类

拦截器写完后需要注册才能生效：

**WebMvcConfiguration.java**
```java
/**
 * 配置类，注册web层相关组件
 */
@Configuration
@Slf4j
public class WebMvcConfiguration extends WebMvcConfigurationSupport {

    @Autowired
    private JwtTokenUserInterceptor jwtTokenUserInterceptor;
    @Autowired
    private JwtProperties jwtProperties;

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        log.info("当前线程的ID:"+Thread.currentThread().getId());

        if (!(handler instanceof HandlerMethod)) {
            return true;
        }

        String token = request.getHeader(jwtProperties.getUserTokenName());

        if (token == null || token.isEmpty()) {
            log.warn("请求头中未携带token，路径:{}", request.getRequestURI());
            response.setStatus(401);
            return false;
        }

        try {
            log.info("jwt校验:{}", token);
            Claims claims = JwtUtil.parseJWT(jwtProperties.getUserSecretKey(), token);
            Long userId = Long.valueOf(claims.get(JwtClaimsConstant.USER_ID).toString());
            log.info("当前用户id：{}", userId);
            BaseContext.setCurrentId(userId);
            return true;
        } catch (Exception ex) {
            log.error("JWT解析失败:{}", ex.getMessage());
            response.setStatus(401);
            return false;
        }
    }


    /**
     * 注册自定义拦截器
     * @param registry
     */
    protected void addInterceptors(InterceptorRegistry registry) {
        log.info("开始注册自定义拦截器...");

        //todo 在这里可以指定不拦截用户访问哪些接口
        registry.addInterceptor(jwtTokenUserInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns("/user/login", "/user/register")
                .excludePathPatterns("/product/detail/**")
                .excludePathPatterns("/category/list")
                .excludePathPatterns("/product/hot")
                .excludePathPatterns("/doc.html", "/webjars/**", "/swagger-resources/**", "/v2/api-docs/**", "/swagger-ui.html/**");

    }


}

```


**application.yml**

```yml
#JWT配置
sky:
  jwt:
    # 管理端
    # 设置jwt签名加密时使用的秘钥
    admin-secret-key: itcast
    # 设置jwt过期时间
    admin-ttl: 7200000
    # 设置前端传递过来的令牌名称
    admin-token-name: token

    # 用户端
    # 设置jwt签名加密时使用的秘钥
    user-secret-key: itsean
    # 设置jwt过期时间
    user-ttl: 7200000
    # 设置前端传递过来的令牌名称
    user-token-name: token
```

### 4）Filter 与 Interceptor 区别（面试必背）

| 对比项 | Filter（过滤器） | Interceptor（拦截器） |
|---|---|---|
| **来源** | Servlet规范，tomcat等容器支持 | Spring框架提供 |
| **执行时机** | 请求进入Servlet之前 | DispatcherServlet之后，Controller之前 |
| **依赖** | 不依赖Spring容器 | 依赖Spring容器 |
| **获取Bean** | 不能直接获取Spring Bean | 可以注入Spring Bean |
| **使用范围** | 任何Java Web项目 | 只有Spring项目能用 |
| **执行链** | doFilter中手动调用chain.doFilter() | preHandle返回true自动放行 |

**记忆口诀**：Filter是Servlet的"门卫"，Interceptor是Spring的"管家"；Filter在门外，Interceptor在门里。

**注意：如果先开发登录功能接口，在每次接口文档测试时，由于请求未带有JWT令牌，所以接口文档测试请求会一直不通过，于是我们需要获得令牌并放入请求头当中（knife4j）**
![alt text](image-3.png)

## 4.ThreadLocal传递当前登录用户ID（实战高频）

登录校验通过后，后续业务经常需要知道"当前登录用户是谁"。使用 **ThreadLocal** 在同一次请求中共享用户ID：

**BaseContext.java**
```java
public class BaseContext {
    private static final ThreadLocal<Long> threadLocal = new ThreadLocal<>();

    public static void setCurrentId(Long id) {
        threadLocal.set(id);
    }

    public static Long getCurrentId() {
        return threadLocal.get();
    }

    public static void remove() {
        threadLocal.remove(); //防止内存泄漏，请求结束后清理
    }
}
```

**使用场景**：在拦截器校验Token后，将解析出的用户ID存入ThreadLocal，后续Service层直接 `BaseContext.getCurrentId()` 获取，无需重复解析Token。

**使用步骤**：
1. 拦截器校验通过 → `BaseContext.setCurrentId(userId)`
2. Service层需要当前用户 → `BaseContext.getCurrentId()`
3. afterCompletion中 → `BaseContext.remove()` 清理数据

**为什么用ThreadLocal？**
- 每个线程有独立副本，**线程隔离**，多请求并发不会串数据
- 同一次请求中（同一个线程），任何地方都能取到值

**注意**：请求结束后一定要 `remove()`，否则Tomcat线程复用会导致数据错乱。

## 5.登录功能开发完整流程速记

| 步骤 | 操作 | 对应技术 |
|---|---|---|
| **1.用户登录** | 校验账号密码 → 生成JWT → 返回给前端 | JWT |
| **2.前端存储** | 将JWT存入浏览器本地存储（LocalStorage） | 前端技术 |
| **3.后续请求** | 每次请求在请求头中携带Token | Axios拦截器 |
| **4.后端校验** | Filter/Interceptor拦截 → 解析Token → 放行或返回401 | Filter / Interceptor |
| **5.传递用户** | 解析出的用户ID存入ThreadLocal，供业务层使用 | ThreadLocal |
| **6.退出登录** | 前端删除Token即可（JWT无状态，服务端无需处理） | 前端技术 |

**记忆口诀**：登录发令牌，请求带头走，Filter来校验，ThreadLocal传用户。