# Spring

## Spring简介

1. **Spring框架**是 [Java](https://zh.wikipedia.org/wiki/Java) 平台的一个[开源](https://zh.wikipedia.org/wiki/开放源代码)的全栈（[full-stack](https://zh.wikipedia.org/wiki/全栈)）[应用程序框架](https://zh.wikipedia.org/wiki/软件框架)和[控制反转](https://zh.wikipedia.org/wiki/控制反转)容器实现，一般被直接称为 Spring。该框架的一些核心功能理论上可用于任何 Java 应用，但 Spring 还为基于[Java企业版](https://zh.wikipedia.org/wiki/Jakarta_EE)平台构建的 Web 应用提供了大量的拓展支持。Spring 没有直接实现任何的编程模型，但它已经在 Java 社区中广为流行，基本上完全代替了[企业级JavaBeans](https://zh.wikipedia.org/wiki/EJB)（EJB）模型
2. SSH与SSM
3. Interface21
4. 控制反转（IOC，Inverse Of Control），即把创建对象的权利交给框架，也就是指将对象的创建、对象的存储、对象的管理交给了Spring容器。Spring容器是Spring中的一个核心模块，用于管理对象，底层可以理解为是一个Map集合。
5. 面向切面编程（Aspect-Oriented Programming, AOP） 就是将那些与业务无关，却为业务模块所共同调用的逻辑或责任分开封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性。
6. **Spring Data**实现了对数据访问接口的统一，支持多种数据库访问框架或组件（如：JDBC、[Hibernate](https://zh.wikipedia.org/wiki/Hibernate)、[MyBatis](https://zh.wikipedia.org/wiki/MyBatis)（[iBatis](https://zh.wikipedia.org/wiki/IBATIS)））作为最终数据访问的实现。
7. 事务管理
8. MVC：模型-视图-控制器
9. 远程访问：约定大于配置 Spring Boot
10. 批处理
11. 集成框架
12. 总结：Spring就是一个轻量级的控制反转IOC和面向切面编程AOP的框架



## Spring组成及扩展

1. Spring七大模块
   ![image-20211109212946443](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211109212946443.png)

2. build：构建一切  coordinate：协调一切  connect：连接一切



## IOC理论推导（Inversion of Control)

导spring-webmvc的maven包

**service是业务层，dao是数据访问层**。在service里直接调用dao，service里面就new一个dao类对象。标准主流现在的编程方式都是采用MVC综合设计模式，MVC本身不属于设计模式的一种，它描述的是一种结构，最终目的达到解耦，**解耦说的意思是你更改某一层代码，不会影响其他层代码**，如果你会像spring这样的框架，你会了解面向接口编程，**表示层调用控制层，控制层调用业务层，业务层调用数据访问层。

**初期也许都是new对象去调用下一层，比如你在业务层new一个DAO类的对象，调用DAO类方法访问数据库，这样写是不对的，因为在业务层中是不应该含有具体对象，最多只能有引用，如果有具体对象存在，就耦合了。****后期是@Autowired直接注入**  当那个对象不存在，我还要修改业务的代码，这不符合逻辑。好比主板上内存坏了，我换内存，没必要连主板一起换。我不用知道内存是哪家生产，不用知道多大容量，只要是内存都可以插上这个接口使用。这就是MVC的意义。

1. UaserDao**接口**
   
2. UserDaoImpl**实现类**
   **重复拼写大量代码实现同一个**

3. 通过set进行动态实现值的注入,可以根据用户选择，避免重复修改，**使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象** 这就是**控制反转**？系统的耦合性大大降低

   ```Java
   //通过set进行动态实现值的注入
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
   
   
   public class MyTest {
       public static void main(String[] args) {
           UserService userService = new UserServiceImpl();
   //        userService.getUser();
           ((UserServiceImpl) userService).setUserDao(new UserDaoImpl());
           userService.getUser();
       }
   }
   ```

   

   ![image-20211110161555106](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110161555106.png)

   ![image-20211110161939282](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110161939282.png)

4. UserService**业务接口**
   
5. UserServiceImpl**业务实现**



## IOC本质

![image-20211110163137651](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110163137651.png)

1. 控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![image-20211110163648698](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110163648698.png)

2. IoC是Spring框架的核心内容，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![image-20211110163759917](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110163759917.png)

3. 采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。

## Hello Spring

1. **beans 是容器？**

```xml
<!--使用spring来创建对象，在spring这些都成为Bean
	类型 变量名 = new 类型（）;
	Hello hello = new Hello();
	id = 变量名       class = new的对象
	property = 给对象的属性设置	-->
    <bean id="hello" class="com.du.pojo.Hello">
        <property name="str" value="String"/>
    </bean>
```

2. **IOC就是对象由spring创建，管理，配置**



## IOC创建对象方式

1. 使用无参创建对象

```java
public User() {
        System.out.println("User的无参构造");
    }
<bean id="user" class="com.du.pojo.User">
        <property name="name" value="DU"/>
    </bean>

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        User user = (User) context.getBean("user");
        user.show();
    }
}
```

2. 使用有参构造创建对象

   第一种：通过下标赋值

   ```xml
    <bean id="user" class="com.du.pojo.User">
           <constructor-arg index="0" value="DU"/>
       </bean>
   index=0,在user中是string name的下标
   ```

   第二种：type,**若是有两个string类型的参数则无法区分**

   ```xml
   <bean id="user" class="com.du.pojo.User">
           <constructor-arg type="java.lang.String" value="DU"/>
       </bean>
   ```

   第三种：直接通过参数名

   ```xml
   <bean id="user" class="com.du.pojo.User">
           <constructor-arg name="name" value="Du"/>
       </bean>
   ```

   **spring在创建bean时已经实例化了？**



## Spring配置

1. 别名Alias
   <alias name="user" alias="userNew"/>

   ![image-20211110193057603](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110193057603.png)

2. Bean的配置
   id: bean的唯一标识符
   class： bean对象对应的全限定名
   name：别名，可以同时取多个别名

   ```xml
   <bean id="user"  class="com.du.pojo.User" name="user2,u2">
   <!--        <constructor-arg index="0" value="DU"/>-->
   <!--        <constructor-arg type="java.lang.String" value="DU"/>-->
           <constructor-arg name="name"  value="Du"/>
       </bean>
   ```

3. import
   用于团队开发,可以将多个配置文件合并为一个

   ```xml
   import resource = "beans.xml"/>
   import resource = "beans2.xml"/>
   ```

   

## DI依赖注入环境(Dependency Injection)

1. 构造器注入
   在构造方法上注入

   ```java
   @Controller
   public class FooController {  
       private final FooService fooService;  
       @Autowired
       public FooController(FooService fooService) {
           this.fooService = fooService;
       }
   }
   ```
   
2. **set方式注入**
   依赖注入：在set方法上注入
   **依赖： bean对象的创建依赖于容器**

   **注入：bean对象中的所有属性，由容器注入**

   ```java
   @Controller
   public class FooController {
       private FooService fooService;
       @Autowired
       public void setFooService(FooService fooService) {
           this.fooService = fooService;
       }
   }
   ```
   
3. 其他方式



## 依赖注入之set注入

1. ![image-20211110203439049](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110203439049.png)

![image-20211110203618939](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110203618939.png)

![image-20211110203654593](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110203654593.png)

![image-20211110203828422](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110203828422.png)



## c命名和p命名空间注入

1. p就是property,可以直接注入属性的值

   ```xml
   <bean id="user" class="com.du.pojo.User" p:name="du" p:age="20"/>
   ```

2. c就是constructor-arg

   没有在user构造函数，constructor-args需要有参构造，p需要无参构造，系统默认自带无参构造，所以c不报错

   ![image-20211110214019977](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110214019977.png)

## Bean的作用域 bean scopes

![image-20211110214325732](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211110214325732.png)

1. **单例模式 scope=“singleton” spring的默认**
2. 原型模式：每次从容器get都会从容器产生新对象



## 自动装配Bean

上方都为手动装配bean

1. 自动装配是Spring满足bean依赖的一种方式

2. Spring会在上下文中自动寻找，并自动给bean装配属性

3. spring的三种装配方式： 在xml现实的配置   在java中装配   隐式的自动装配Autowired
   **环境搭建：**一个人，两个宠物，宠物会叫

   ```xml
   <bean id="cat" class="com.du.pojo.Cat"/>
       <bean id="dog" class="com.du.pojo.Dog"/>
       <bean id="people" class="com.du.pojo.People" >
           <property name="name" value="金金"/>
           <property name="cat" ref="cat"/>
           <property name="dog" ref="dog"/>
       </bean>
   ```

   byName自动注入，会自动在容器上下文查找和自己对象set方法后面的值对应的beanid

   ```xml
   <bean id="cat" class="com.du.pojo.Cat"/>
       <bean id="dog" class="com.du.pojo.Dog"/>
       <bean id="people" class="com.du.pojo.People" autowire="byName">
           <property name="name" value="金金"/>
   <!--        <property name="cat" ref="cat"/>-->
   <!--        <property name="dog" ref="dog"/>-->
       </bean>
   ```

   ![image-20211111191350921](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211111191350921.png)

   byType自动注入，会自动在容器上下文查找和自己对象属性类型相同的bean

   ```xml
   <bean  class="com.du.pojo.Cat"/>
       <bean  class="com.du.pojo.Dog"/>
       <bean id="people" class="com.du.pojo.People" autowire="byType">
           <property name="name" value="金金"/>
       </bean>
   ```



## 注解实现自动装配

1. 导入约束：context   配置注解

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/aop
          https://www.springframework.org/schema/context/spring-aop.xsd">
   
          <bean id="cat" class="com.du.pojo.Cat"/>
          <bean id="dog" class="com.du.pojo.Dog"/>
          <bean id="people" class="com.du.pojo.People" autowire="byType"/>
   
   </beans>
   ```

2. **注解是用反射实现的，所以实体类只需get方法，不需set方法？**我不是这样啊，set

   @Autowired

   ```java
   @Autowired
   private Cat cat;
   ```

   @Autowired(required=null)

   @Qulifier(value=“dog”),可以配合@Autowired使用，指定唯一的注解
   @Resource注解,指定内容

   ```
   @Resource(name="dog")
   private Dog dog;
   ```

![image-20211111203011052](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211111203011052.png)

## Spring注解开发

在Spring4之后，要使用注解开发就必须要保证aop的包导入了（spring-webmvc包含了），使用注解要导入context约束，增加注解的支持

1. bean

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/aop
          https://www.springframework.org/schema/context/spring-aop.xsd">
   
          <context:component-scan base-package="com.du.pojo"/>
          <context:annotation-config/>
          <bean scope="prototype"/>
   </beans>
   ```

   

2. 属性如何注入

```java
@Component
public class User {

    public String name = "du";

}//等于
@Component
public class User {
    @Value("du")
    public String name;

}//等于
@Component
public class User {
    public String name;
    @Value("du")
    public void setName(String name) {
        this.name = name;
    }
}
```

3. 衍生的注解
   @Conponent有几个衍生注解，在web开发时按照mvc三层架构分层
   - dao: @Repository
   - service: @Service
   - controller: @Controller
   - 这四个注解功能都是一样的，都是代表将某个类注册到Spring中装配Bean

4. 自动装配配置

   ```
   @Autowired: 通过类型、名字自动装配，如果@Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value=",,,")
   @Nullable: 字段标记了这个注解，说明这个字段可以为null
   @Resource: 通过名字、类型自动装配
   ```

5. 作用域 scope

   ```xml
    <bean scope="prototype"/>  原型模式
   
   @Component
   @Scope("prototype")
   public class User {
   }
   ```

6. 小结
   ![image-20211111212550878](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211111212550878.png)

## 使用JavaConfig实现配置

```java
public class MyTest {
    public static void main(String[] args) {
       ApplicationContext context = new AnnotationConfigApplicationContext(myConfig.class);
        User getUser = (User) context.getBean("getUser");
        System.out.println(getUser.getName());
    }
}

@Component//说明这个类被spring接管了，注入到容器中
public class User {
    private String name;
    public String getName() {
        return name;
    }
    @Value("du")
    public void setName(String name) {
        this.name = name;
    }
}

@Configuration//代表这是一个配置类
@ConponentScan("com.du.pojo")//可以扫描这个地址的包
@Import(myConfig2.class) //将两个配置类引为一个
public class myConfig {
    @Bean
    public User getUser(){
       return new User();
    }
}
```

**所有的类都需要装配到bean里，所有的bean都需要容器去取**，**容器中取得的就是一个对象，可以直接使用**



## 代理模式

1. **为什么学代理模式**：这是SpringAOP的底层
2. 代理模式的分类：静态代理和动态代理，相当于中介，不同的中介不同功能
3. 静态代理
   角色分析：
   抽象角色：一般会使用接口或者抽象类来解决
   真实角色：被代理的角色
   代理角色：代理真实角色，代理真实角色后，会做一些附属操作
   客户：访问代理角色



## 静态代理再理解

**一个真实角色会产生一个代理角色，会导致开发效率低，但是耦合性降低**

用set实现spring

proxySetUserService(UserService);

proxy.query();

![image-20211112204543505](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211112204543505.png)



## 动态代理详解

**Proxy:代理，  InvocationHandler:调用处理程序**

1. **动态代理的底层都是反射，反射可以动态的加载一些类**
2. 动态代理和静态代理角色一样
3. 动态代理分为两大类：基于接口：JDK； 基于类：cglib； java字节码实现：javasist
4. proxy--> InvocationHandler-->invoke
   ![image-20211112210540794](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211112210540794.png)

![image-20211112215527904](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211112215527904.png)



## AOP

1. 什么是AOP：
   Aspect Oriented Programming, 面向切面编程，通过[预编译](https://baike.baidu.com/item/预编译/3191547)方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是[OOP](https://baike.baidu.com/item/OOP)的延续，是软件开发中的一个热点，也是[Spring](https://baike.baidu.com/item/Spring)框架中的一个重要内容，是[函数式编程](https://baike.baidu.com/item/函数式编程/4035031)的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的[耦合度](https://baike.baidu.com/item/耦合度/2603938)降低，提高程序的可重用性，同时提高了开发的效率。
   OOP（[面向对象编程](https://baike.baidu.com/item/面向对象编程)）针对业务处理过程的实体及其属性和行为进行抽象封装，以获得更加清晰高效的[逻辑单元](https://baike.baidu.com/item/逻辑单元)划分。
   AOP则是针对业务处理过程中的切面进行提取，它所面对的是处理过程中的某个步骤或阶段，以获得逻辑过程中各部分之间低[耦合性](https://baike.baidu.com/item/耦合性)的隔离效果。这两种设计思想在目标上有着本质的差异

![image-20211113110639084](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211113110639084.png)

## AOP方法实现

1. 使用原生SpringAPI

   ```xml
   <!--注册bean-->
       <bean id="userService" class="com.du.service.UserServiceImpl"/>
       <bean id="log" class="com.du.log.Log"/>
       <bean id="afterLog" class="com.du.log.AfterLog"/>
   <!--方式一：使用原生SpringAPI-->
   <!--    配置aop,需要导入aop的约束-->
       <aop:config>
   <!--        切入点： expression表达式-->
           <aop:pointcut id="pointcut" expression="execution(* com.du.service.UserServiceImpl.*(..))"/>
   <!--执行环绕增加-->
           <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
           <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
       </aop:config>
   ```

   ```java
   public class Log implements MethodBeforeAdvice {
       @Override
       //method:要执行的目标对象的方法   args:参数   target:目标对象
       public void before(Method method, Object[] args, Object target) throws Throwable {
           System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
       }
   }
   
   public class AfterLog implements AfterReturningAdvice {
       @Override
       //returnValue:返回值
       public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
           System.out.println("执行了"+method.getName()+"方法，返回结果为："+returnValue);
       }
   }
   ```

2. 自定义

   ```xml
   <!--方式二：自定义类-->
       <bean id="diy" class="com.du.diy.DiyPointCut"/>
       <aop:config>
           <aop:aspect ref="diy">
               <aop:pointcut id="point" expression="execution(* com.du.service.UserServiceImpl.*(..))"/>
               <aop:before method="before" pointcut-ref="point"/>
               <aop:after method="after" pointcut-ref="point"/>
           </aop:aspect>
       </aop:config>
   
   ```

   ```java
   public class DiyPointCut {
       public void before(){
           System.out.println("=======方法执行前=======");
       }
       public void after(){
           System.out.println("=======方法执行后=======");
       }
   }
   ```



## 注解实现AOP

```xml
<!--    方式三：注解实现-->
    <bean id="annotationPointCut" class="com.du.diy.AnnotationPointCut"/>
<!--    开启注解支持-->
    <aop:aspectj-autoproxy/>
```

```java
@Aspect //标记这个类是一个切面
public class AnnotationPointCut {
    @Before("execution(* com.du.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("======方法执行前======");
    }
    @After("execution(* com.du.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("======方法执行后======");
    }
    @Around("execution(* com.du.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("======环绕前======");
        Object proceed = joinPoint.proceed();
        System.out.println("======环绕后======");
        System.out.println(proceed);
    }
}
```

## 回顾mybatis

步骤：导入相关jar包(junit, mybatis, mysql, spring, aop, spring-mybatis)   配置相关文件，测试

**编写实体类， 编写核心配置文件， 编写接口， 编写Mapper.xml， 测试**

1.编写实体类

```java
@Data
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

2.编写核心配置文件

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    <properties resource="db.properties"/>

    <typeAliases>
        <typeAlias type="com.du.pojo.User" alias="User"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper class="com.du.mapper.UserMapper"/>
    </mappers>
</configuration>
```

```xml
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:33060/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username=root
password=DST773344
```

3. 编写接口

```java
public interface UserMapper {
    public List<User> queryUser();
}

```

4.编写Mapper.xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--核心配置文件-->
<mapper namespace="com.du.mapper.UserMapper">

    <select id="queryUser" resultType="user">
        select * from user
    </select>
</mapper>
```

5.测试

```java
public class MyTest {
    @Test
    public void test() throws IOException {
        String resources = "mybatis-config.xml";
        InputStream resourceAsStream = Resources.getResourceAsStream(resources);
        //自动提交为true
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sessionFactory.openSession(true);
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.queryUser();
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```



## 整合Mybatis-spring一

**mybatis-spring首先需要配DataSource和SqlSessionFactory，之后SqlSessionFactory这类的代码再也不需要写了**

1. DataSource:使用Spring的数据源替换Mybatis的配置
2.  sqlSessionFactory
3. SqlSessionTemplate:就是我们使用的sqlSession
4. 注入类

```xml
<!--DataSource:使用Spring的数据源替换Mybatis的配置 c3p0 dbcp druid
    这里使用Spring 提供的JDBC：org.springframework.jdbc.datasource-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:33060/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
    </bean>
<!--    sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
<!--        绑定mybatis配置文件-->
        <property name="configuration" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/du/mapper/*.xml"/>
    </bean>
<!--    SqlSessionTemplate:就是我们使用的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
<!--        只能使用构造器注入sqlSessionFactory, 因为他没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
<!--    注入类-->
    <bean id="userMapper" class="com.du.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
```

```xml
当出现Mapped Statements collection already contains value for。。。。。（意思大概：xxx 在statements 执行语句的集合中已经存在了。。。（塑料英语，大佬轻拍））

这种报错时

错误原因有：
1.mapper中存在id重复的值
2.mapper中的parameterType或resultType为空。
例如：<select id="findProductById" parameterType=" "> </select>
 
3. 在使用@Select等注解的情况下，方法名即为mapper的id，重载的方法会报这个错。
4. mapper复制  忘了改namespace指向的类，所以两个mapper指向同一个mapper，所以报了这个错。
5.sqlMapperConfig.xml与application-dao.xml（文件名不一定相同，我在这个文件中主要配置的是，sqlSessionFactory放入IOC容器和扫描mapper包下的文件，并将mapper接口的实现类放入容器）中
例如：
 <mappers>
  　　<package name="org.blog.dao"/>
 </mappers>
和
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <property name="basePackage" value="org.blog.dao"></property>
 </bean>
重复了，删掉一个就可以了。。。。
```

![image-20211114173025442](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211114173025442.png)



## 整合Mybatis方式二

![image-20211114185043884](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211114185043884.png)

## 声明式事务

1. 回顾事务

   要么都成功，要么都失败，需要确保事务的完整性和一致性

   ACID原则：原子性，一致性，隔离性，持久性



## Spring中的事务管理

一共有两类：声明式事务AOP应用、编程式事务

![image-20211114195543662](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211114195543662.png)

将这几个改为Java config
