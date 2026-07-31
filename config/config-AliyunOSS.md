# 阿里云OSS使用指南

## 1. 前置准备

在编写代码之前，请确保已完成以下操作：

* **阿里云账号**：开通 OSS 服务。
* **创建 Bucket**：记录下你的 `Bucket 名称`。
* **获取密钥**：在阿里云控制台创建 AccessKey（AccessKey ID 和 AccessKey Secret）。
* **获取 Endpoint**：在 Bucket 概览页获取对应的外网访问节点。
* **依赖引入**：在 `pom.xml` 中引入必要的 SDK 依赖：

```xml
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.17.4</version> </dependency>

```

## 2. 配置说明

在 `application.yml` (或 `application.properties`) 中配置 OSS 相关信息：

```yaml
sky:
  alioss:
    endpoint: https://oss-cn-hangzhou.aliyuncs.com
    access-key-id: yourAccessKeyId
    access-key-secret: yourAccessKeySecret
    bucket-name: yourBucketName

```

---

## 3. 核心组件实现 (代码实现)
* **AliOssProperties**: 用于读取 yml 配置文件的属性类。

```java
@Component
@ConfigurationProperties(prefix = "sky.alioss")
@Data
public class AliOssProperties {

    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;

}
```

* **AliOssUtil**: 核心工具类，封装了 `OSSClient` 的创建、文件上传逻辑及访问路径拼接。

```java
@Data
@AllArgsConstructor
@Slf4j
public class AliOssUtil {

    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;

    /**
     * 文件上传
     *
     * @param bytes
     * @param objectName
     * @return
     */
    public String upload(byte[] bytes, String objectName) {

        // 创建OSSClient实例。
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        try {
            // 创建PutObject请求。
            ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(bytes));
        } catch (OSSException oe) {
            System.out.println("Caught an OSSException, which means your request made it to OSS, "
                    + "but was rejected with an error response for some reason.");
            System.out.println("Error Message:" + oe.getErrorMessage());
            System.out.println("Error Code:" + oe.getErrorCode());
            System.out.println("Request ID:" + oe.getRequestId());
            System.out.println("Host ID:" + oe.getHostId());
        } catch (ClientException ce) {
            System.out.println("Caught an ClientException, which means the client encountered "
                    + "a serious internal problem while trying to communicate with OSS, "
                    + "such as not being able to access the network.");
            System.out.println("Error Message:" + ce.getMessage());
        } finally {
            if (ossClient != null) {
                ossClient.shutdown();
            }
        }

        //文件访问路径规则 https://BucketName.Endpoint/ObjectName
        StringBuilder stringBuilder = new StringBuilder("https://");
        stringBuilder
                .append(bucketName)
                .append(".")
                .append(endpoint)
                .append("/")
                .append(objectName);

        log.info("文件上传到:{}", stringBuilder.toString());

        return stringBuilder.toString();
    }
}

```

* **OssConfiguration**: 通过 `@Bean` 方式将工具类交给 Spring 容器管理，实现单例模式，提高性能。

```java
@Configuration
@Slf4j
public class OssConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public AliOssUtil aliOssUtil(AliOssProperties aliOssProperties){
        log.info("开始创建阿里云文件上传工具类对象：{}",aliOssProperties);
        return new AliOssUtil(aliOssProperties.getEndpoint(),
                aliOssProperties.getAccessKeyId(),
                aliOssProperties.getAccessKeySecret(),
                aliOssProperties.getBucketName());
    }
}
```

> **优化建议**：目前你的 `upload` 方法中每次上传都 `new OSSClientBuilder().build(...)`，在高并发下会有性能损耗。建议将 `OSSClient` 实例作为 `AliOssUtil` 的成员变量，在构造方法中初始化，并在 `AliOssUtil` 中实现 `DisposableBean` 接口以销毁 `ossClient`。

---

## 4. 功能使用 (Controller)

通过 `CommonController` 提供统一的 RESTful API 接口。

* **接口说明**：`POST /user/common/upload`
* **参数**：`MultipartFile file` (文件对象)
* **逻辑**：
1. 提取文件原始后缀。
2. 利用 `UUID` 重命名文件，防止文件名冲突（推荐做法）。
3. 调用 `AliOssUtil` 上传并获取访问路径。

```java
/**
 * 通用接口
 */
@RestController
@RequestMapping("/user/common")
@Api(tags = "通用接口")
@Slf4j
public class CommonController {

    @Autowired
    private AliOssUtil aliOssUtil;

    /**
     * 文件上传
     * @param file
     * @return
     */
    @PostMapping("/upload")
    @ApiOperation("文件上传")
    public Result<String> upload(MultipartFile file){
        log.info("文件上传，{}",file);

        try {
            //原始文件名
            String originalFileName = file.getOriginalFilename();
            //获取原始文件名的后缀
            String extension = originalFileName.substring(originalFileName.lastIndexOf("."));

            String objectName = UUID.randomUUID().toString() + extension;

            //文件的请求路径
            String filePath = aliOssUtil.upload(file.getBytes(),objectName);
            return Result.success(filePath);
        } catch (IOException e) {
            log.error("文件上传失败：{}", e);
        }

        return Result.error(MessageConstant.UPLOAD_FAILED);
    }
}

```

---

## 5. 注意事项

1. **安全性**：切勿将 `AccessKey` 硬编码在代码中。建议使用环境变量或配置中心加密存储。
2. **异常处理**：目前的 `upload` 方法中使用了 `System.out.println`，在正式项目中建议使用 `log.error` 记录到日志系统，并抛出自定义异常。
3. **文件名管理**：如果业务需要保留原始文件名，可以在数据库中设计表结构，一张表存原始文件名，另一张表存 OSS 存储路径（建议使用 UUID 命名文件防止覆盖）。
4. **权限控制**：确保 Bucket 的读写权限（ACL）设置正确，如果是公开读取，设置为“公共读”；如果是私有资源，需要使用生成的签名 URL 访问。

---

## 6. 常见问题排查

* **上传失败 (ClientException)**：检查网络连通性，以及是否配置了正确的 Proxy。
* **鉴权失败 (OSSException)**：检查 AccessKey 是否过期，以及是否拥有对指定 Bucket 的操作权限。
* **路径无法访问**：确认 Bucket 权限是否为“公共读”，若为“私有”，则需要使用 `generatePresignedUrl` 获取临时访问地址。