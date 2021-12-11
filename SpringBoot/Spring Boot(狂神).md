# Spring Boot

![image-20211017140858479](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017140858479.png)

**最核心：自动装配原理**

![image-20211017141258733](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017141258733.png)

![image-20211017141432774](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017141432774.png)

![image-20211017141536343](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017141536343.png)

![image-20211017142109771](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017142109771.png)

## 1. 什么是Spring

- 轻量级框架
- 简化复杂性企业级开发

- **约定大于配置**（Maven, Spring MVC 等）

![image-20211017142406705](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017142406705.png)

## 2. 微服务架构（看论文）

- Dubbo
- 单体应用框架（all in one），将一个应用中的所有应用服务都封装在一个应用中
- 高内聚，低耦合

![image-20211017144319324](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017144319324.png)

## 3. 第一个Spring Boot程序

![image-20211017145546312](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017145546312.png)

### 自动配置：pom.xml

```xml
<!--        启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

```java
@RestController
@RequestMapping("/abc")
public class HelloController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello(){
        return "hello DU";
    }
}
//需要这样访问：http://localhost:7399/abc/hello，

```

![image-20211017154010511](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017154010511.png)

这个是Spring boot banner, 在resources底下加一个banner,txt即可修改

### 启动器 starter

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

- 启动器就是SpringBoot的启动场景
- spring-boot-starter-web可以自动导入web环境所有的依赖
- spring-boot会将所有的功能场景，变成一个个启动器
- 要使用什么功能，找到对应的starter即可：starter-web

### 主程序

```java
@SpringBootApplication//标注这是一个springboot的应用
public class Springboot01HellowordApplication {

    public static void main(String[] args) {
        //将springboot应用启动
        SpringApplication.run(Springboot01HellowordApplication.class, args);
    }

}
```

**自动配置原理分析：**

![image-20211017161037198](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017161037198.png)

结论：SpringBoot所有的自动配置都是在启动时扫描并加载，但是不一定生效，要导入对应的start启动器，自动装配生效，配置成功

![image-20211017161302852](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017161302852.png)

**主启动类怎么应用** run方法

```
SpringApplication.run(Springboot01HellowordApplication.class, args);
```

```
@SpringBootApplication//标注这是一个springboot的应用
//也可以写到其他类中，将其改为springboot应用
```

### yaml语法及其给实体类赋值

### JSR303校验

核心：@Pattern 正则表达式

@Validated：数据校验

![image-20211017170710398](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017170710398.png)

多环境配置集文件的位置：yaml文件可以在多个路径下

![image-20211017192712917](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017192712917.png)

同时存在时，不同位置的Yml文件的优先级不同

![image-20211017192849526](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017192849526.png)

yml文件可以实现多环境配置，通过---进行分割

![image-20211017193311392](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017193311392.png)

spring:

​	profiles:

​		active: dev

### @Configuration中的proxyBeanMethods属性详解

Full(proxyBeanMethods = true) :proxyBeanMethods参数设置为true时即为：Full 全模式。 该模式下注入容器中的同一个组件无论被取出多少次都是同一个bean实例，即单实例对象，在该模式下SpringBoot每次启动都会判断检查容器中是否存在该组件
Lite(proxyBeanMethods = false) :proxyBeanMethods参数设置为false时即为：Lite 轻量级模式。该模式下注入容器中的同一个组件无论被取出多少次都是不同的bean实例，即多实例对象，在该模式下SpringBoot每次启动会跳过检查容器中是否存在该组件
什么时候用Full全模式，什么时候用Lite轻量级模式？
当在你的同一个Configuration配置类中，注入到容器中的bean实例之间有依赖关系时，建议使用Full全模式
当在你的同一个Configuration配置类中，注入到容器中的bean实例之间没有依赖关系时，建议使用Lite轻量级模式，以提高springboot的启动速度和性能

Full 全模式，proxyBeanMethods默认是true：使用代理，也就是说该配置类会被代理，直接从IOC容器之中取得bean对象，不会创建新的对象。SpringBoot总会检查这个组件是否在容器中是否存在，保持组件的单实例
Lite 轻量级模式，proxyBeanMethods设置为false：每次调用@Bean标注的方法获取到的对象是一个新的bean对象,和之前从IOC容器中获取的不一样，SpringBoot会跳过检查这个组件是否在容器中是否存在，保持组件的多实例

------------------------------------------------
![image-20211017203907470](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017203907470.png)

没有自动配置，导入以来的starter启动即可



## Spring Boot Web开发

jar: webapp,  自动装配

- xxxxAutoConfiguration:向容器中自动配置组件
- xxxxProperties:自动配置类，装配配置文件中自定义的一些内容

要解决的问题：

- 导入静态资源

```java
 public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
                this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
              registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                    if (this.servletContext != null) {
                        ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                        registration.addResourceLocations(new Resource[]{resource});
                    }

                });
            }
        }
