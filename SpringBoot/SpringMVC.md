控制器的 模型是dao,service, 视图是jsp，底层是Servlet:转发，重定向

![image-20211114215828102](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211114215828102.png)

## 回顾servlet

![image-20211117170110335](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211117170110335.png)

学MVC要回顾tomcat  servlet

**MVC框架要做哪些事情**

1. 将url映射到java类或java类的方法 .
2. 封装用户提交的数据 .
3. 处理请求--调用相关的业务处理--封装响应数据 .
4. 将响应的数据进行渲染 . jsp / html 等表示层数据 .
5. 常见的服务器端MVC框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端MVC框架：**vue、angularjs、react**、backbone；由MVC演化出了另外一些模式如：MVP、MVVM（ModelAndView) 等等....

![image-20211117170341805](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211117170341805.png)

![image-20211116133146285](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116133146285.png)

![image-20211116133525779](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116133525779.png)

1. DispatcherServlet视图解析器是什么：视图解析器是用来**接收经过处理器适配器调用具体的controller后生成的逻辑视图的**，它接受 DispatcherServlet传过来的ModelAndView，然后**将ModelAndView数据填充到相应的视图中**，然后**返回一个带有数据的视图再传给DispatcherServlet.**
   视图解析器的处理流程：**调用目标方法**，SpringMVC将目标方法返回的String、View、ModelMap或是ModelAndView都**转换为一个ModelAndView对象**；通过视图解析器（**ViewResolver**）对ModelAndView对象**中的View对象进行解析**，**将该逻辑视图View对象解析为一个物理视图View对象**；最后**调用物理视图View对象的render()方法进行视图渲染**，得到响应结果。

![image-20211116143210002](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116143210002.png)

前端控制器：DispatcherServlet

模型装数据，视图负责跳转

![image-20211116143424919](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116143424919.png)

![image-20211116143719620](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116143719620.png)

![image-20211116143845777](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116143845777.png)

![image-20211116144629926](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211116144629926.png)

## 使用注解开发SpringMVC

@Controller

网页要实现视图的复用

@RequestMapping



## RestFul风格

restful是一种**软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件**。它主要用于**客户端和服务器交互类**的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

REST，即**Representational State Transfer**的缩写。直接翻译的意思是"表现层状态转化"。它是一种互联网应用程序的API设计理念：**URL定位资源，用HTTP动词（GET,POST,DELETE,DETC）描述操作**。

**REST 的基本原理包括：**系统上的一切对象都要抽象为资源**；每个资源对应唯一的资源标识（URI）；对资源的操作不能改变资源标识（URI）本身**；所有的操作都是无状态的等等。URI，是uniform resource identifier，**统一资源标识符**，用来唯一的标识一个资源。URL是uniform resource locator，**统一资源定位器**，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源。

由于近些年互联网不断发展，各种各样的电子设备层出不穷，为了方便不同设备前后端交互通信，就需要有一种统一的机制，即RESTful，通过统一的接口为不同设备提供服务。如果我们的客户端需要对服务器上的这个资源进行操作，就需要**通过http协议执行相应的动作来操作它**，比如进行获取，更新，删除。

状态的转移：访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。**互联网通信协议HTTP协议**，是一个无状态协议。这意味着，所有的资源状态都保存在服务器端。因此，**如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。**

**@PathVariable**

```java
@Controller
public class RestfulController {
    @RequestMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a, @PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}
```

![image-20211117101708081](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211117101708081.png)

## 重定向和转发

1. 结果跳转方式：设置**ModelAndView**对象 , 根据view的名称 , 和视图解析器跳到指定的页面**页面 : {视图解析器前缀} + viewName +{视图解析器后缀}**
2. 通过设置ServletAPI , 不需要视图解析器，通过**HttpServletResponse**进行输出，实现重定向和转发



## 什么是JSON

前后端分离，后端部署后端，提供接口；前段独立部署Layui,负责渲染

前后端要建立连接，约定用JSON，是一种数据交换格式

