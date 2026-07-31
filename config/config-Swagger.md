# Swagger (Knife4j) 使用指南

## 1. 引入依赖

在项目的 `pom.xml` 文件中添加 Knife4j 的 Maven 坐标。Knife4j 是基于 Swagger 的增强工具，能够提供更友好的接口文档界面。

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>4.3.0</version> </dependency>

```

## 2. 配置 Knife4j

在配置类中添加 `Docket` Bean，用于定义接口文档的基本信息（如标题、描述、版本号等），并配置扫描路径。

```java
@Configuration
@EnableSwagger2
public class SwaggerConfiguration {

    @Bean
    public Docket docket() {
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("项目接口文档")
                .version("1.0")
                .description("项目接口详细描述")
                .build();
        
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.controller")) // 修改为你的Controller包路径
                .paths(PathSelectors.any())
                .build();
    }
}

```

**application.yml**
```yml
knife4j:
  enable: true
```

## 3. 设置静态资源映射

如果你的项目拦截了所有请求（例如配置了全局拦截器或静态资源路径限制），必须放行 Swagger 的相关资源，否则接口文档页面将无法访问或样式加载失败。

```java
@Configuration
public class WebMvcConfiguration extends WebMvcConfigurationSupport {

    /**
     * 设置静态资源映射
     */
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
}

```

---

### 注意事项

* **文档访问地址**：启动项目后，通常访问 `http://localhost:8080/doc.html` 即可查看接口文档。
* **接口注解**：在 Controller 和实体类上配合使用 `@Api`、`@ApiOperation`、`@ApiModel` 等注解，可以让生成的文档更加清晰易懂。
* **环境隔离**：建议在生产环境通过配置文件控制 Swagger 的开启状态（使用 `@Profile({"dev", "test"})`），避免生产环境暴露接口定义信息。

**WebMvcConfiguration.java(可直接使用)**
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


    /**
     * 通过knife4j生成接口文档
     * @return
     */
    @Bean
    public Docket docket() {
        log.info("准备生成管理端接口文档...");
        ApiInfo apiInfo = new ApiInfoBuilder()
                .title("YOUR_API_DOC_NAME")
                .version("YOUR_API_VERSION")
                .description("YOUR_API_DOC_DESCRIPTION")
                .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .groupName("YOUR_GROUP_NAME")
                .apiInfo(apiInfo)
                .select()
                .apis(RequestHandlerSelectors.basePackage("YOUR_PACKAGE_NAME"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }


    /**
     * 设置静态资源映射
     * @param registry
     */
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        log.info("开始设置静态资源映射...");
        registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
    }

}
```