```

什么是webjars

http://localhost:7399/webjars/jquery/3.4.1/jquery.js  访问下面的源码

需要向pom里添加webjars的maven启动才可以访问，可以拿到web资源

![image-20211017213732002](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211017213732002.png)

这个路径下的文件都可以直接读取，比如localhost:7399/1.js

优先级：resource>static>public，oublic一般放公共配置资源

- 首页

- jsp，模板引擎Thymeleaf
- 装配扩展SpringMVC
- 增删改查
- 拦截器
- 国际化！

### 首页定制

getIndexHtml

### 模板引擎

```
//在templates目录下的所有页面，只能通过controller来跳转,这个需要模板引擎的支持：thymeleaf
@Controller
public class IndexController {

    @RequestMapping("/test")
    public String index(){
        return "test";
    }
}
```

![image-20211018123436424](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211018123436424.png)

```
@Controller
public class IndexController {

    @RequestMapping("/test")
    public String index(Model model){
        model.addAttribute("msg","hello springboot");
        return "test";
    }
}

<h1>Test</h1>
<!--s所有的html元素都可以被thymleaf替换接管， th:-->
<h2 th:text="${msg}"></h2>
</body>
</html>
```

**Thymeleaf语法**

![image-20211018124808840](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211018124808840.png)

![image-20211018125652618](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211018125652618.png)

```
@Controller
public class IndexController {

    @RequestMapping("/test")
    public String index(Model model){
        model.addAttribute("msg","hello springboot");
        model.addAttribute("users", Arrays.asList("qwer","asff"));
        return "test";

    }
}

<h3 th:each="user:${users}" th:text="${user}"></h3>
```

### 扩展视图解析器

```
//扩展 springnvc  dispatchservlet
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    //ViewResolver 实现了视图解析器接口的类，我们就可以将其看作视图解析器
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }

    public static class MyViewResolver implements ViewResolver{
        //自定义一个自己的视图解析器MyViewResolver,并注册到Bean里就会自动装配上
        @Override
        public View resolveViewName(String viewName, Locale locale) throws Exception{
            return null;
        }
    }

}
```

```
//扩展Spring MVC, 视图跳转
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addRedirectViewController("/kuang","test");
}
```

**此时视图跳转，虽然输入为，localhost:7399/kuang，但是仍是访问/test网页  ？**

![image-20211018132959507](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211018132959507.png)

```
//@EnableWebMvc,不能轻易使用，会崩， condition no on
```

学会写starter文件，configration+properties打包jar包放到autoconfiguration下



## 第一个程序

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Department {

    private Integer id;
    private String departmentName;
}

public class DepartmenrDao {

    //模拟数据库的数据
    private static Map<Integer, Department> departments = null;

    static {
        departments = new HashMap<Integer,Department>();

        departments.put(101,new Department(101,"教学部"));
        departments.put(102,new Department(102,"市场部"));
        departments.put(103,new Department(103,"教研部"));
        departments.put(104,new Department(104,"运营部"));
        departments.put(105,new Department(105,"后勤部"));

    }

    //获得所有部门信息
    public Collection<Department> getDepartment(){
        return departments.values();
    }
    //通过id获得部门
    public Department gerDepartmentById(Integer id){
        return departments.get(id);
    }
}
```