- JSON(**JavaScript Object Notation**, JS 对象标记) 是一种**轻量级的数据交换格式**，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 **JavaScript 语言中，一切都是对象**。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 是 JavaScript 对象的字符串表示法**，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

**JSON 和 JavaScript 对象互转**

要实现**从JSON字符串转换为JavaScript 对象，使用 JSON.parse()** 方法：

```html
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现**从JavaScript 对象转换为JSON字符串，使用 JSON.stringify()** 方法：

```
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```

## Jackson使用

![image-20211117144021561](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211117144021561.png)



## ssmbuild编写

1.要把Books这个参数删了，为什么

![image-20211119180303592](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211119180303592.png)

2.这些类型的选择

![image-20211119194245957](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211119194245957.png)

3.他俩的区别  bean id, class的区别

ref 引用一个已经存在的对象  value 创建一个新的对象

![image-20211119201734607](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211119201734607.png)

4. 为什么他在mvc.xml中

```xml
<context:component-scan base-package="com.du.controller"/>
```



## Ajax

**AJAX**即“**Asynchronous JavaScript and XML**”（异步的[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)与[XML](https://zh.wikipedia.org/wiki/XML)技术），指的是一套综合了多项技术的[浏览器](https://zh.wikipedia.org/wiki/瀏覽器)端[网页](https://zh.wikipedia.org/wiki/網頁)开发技术。

传统的Web应用允许**用户端填写表单**（form），当提交表单时就向[网页服务器](https://zh.wikipedia.org/wiki/網頁伺服器)发送一个请求。**服务器接收并处理传来的表单，然后送回一个新的网页，但这个做法浪费了许多带宽**，因为在前后两个页面中的大部分[HTML](https://zh.wikipedia.org/wiki/HTML)码往往是相同的。由于每次应用的沟通都需要向服务器发送请求，**应用的回应时间依赖于服务器的回应时间**。这导致了用户界面的回应比本机应用慢得多。

与此不同，AJAX应用可以仅向服务器发送并取回必须的数据，并在客户端采用JavaScript处理来自服务器的回应。因为在服务器和浏览器之间交换的数据大量减少，服务器回应更快了。同时，**很多的处理工作可以在发出请求的[客户端](https://zh.wikipedia.org/wiki/客户端)机器上完成**，因此Web服务器的负荷也减少了。

类似于[DHTML](https://zh.wikipedia.org/wiki/DHTML)或[LAMP](https://zh.wikipedia.org/wiki/LAMP)，AJAX不是指一种单一的技术，而是有机地利用了一系列相关的技术。虽然其名称包含XML，但实际上数据格式可以由[JSON](https://zh.wikipedia.org/wiki/JSON)代替以进一步减少数据量。而客户端与服务器也并不需要异步。一些基于AJAX的“派生／合成”式（derivative/composite）的技术也正在出现，如[AFLAX](https://zh.wikipedia.org/wiki/AFLAX)。

![image-20211120214742274](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211120214742274.png)

![image-20211120215100509](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211120215100509.png)

applicationContext.xml的三个要素：**注解驱动，静态资源过滤，视图解析器，**

JQuery地址  JQuery的符号是$

```jsp
<script src="https://code.jquery.com/jquery-3.6.0.js" integrity="sha256-H+K7U5CnXl1h5ywQfKtSj8PCmoN9aaq30gDh27Xc0jk=" crossorigin="anonymous"></script>
```

从后端取数据与从表单取数据

![image-20211121131225272](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211121131225272.png)

![image-20211121134238118](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211121134238118.png)

status： 200成功， 300+重定向或者转发， 400+客户端错误， 500+服务器错误





## 登陆判断验证

1.在WEB-INF下的所有页面或者资源只能通过controller/servlet进行访问

![image-20211122105355550](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211122105355550.png)

2.Caused by: java.lang.IllegalStateException: Ambiguous mapping. Cannot map 'userController' method 

![image-20211123100904402](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211123100904402.png)

![image-20211123105346396](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211123105346396.png)

**/是一个符号， /*是过滤一层，/**是过滤所有**
