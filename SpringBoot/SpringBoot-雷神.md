# SpringBoot

## SpringBoot-HelloWorld

web应用需要添加依赖

@SpingBootApplication注解，标记这是一个SpingBoot的应用

Controller层需要@RequestMapping(“/hello”)来映射请求，**@ResponseBody**来回应映射

`@RequestMapping`是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径；用于方法上，表示在类的父路径下追加方法上注解中的地址将会访问到该方法。例如，

```java
// 用于类上，可以没有
@RequestMapping(value = "/controllerDemo")
public class ControllerDemo {
	// 用于方法上，必须有
    @RequestMapping(value = "/methodDemo")
    public String methodDemo() {
        return "helloWorld";
    }
}

```

其对应的相对请求路径就是`controllerDemo/methodDemo`，访问该路径就会跳转到`helloWorld`页面。

- @Responsebody注解表示该方法的返回的结果直接写入 HTTP 响应正文中，一般在异步获取数据时使用；
- 在使用@RequestMapping后，返回值通常解析为跳转路径，加上@Responsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP 响应正文中。例如，异步获取json数据，加上@Responsebody注解后，就会直接返回json数据。
- @RequestBody注解则是将 HTTP 求正文插入方法中，使用适合的HttpMessageConverter将请求体写入某个对象。

------------------------------------------------
但是！ **@RestController=@Controller+@ResponseBody**,仅用@RestController即可

![image-20211104213203390](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211104213203390.png)

## SpringBoot-依赖管理

![image-20211104213800559](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211104213800559.png)

**spring-boot-starter-*场景启动器**

## SpringBoot-自动配置

spring-boot-starter-web自动配好了tomcat, springmvc, 以及他们的常用组件，等等

默认的包结构：主程序所在的包及其下面的子包里面的配件都会被扫描进来

## 底层注解-@Configuration

![image-20211106144035287](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211106144035287.png)

![image-20211123214743308](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211123214743308.png)

![image-20211123220235593](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211123220235593.png)

无得话会报错: Exception in thread "main" org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'tom' available

![image-20211124105551380](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124105551380.png)

```java
proxyBeanMethods = true
//proxyBeanMethods配置类是用来指定@Bean注解标注的方法是否使用代理，默认是true使用代理，直接从IOC容器之中取得对象；如果设置为false,也就是不使用注解，每次调用@Bean标注的方法获取到的对象和IOC容器中的都不一样，是一个新的对象，所以我们可以将此属性设置为false来提高性能, 解决组件依赖问题
//true:检查组件是否在容器中， false则跳过这个步骤。springboot启动更快
@Configuration(proxyBeanMethods = true) //这个注解相当于beans.xml
public class MyConfig {
    @Bean //给容器增加组件，方法名为id，返回类型为组件类型，返回的值就是组件在容器中的实例
    public User user01(){
        return new User("zhangsan",18);
//        zhangsan.setPet(tomcatPet());
    }
    @Bean
    public Pet tom(){
        return new Pet("tom");
    }
}

```

![image-20211124110138791](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124110138791.png)



## 底层注解-@Import导入组件

![image-20211124111419623](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124111419623.png)



## 底层注解-@Conditional条件装配

![image-20211124111935174](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124111935174.png)



## 底层注解-@ImportResource导入Spring配置文件

```java
@ImportResource("classpath:beans.xml")

public class MyConfig {}
```



## 底层注解-@ConfigurationProperties配置绑定

![image-20211124113411703](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124113411703.png)

或者不用Component

![image-20211124123711367](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124123711367.png)



## 自动配置-自动包规则原理





## 自动配置-初始自动加载类



![image-20211124135259898](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124135259898.png)

**pom中加devtools依赖，编译用ctr**+F9: **Restart**

**JRebel: Reload**



这是为何？

![image-20211124152036891](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124152036891.png)
