## MVC模式

- Model :模型层
- View：视图层
- **Controller: 控制层**

<img src="C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023131259940.png" alt="image-20211023131259940" style="zoom:67%;" />

![image-20211023131528298](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023131528298.png)

Current+ GA(general availability)  snapshot:快照版本

![image-20211023132926633](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023132926633.png)

![image-20211023141253101](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023141253101.png)

@ResponseBody可以将hello转换为字符串类型？ 

@RestController=  @ResponseBody+@Controller

前后端分离？

**@RestController 包含 @GetMapping  @PostMapping  @DeleteMapping  @PutMapping**

Postman是rest的一种工具

前端调后端，4开头的问题属于前端问题，5开头的问题属于后端问题。

**application.yml bootstrap.yml**

![image-20211023145039412](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023145039412.png)

bootstrap.yml 的使用场景SpringCloud, 加密解密，固定参数

![image-20211023145501102](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023145501102.png)

dev tools的使用，不需要重启，适用于轻量级项目

![image-20211023152630106](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023152630106.png)

加了@Configuration, 文件可以扫描到它的配置

## SpringBoot自定义属性资源配置

![image-20211023162356355](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023162356355.png)

User的属性的值在MyConfig.properties文件里定义

- 在yml中实现自定义配置与表达式

在yml里配属性：

```yaml
self:
	custom:
		sdkSecret: 1234	
```

需要取值用@Value和$符号

```java
@Value("${self.custom.sdkSecret}")
private String sdkSecret
```

自定义logo

在resources目录下建一个banner.txt的文件，并在yml中配置

```yml
spring:
	banner:
		image:
			location: classpath:banner/cat.png
```

## web请求静态资源

spring:mvc:static-path-pattern: /abc/**



## Restful接口请求风格

四个请求：get, post, put, delete

![image-20211023171325801](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023171325801.png)

![image-20211023171337049](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023171337049.png)

精简，把stu去掉，统一移到前面的@RequestMapping

## 接收参数的常用注解

### @PathVariable: 路径参数   @RequestParam:请求参数

```java
@GetMapping("{stuId}/get")
public String getStu(@PathVariable("stuId") String StuId,
                     @RequestParam Integer id,
                     @RequestParam String name)
```

 @RequestParam: 用于获得url中的请求参数，如果参数变量保持一致，该注解可以省略

## 接口返回响应对象

JSONResult, 告诉前端处理的数据以及是否反应成功

如何降低耦合度

## 文件上传

前端发起上传，后端提供接口

![image-20211023212420828](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023212420828.png)

**前端是getmapping, postmapping是后端向前端提交，不是访问地址**

![image-20211023212716528](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023212716528.png)

## 自定义异常页面（springboot内置，无法前后端分离）

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>静态资源文件</title>
</head>
<body>
    <div id="test" style="color: red">
        系统发生异常，请联系管理员~
    </div>

</body>
</html>

spring:
  servlet:
    multipart:
      max-file-size: 1MB #文件上传大小的限制
      max-request-size: 3MB #批量上传最大限制
```

![image-20211023215435237](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023215435237.png)

## 统一异常封装处理

```java
@ControllerAdvice
public class GraceExceptionHandler {
    @ExceptionHandler(FileSizeLimitExceededException.class)
    @ResponseBody
    public JSONResult returnMaxFileSizeLimit(FileSizeLimitExceededException e){
        return JSONResult.errorMsg("文件大小不能超过500KB");
    }
}
```

## 实现拦截器

```java
@Configuration//要被容器扫描到
public class InterceptorConfig implements WebMvcConfigurer {
    @Bean //
    public UserInfoInterceptor userInfoInterceptor(){
        return new UserInfoInterceptor();
    }
    @Override
    public void addInterceptors(InterceptorRegistry registry){
        //注册拦截器
        registry.addInterceptor(userInfoInterceptor()).addPathPatterns("/upload");
    }

}
```

```java
@Slf4j
public class UserInfoInterceptor implements HandlerInterceptor {
    /**
     *拦截请求，访问controller之前
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String userId = request.getHeader("userId");
        String userToken = request.getHeader("userToken");
        if (!StringUtils.hasLength(userId)||!StringUtils.hasLength(userToken)){
            log.info("用户校验不通过，信息不完整");
            return false;
        }
        if (!userId.equalsIgnoreCase("1001")||userId.equalsIgnoreCase("abc")){
            log.error("用户权限不通过");
            return false;
        }
        return false;
    }

    /**
     * 请求访问到controller之后，渲染视图之前
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @throws Exception
     */

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }

    /**
     *请求访问到controller之后，渲染视图之后
     * @param request
     * @param response
     * @param handler
     * @param ex
     * @throws Exception
     */

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
    }

}
```

## 定时任务的实现

定时任务表达式

```java
@Configuration  //1.标记配置类，使得springboot容器可以扫描到
@EnableScheduling   //2.开启定时任务
@Slf4j
public class MyTask {
    //添加一个任务，并且注明任务的运行表达式
    @Scheduled(cron ="*/5 * * * * ?" )
    public void publishMsg(){
        log.warn("开始执行任务"+ LocalDateTime.now());
    }
}
```

## 异步任务

```java
@Component
@EnableAsync
@Slf4j
public class MyAsyncTask {
    @Async
    public void publishMsg(){
       try {
           Thread.sleep(5000);
           log.info("异步任务完成");
       }catch (InterruptedException e) {
           e.printStackTrace();
       }
    }
}
```

持久层框架: Hibernate   JPA  **Mybatis**

数据源：C3P0  DBCP  **DRUID**  BoneCP  **HicariCP**(SPRINGBOOT官方首推默认)

## 数据源

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
```