```JAVA
@Data
@NoArgsConstructor
public class Employee {
    private Integer id;
    private String name;
    private String email;
    private Integer gender; // 0:female; 1:male
    private Department department;
    private Date birth;


    public Employee(Integer id, String name, String email, Integer gender, Department department) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.gender = gender;
        this.department = department;
        this.birth = birth;
        //默认日期
        this.birth = new Date();
    }
}

public class EmployeeDao {
    //模拟数据库的数据
    private static Map<Integer, Employee> employees = null;

    @Autowired
    private DepartmenrDao departmenrDao;

    static {
        employees = new HashMap<Integer,Employee>();

        employees.put(1001,new Employee(1001,"AM","12324@qq.com",0,new Department(101,"教学部")));
        employees.put(1002,new Employee(1002,"WE","12124@qq.com",1,new Department(103,"教研部")));
        employees.put(1003,new Employee(1003,"QW","15324@qq.com",1,new Department(101,"教学部")));
        employees.put(1004,new Employee(1004,"ZX","17324@qq.com",0,new Department(103,"教研部")));
        employees.put(1005,new Employee(1005,"FG","12624@qq.com",1,new Department(103,"教研部")));

    }

    //增删改查
    //主键自增
    private static Integer initId = 1006;
    //增加一个员工
    public void save(Employee employee){
        if (employee.getId()==null){
            employee.setId(initId++);
        }
        employee.setDepartment(departmenrDao.gerDepartmentById((employee.getDepartment().getId())));

        employees.put(employee.getId(),employee);
    }
    //查询全部员工
    public Collection<Employee> getAll(){
        return employees.values();
    }
    public Employee getEmployeeById(Integer id){
        return employees.get(id);
    }
    public void delete(Integer id){
        employees.remove(id);
    }


}
```

### 页面国际化

要确保yml或者property是UTF-8

![image-20211018210710724](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211018210710724.png)

映射不到静态资源的问题

将路径改为：

```
mvc:
  static-path-pattern: /static/**
```

```
<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
<input type="text" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
<input type="password" class="form-control" th:placeholder="#{login.password}" required="">
```

```
<input type="text" class="form-control" placeholder="Username" required="" autofocus="">
<label class="sr-only" th:text="#{login.password}"> Password</label>
<input type="password" class="form-control" placeholder="Password" required="">
```

这两个的区别，访问不到里面所以修改为上面的，是什么原理？

```java
<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}"> English</a>

public class MylocaleResolver implements LocaleResolver {
    //解析请求
    @Override //需要放到MvcConfig的beanli才能生效
    public Locale resolveLocale(HttpServletRequest request) {
        //获取请求中的语言参数
        String langauge = request.getParameter("l");
        Locale locale = Locale.getDefault();
        //如果请求带了国际化的参数
        if (StringUtils.hasLength(langauge)){
            //zh_CN
            String[] split = langauge.split("_");
            //国家，地区
            locale = new Locale(split[0],split[1]);

        }
        return locale;
    }
}
/*
    addViewControllers可以方便的实现一个请求直接映射成视图，而无需书写controller
    registry.addViewController("请求路径").setViewName("请求页面文件路径")
     */
```

![image-20211019162657952](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211019162657952.png)

Resolved [org.springframework.web.bind.MissingServletRequestParameterException: Required List parameter ‘customerNoList’ is not present]
原因：请求体过大，超出了tomcat限制的2M大小
解决方案：使用的是springboot内嵌的tomcat，通过在application.properties 或者 bootstrap.yaml 中修改配置
------------------------------------------------
```yml
server:
  tomcat:
  	# -1 表示不限制请求大小
  	# 单位是字节 byte
    max-http-form-post-size: -1
```

```
@ResponseBody
//Annotation that indicates a method return value should be bound to the web response body. Supported for annotated handler methods.
```

s所以报错

![image-20211019165132404](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211019165132404.png)

![](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211019165141476.png)

![image-20211019200245154](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211019200245154.png)



@Controller注解很重要，加上这个，底下的mapping才生效，将东西存放到@Bean里，然后再取出来



```
<div th:insert="~{dashboard::sidebar}"></div>

<!--				抽取页面？ 侧边栏-->
				<nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar">
```

![image-20211019210113696](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211019210113696.png)

```
<html lang="en" xmlns:th="http://www.thymelaf.org">
thymeleaf的命名空间
```

TemplateInputException: An error happened during template parsing (template: "class path resource [templates/dashboard.html]")] with root cause

![image-20211020112547408](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211020112547408.png)

```
<div th:insert="~{common/commons::topbar}"></div>
```

地址要写对

![image-20211020150233629](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211020150233629.png)

咋写啊，因为dashboardcontroller只是起跳转作用，所以无需写dao和@Autowired注入

```html
<div class="container-fluid">
   <div class="row">
      <div th:insert="~{common/commons::sidebar(active='list.html')}"></div>
      <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
         <h2>员工列表</h2>
         <div class="table-responsive">
            <table class="table table-striped table-sm">
               <thead>
                  <tr>
                     <th>id</th>
                     <th>name</th>
                     <th>email</th>
                     <th>gender</th>
                     <th>department</th>
                     <th>birth</th>
                     <th>操作</th>
                  </tr>
               </thead>
               <tbody>
                  <tr th:each="emp:${emps}">
                     <td th:text="${emp.getId()}"></td>
                     <td th:text="${emp.getName()}"></td>
                     <td th:text="${emp.getEmail()}"></td>
                     <td th:text="${emp.getGender()==0?'女':'男'}"></td>
                     <td th:text="${emp.department.getDepartmentName()}"></td>
                     <td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
                     <td>
                        <button class="btn btn-sm btn-primary">编辑</button>
                        <button class="btn btn-sm btn-danger">删除</button>
                     </td>
                  </tr>
               </tbody>
            </table>
         </div>
      </main>
   </div>
</div>
```