## 配置HicariCP数据源

```yml
spring:
  servlet:
    multipart:
      max-file-size: 500KB #文件上传大小的限制
      max-request-size: 2MB #批量上传最大限制
  datasource:
    type: com.zaxxer.hikari.HikariDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:33060/imooc-springboot-learn?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true
    username: root
    password: DST773344
    hikari:
      connection-timeout: 30000
      minimum-idle: 5
      maximum-pool-size: 20
      auto-commit: true
      idle-timeout: 600000
      pool-name: DateSourceHikariCP
      max-lifetime: 1800000
      connection-test-query: SELECT 1


debug: true
#myabtis相关配置
mybatis:
  type-aliases-package: com.du.pojo
  mapper-locations: classpath:mappers/*.xml
mapper:
  mappers: com.du.mymapper.MyMapper
  not-empty: false
  identity: MYSQL
  #分页配置
pagehelper:
  helper-dialect: mysql
  support-methods-arguments: true
```



## Mybatis逆向工具生成mapper和pojo

![image-20211026192138286](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211026192138286.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
<!--    <classPathEntry location="/Program Files/IBM/SQLLIB/java/db2java.zip" />-->

    <context id="MysqlContext" targetRuntime="MyBatis3" defaultModelType="flat">
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
            <property name="mappers" value="com.du.my.mapper.MyMapper"/>
        </plugin>
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:33060/imooc-springboot-learn"
                        userId="root"
                        password="DST773344">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <javaModelGenerator targetPackage="com.du.pojo" targetProject="src/main/java"/>


        <sqlMapGenerator targetPackage="mapper"  targetProject="src/main/resources"/>


        <javaClientGenerator type="XMLMAPPER" targetPackage="com.du.mapper"  targetProject="src/main/java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <table tableName="stu"/>



    </context>
</generatorConfiguration>

```

## 

![image-20211027195345374](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027195345374.png)



## 通过接受bean的业务对象引出验证框架

![image-20211027195725004](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027195725004.png)



## 使用Hibernate对Bean参数进行校验 

Hibernate常用校验注解

@NotNull	@NotEmpty	@NotBlank

![image-20211027200058311](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027200058311.png)

![image-20211027200137574](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027200137574.png)

![image-20211027200515178](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027200515178.png)

![image-20211027204532317](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027204532317.png)

![image-20211027204959318](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027204959318.png)

##  整合MyBatis - 实现查询操作 

![image-20211027205538580](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027205538580.png)

![image-20211027205646193](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027205646193.png)

![image-20211027205822871](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027205822871.png)

![image-20211027210032395](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027210032395.png)

![image-20211027210107532](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027210107532.png)

![image-20211027210259611](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027210259611.png)

## 整合MyBatis - 实现修改操作

![image-20211027210717339](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027210717339.png)

## 整合MyBatis - 实现删除操作

![image-20211027210844684](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027210844684.png)

![image-20211027210909210](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027210909210.png)

![image-20211027211217867](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027211217867.png)

## Service层引入事务回滚

新增、删除、修改需要事务回滚；先在接口定义，再到实现的类中实现

![image-20211027213929999](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211027213929999.png)

**@Transactional**

**propagation.REQUIRED**

![image-20211028185446383](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028185446383.png)

![image-20211028191709013](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028191709013.png)

##  实现Mybatis自定义sql的查询

select * from stu where id = ‘1001’ 不建议使用，*使mybatis对所有列进行查询，耗费性能

推荐： select  id,name,sex  from stu where id = ‘1001’ 

**自定义mapper不需继承通用mapper**

mapper映射、xml配置、service调用、接口、controller

![](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028193458002.png

![image-20211028193611128](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028193611128.png)

![image-20211028193625827](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028193625827.png)

![image-20211028193849357](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028193849357.png)

![image-20211028193910365](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028193910365.png)

## 整合自定义阿里Druid数据源

把依赖和配置修改一下

## 开启mybatis的sql执行日志打印 

![image-20211028200611472](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028200611472.png)

## 使用AOP监控service执行时间

![image-20211028200910895](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028200910895.png)

![image-20211028201035141](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028201035141.png)

aspect代表是切面， 环绕通知计算时间

![image-20211028201240335](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028201240335.png)

![image-20211028201634943](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028201634943.png)

![image-20211028201841453](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028201841453.png)



## springboot整合junit5进行测试

![image-20211028202026041](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028202026041.png)

![image-20211028202115624](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028202115624.png)

@Test是junit的

##  SpringBoot多环境profile定义

![image-20211028202444981](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028202444981.png)

## SpringBoot项目健康检查与打包jar

![image-20211028203023027](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028203023027.png)

![image-20211028203051572](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211028203051572.png)

**routeFunctions**

**reactive的flux或者mono是异步处理**