```html
<select class="form-control" name="department">
   <option th:each="dept:${department}" th:text="${dept.getDepartmentName()}" th:value="${dept.getId()}"></option>
</select>
```

name="department" 必须要有 

```java
@PostMapping("/emp")
public String toAddpage(Model model){
    //查出所有部门的信息
    Collection<Department> department = departmenrDao.getDepartment();
    model.addAttribute("department",department);
    return "emp/add";
}

<form th:action="@{/emp}" method="post">//通过postf
```

![image-20211020202512353](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211020202512353.png)

Mapping的地址一定要写，否则Resolved [org.springframework.web.HttpRequestMethodNotSupportedException: Request method ‘POST’ not supported]

![image-20211020203035146](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211020203035146.png)

多种日期格式可不可以？

```
<!--                         在controller接收的是一个Employee,所以需要提交其中一个属性，如dept.getId()-->
                        <option th:each="dept:${department}" th:text="${dept.getDepartmentName()}" th:value="${dept.getId()}"></option>
```

![image-20211020204123106](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211020204123106.png)

a是标签， button是按钮，有啥区别

![image-20211020205822107](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211020205822107.png)

```
<a class="btn btn-sm btn-primary" th:href="@{/emp/(emp.id)}">编辑</a>
```

语法@{/emp/(emp.id)}



![image-20211021111736446](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211021111736446.png)

![image-20211021131848418](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211021131848418.png)

![image-20211021132755014](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211021132755014.png)

![image-20211021143414176](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211021143414176.png)

![image-20211021152442442](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211021152442442.png)

## Spring访问mybatis

```java
@RestController
public class JDBCController {

    @Autowired
    JdbcTemplate jdbcTemplate;

    //查询数据库的所有信息

    //没有实体类怎么获取数据库中的东西， 万能MAP
    @GetMapping("/userList")
    public List<Map<String,Object>> userList(){
        String sql = "select * from user";
        List<Map<String,Object>> list_maps = jdbcTemplate.queryForList(sql);
        return list_maps;
    }

}
```

```yaml
spring:
  datasource:
    username: root
    password: DST773344
    url: jdbc:mysql://localhost:33060/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    
    
```

```java
//JDBC整合
@RestController
public class JDBCController {

    @Autowired
    JdbcTemplate jdbcTemplate;


//不需要传值，直接增加到mysql，所以不需要传到userlist
@GetMapping("/updUser/{id}")
public String updUser(@PathVariable("id") int id){
    String sql = "update mybatis.user set name =?,pwd=? where id="+id;
    //封装
    Object[] objects = new Object[2];
    objects[0]="小红";
    objects[1]="DST";
    jdbcTemplate.update(sql,objects);
    return "updUser-ok";
}
}
```

```
#SpringBoot默认是不注入这些的，需要自己绑定
#druid数据源专有配置
initialSize: 5
minIdle: 5
maxActive: 20
maxWait: 60000
timeBetweenEvictionRunsMillis: 60000
minEvictableIdleTimeMillis: 300000
validationQuery: SELECT 1 FROM DUAL
testWhileIdle: true
testOnBorrow: false
testOnReturn: false
poolPreparedStatements: true

#配置监控统计拦截的filters，stat：监控统计、log4j：日志记录、wall：防御sql注入
#如果允许报错，java.lang.ClassNotFoundException: org.apache.Log4j.Properity
#则导入log4j 依赖就行
filters: stat,wall,log4j
maxPoolPreparedStatementPerConnectionSize: 20
useGlobalDataSourceStat: true
connectionoProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

![image-20211021205820407](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211021205820407.png)

### mybatis整合

```yml
#整合mybatis
mybatis:
  type-aliases-package: com.du.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
```

![image-20211022150955806](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211022150955806.png)

MVC:

​	M: model，数据和业务； V：HTML；C：controller,交接

## Spring Security环境搭建

安全的框架，shiro, springsecurity

功能：认证、授权

![image-20211023130405684](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023130405684.png)

![image-20211023130804235](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211023130804235.png)

