

# Mybatis3

环境：JDK, Mysql, maven, IDEA

基础：JDBC, Mysql, Java基础, Maven, Junit

SSM框架：配置文件看官网

## 1.简介

### 1.1 什么是mybatis

-  MyBatis 是一款优秀的持久层框架 
-  它支持自定义 SQL、存储过程以及高级映射 
-  中文文档：MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作 
-  MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
- github 找mybatis
- maven仓库找mybatis的jar包



### 1.2持久化

- 数据持久化：将程序的数据在持久状态和瞬时状态转化的过程
- 内存：断电即失
- 数据库（jdbc），io文件持久化
- 因为需要持久化而持久化（内存贵，某些东西需要长久保存）

### 1.3持久层

#### MVC模式

 MVC：Model、View、Controller 

- 完成持久化工作的代码块
- 层界限十分明显

![1633607840122](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633607840122.png)

- Service层:服务层，负责一些业务处理，比如说：获取数据库连接，关闭数据库连接，事务回滚或者一些复杂的逻辑业务处理
- Dao层：数据访问层，(Database Access Object)  负责访问数据库，对数据的操作，获取结果集，将结果集中的数据装到OV（Object Value）对象中，之后再返回给Service层
-  Controller层：业务层，主要功能是处理用户的请求 
-  View层：主要负责显示数据（Html、Css、jQuery等等） 
- 调用顺序： View>Controller>Service>Dao，如果上层代码对下层代码的依赖程度过高，就需要对每层的代码定义一个(标准)接口。 



### 1.4为什么要mybatis

- 方便
- 帮助程序员将数据存到数据库中
- 简化框架，自动化
- 不用mybatis也可以

- 优点

## 2.第一个mybatis程序

思路：搭建环境--导入mybatis--编写代码--测试

- MybatisUtils.java
- 到resources里配置文件mybatis-config.xml
- 实体类User.java(pojo)
- 编写接口UserDao.java
- maven: UserMapper.xml
- 测试：UserDaoTest.java
- pom.xml配置

### 2.1搭建环境

1. 搭建数据库

```java
CREATE DATABASE `mybatis`;
USE `mybatis`;

create table `user`(
	`id` int(20) not null auto_increment,
	`name` VARCHAR(30) DEFAULT null,
	`pwd` VARCHAR(30) DEFAULT null,
	primary key(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

insert into `user`(`name`,`pwd`) VALUES
('kuang','123456'),
('shen','123456'),
('meng','123456')
```

2. 新建项目

   - 新建一个maven项目
   - 删除src，使之成为辅工程
   - 导入maven依赖

   ```java
   <!--导入依赖-->
       <!--mysql驱动-->
       <dependencies>
       <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.26</version>
       </dependency>
       <!--mybatis驱动-->
       <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.7</version>
       </dependency>
   
       <!--junit-->
       <!-- https://mvnrepository.com/artifact/junit/junit -->
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.13.2</version>
           <scope>test</scope>
       </dependency>
       </dependencies>
   
   
   ```

### 2.2创建一个模块

- 编写mybatis的核心配置文件

```java
public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory ;

    static{
        try{
            //使用mybatis第一步：获取sqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    //sqlsseion完全包含了面向数据库执行SQL命令所需的所有方法
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }

}

```



- 编写工具类

### 2.3 编写代码

- 实体类

```java
package src.main.java.com.kuang.pojo;

public class User {
    private int id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```



- dao层
- Dao接口

```java
public interface UserDao {
    List<User> getUserList();
}
```

- 接口实现类由原来的UserDaoImpl转变为一个Mapper配置文件

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="org.mybatis.example.BlogMapper">
<!--select语句-->
    <select id="getUserList" resultType="com.kuang.pojo.User" >
        select * from mybatis.user
    </select>
</mapper>
```

### 2.4 测试

注意点：

org.apache.ibatis.binding.BindingException: Type interface src.main.java.com.kuang.dao.UserDao is not known to the MapperRegistry.

```java
<!--每一个Mapper.xml都需要在mybatis核心配置文件中注册-->
    <mappers>
        <mapper resource="com/kuang/dao/UserMapper.xml"/>
    </mappers>
```

可能遇到的问题

- 配置文件没有注册
- 绑定接口错误
- 方法名不对
- 返回类型不对
- Maven导出资源问题

## 3.CRUD

### 3.1 namespace

- namespace中的包要与接口的包名一致

### 3.2 select

选择，查询语句：

- id：对应的namespace中的方法名
- resultType：Sql语句执行的返回值
- parameterType: 参数类型

```java
package com.kuang.dao;

import com.kuang.pojo.User;

import java.util.List;

public interface UserDao {
    //查询全部用户
    List<User> getUserList();
    //根据id查询用户
    User getUserById(int id);
    //insert
    Integer addUser(User user);

    Integer updateUser(User user);

    Integer deleteUser(int user);
}
```

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.kuang.dao.UserDao">
<!--select语句-->
    <select id="getUserList" resultType="com.kuang.pojo.User" >
        select * from mybatis.user
    </select>

    <select id="getUserById" parameterType="int" resultType="com.kuang.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>
    
    <insert id="addUser">
        insert into mybatis.user(id, name, pwd) values (#{id},#{name},#{pwd})
    </insert>

    <update id="updateUser">
        update mybatis.user
        set name = #{name},pwd = #{pwd}
        where id = #{id};
    </update>

    <delete id="deleteUser">
        delete from mybatis.user where id = #{id}
    </delete>
</mapper>
```

```java
public class UserDaoTest {

    @Test
    public void test(){
        //第一步：获得sqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        try{
            //执行SQL
            UserDao userDao = sqlSession.getMapper(UserDao.class);
            List<User> userlist = userDao.getUserList();
            for (User user : userlist){
                System.out.println(user);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            sqlSession.close();
        }


    }
    @Test
    public void getUserById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserDao dao = sqlSession.getMapper(UserDao.class);

        User user = dao.getUserById(1);
        System.out.println(user);



        sqlSession.close();
    }
    @Test
    //增删改需要提交事务
    public void addUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserDao dao = sqlSession.getMapper(UserDao.class);

        Integer res = dao.addUser(new User(179010 ,"csfr","2345"));
        System.out.println(res);
        if (res != null && res == 1){
            System.out.println("successfully insert!");
        }
        //提交事务
        sqlSession.commit();

        sqlSession.close();

    }
```

### 3.3 分析错误

- 标签不能匹配错
- resource 绑定mapper,需要使用路径
- 程序配置文件必须符合规范
- NullPointerException, 没有注册到资源
- 输出的xml文件中存在中文乱码（JBK, UTF-8）
- maven资源没有导出 

### 3.4 万能MAP

参数过多情况下使用map

map传递参数，直接在sql取出key即可(parameterType=“map”)

对象传递参数，直接在sql中取对象的属性即可(parameterType=“Object”)

只有一个基本类型参数的情况下，可以直接在sql中取到

```java
int addUser(Map<String,Object> map);

 <insert id="addUser" parameterType="map"> 
        insert into mybatis.user(id, name, pwd) values (#{userid},#{name},#{pwd}) //这后面的数据类型就可以自定义了
    </insert>
//不需要知道数据的具体类型
原来：
    int addUser(User user);
```

多个参数用Map或者注解

### 3.5 模糊查询

模糊查询怎么写？

1. java代码执行的时候，传递通配符 % %

   > List<User> userlist = mapper.getUserLike(“%李%”)

2. 在sql拼接中使用通配符

   > select * from mybatis.user where name like “%”#{value}“%”



## 4 配置解析

### 4.1 核心配置文件

- mybatis-config.xml
-  MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下： 

```xml
configuration（配置）:
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
	environment（环境变量）
		transactionManager（事务管理器）
		dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
```

- 属性 properties  可以在外部进行配置，并可以进行动态替换 
- 动态配置

```xml
mybatis-config.xml:
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>


db.properties:
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:33060/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username=root
password=DST773344
```

### 4.4 类型别名(alias)

-  类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。 

```xml
<typeAliases>
        <typeAlias type="com.kuang.pojo.User" alias="User"/>
    </typeAliases>

<select id="getUserList" resultType="User" >
        select * from mybatis.user
    </select>
```

- 指定包，再写包里的名字   MyBatis 会在包名下面搜索需要的 Java Bean 

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

### 4.5 设置

-  这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。 下表描述了设置中各项设置的含义、默认值等。 

| cacheEnabled             | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true \| false | true  |
| ------------------------ | ------------------------------------------------------------ | ------------- | ----- |
| mapUnderscoreToCamelCase | 是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。 | true \| false | False |

| logImpl | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |
| ------- | ----------------------------------------------------- | ------------------------------------------------------------ | ------ |
|         |                                                       |                                                              |        |

### 4.6 其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)

### 4.7 Mappers

使用相对于类路径的资源引用，或完全限定资源定位符（包括 `file:///` 形式的 URL），或类名和包名等。 

- Mapper Registry：注册绑定mapper文件

  方式一：优先使用，最便捷

  ```xml
  <!--每一个Mapper.xml>都需要在mybatis核心配置文件中注册-->
  <mappers>
    <mapper resource="com/kuang/dao/UserMapper.xml"/>
  </mappers>
  ```
  
  方式二：在class文件绑定注册
  
  ```xml
  <!--每一个Mapper.xml>都需要在mybatis核心配置文件中注册-->
  <mappers>
      <mapper class="com/kuang/dao/UserMapper"/>
  </mappers>
  ```
  
  注意点：
  
  - 接口和它的Mapper配置文件必须同名
  - 接口和他的Mapper配置文件必须在同一个包下

### 4.8 生命周期和作用域

 不同作用域和生命周期类别是至关重要的，因为错误的使用会导致非常严重的并发问题。 

![1633786389866](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633786389866.png)

SqlSessionFactoryBuilder:

- 一旦创建SqlSessionFactory，就不再需要
- 局部变量

SqlSessionFactory：

- 相当于数据库连接池
- SqlSessionFactory以及后期运行一直需要，不能被丢弃和重建
- 最佳作用域是全局应用域
- 单例模式，静态单例模式

SqlSession：

- 是连接到连接池的一个请求
- SqlSession不是线程安全的，所以不能被共享，最佳作用域是请求或方法作用域
- 用完之后立即关闭，否则占用资源

![1633786917216](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633786917216.png)

## 5  解决属性名和字段名不一致的问题

新近一个项目，拷贝之前的，测试实体类字段不一致的情况

### 5.1起别名

![1633848936020](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633848936020.png)

![1633848955873](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633848955873.png)

### 5.2 结果集映射 ResultMap

![1633832461549](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633832461549.png)

```xml
    <resultMap id="UserMap" type="User">
        <result column="pwd" property="password"/>
    </resultMap>

    <select id="getUserById" resultMap="UserMap">
        select * from mybatis.user where id = #{id}
    </select>
```

-  `resultMap` 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBC `ResultSets` 数据提取代码中解放出来 
-  ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。 



## 6 日志

### 6.1 日志工厂

数据库出错，用日志工厂排错

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j
- JDK logging
-  SLF4J | LOG4J | LOG4J2 | JDK_LOGGING | COMMONS_LOGGING | STDOUT_LOGGING | NO_LOGGING 

maybatis-config.xml

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING "/>
</settings>
```

在框架中这种错误一般是没有导包，配置错误

![1633833763890](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633833763890.png)

### 6.3 Log4j

1. 什么是Log4j

-  Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件 
-  可以控制每一条日志的输出格式 
-  通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程 
-  可以通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码。 

2. log4j使用

- 导入log4j的maven jar包:pom.xml  
-  == mybatis-config.xml

<settings>    <setting name="logImpl" value="Log4j"/></settings>

- 建log4j.properties的配置文件

```xml
 <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>

#将等级为debug级别的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,CONSOLE,file
# My logging configuration...
#log4j.logger.com.tocersoft.school=DEBUG
#log4j.logger.net.sf.hibernate.cache=debug
## Console output...
##控制台输出的相关设置
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p %d %C: %m%n


#文件输出的相关设置
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=src/main/resources/logs/b.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss}  %l  %m%n

log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResutSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
log4j.logger.org.apache.activemq.spring=WARN

#see cache info
log4j.logger.org.hibernate.cache=info
```

2. 期间出现bug internal，是console没有大写
3.  **ERROR Could not find value for key log4j.appender.Console** ：

```
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
```

4. 无法生成log文件，路径问题

```
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=src/main/resources/logs/b.log
```

2. 简单使用

- 在要使用log4j的类中导包

- 日志对象，参数为当前类的class

  ```java
  import org.apache.log4j.Logger;
  
  stastic Logger logger = Logger.getLogger(UserDaoTest.class)
  
  //输出
      sout(sqlSession);
  = logger.debug();
  ```

- 日志级别

```java
@Test
    public void testLog4j(){
        logger.info("info: 进入了testLog4j");
        logger.debug("debug: 进入了testLog4j");
        logger.error("error: 进入了testLog4j");
    }
```

## 7 分页

### 7.1 为什么要分页

- 减少数据的处理量

### 7.2 使用mybatis分页，核心是sql,

- ```sql
  --语法
  select * from user limit startindex, pagesize;
  ```

  接口， mapper.xml,  测试

```xml
List<User> getUserByLimit(Map<String,Integer> map);

<select id="getUserByLimit" parameterType="Map" resultType="user">
        select * from mybatis.user limit #{startIndex},#{pageSize}
    </select>
    
    @Test
    public void getUserByLimit(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        HashMap<String, Integer> map = new HashMap<String, Integer>();
        map.put("startIndex",0);
        map.put("pageSize",2);

        List<User> userList =mapper.getUserByLimit(map);
        for (User user: userList){
            System.out.println(user);
        }

    }
```

分页插件：PageHelper

## 8 使用注解开发

### 8.1 什么是面向接口编程

**根本原因：解耦，可扩展，提高复用，分层开发中，上层不用管具体的实现，大家遵守共同的标准，开发变得容易，规范性更好。**

 在一个[面向对象](https://baike.baidu.com/item/面向对象)的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己不重要， 各个对象之间的协作关系则成为系统设计的关键 。

接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

接口的本身反映了系统设计人员对系统的抽象理解。

**关于接口的理解**

接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

接口的本身反映了系统设计人员对系统的抽象理解。

接口应有两类：第一类是对一个<u>个体的抽象</u>，它可对应为一个抽象体(abstract class)；

第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）

[**面向对象](https://baike.baidu.com/item/面向对象)是指，我们考虑问题时，以对象为单位，考虑它的属性及方法

**[面向过程](https://baike.baidu.com/item/面向过程)是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现**

 接口设计与非接口设计是针对复用技术而言的， 与面向对象（过程）不是一个问题，更多体现为对系统整体的架构

### 8.2 使用注解开发

- 注解在接口上实现

```java
 @Select("select * from user")
    List<User> getUsers();
```

- 需要在核心配置文件绑定接口

```xml
<mappers>
        <mapper class="com.kuang.dao.UserMapper"/>
    </mappers>
```

- 测试

```java
public class UserDaoTest {
    @Test
    public void test(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //底层
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.getUsers();
        for (User user : users){
            System.out.println(user);
        }
        sqlSession.close();
    }

}
```

### 8.3 使用注解CRUD

可以在工具类自动提交

```java

    //sqlsseion完全包含了面向数据库执行SQL命令所需的所有方法
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession(true);
    }

//方法存在多个参数，所有参数必须添加注解@param
    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id, @Param("name") String name);

//方法存在多个参数，所有参数必须添加注解@param
    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id);

//一定要在xml绑定接口
@Test
    public void getUserById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        User userById = mapper.getUserById(1);
        System.out.println(userById);

        sqlSession.close();
    }
```

```java
has an unsupported return type//bug
    
    public void addUser()//原
    public int addUser()//修改后
    return 
```

**@Param注解**

- 基本类型的参数或者String类型都需要加上
- 引用类型不需要加
- 在SQL中引用的就是@Param()中设定的属性名

#{}可以预编译

### Lombook

 Lombok 是一种 Java 实用工具，可用来帮助开发人员消除 Java 的冗长，尤其是对于简单的 Java 对象（POJO）。它通过注释实现这一目的。通过在开发环境中实现  Lombok，开发人员可以节省构建诸如 hashCode() 和 equals() 这样的方法以及以往用来分类各种 accessor 和 mutator 的大量时间。 

常用注解：

@Data 注解在类上；提供类所有属性的 getting 和 setting 方法，此外还提供了equals、canEqual、hashCode、toString 方法
@Setter ：注解在属性上；为属性提供 setting 方法
@Setter ：注解在属性上；为属性提供 getting 方法
@Log4j ：注解在类上；为类提供一个 属性名为log 的 log4j 日志对象
@NoArgsConstructor ：注解在类上；为类提供一个无参的构造方法
@AllArgsConstructor ：注解在类上；为类提供一个全参的构造方法
@Cleanup : 可以关闭流
@Builder ： 被注解的类加个构造者模式
@Synchronized ： 加个同步锁
@SneakyThrows : 等同于try/catch 捕获异常
@NonNull : 如果给参数加个这个注解 参数为null会抛出空指针异常
@Value : 注解和@Data类似，区别在于它会把所有成员变量默认定义为private final修饰，并且不会生成set方法。

使用： 安装插件、在pom导入jar包，注解使用

```java
import lombok.Data;

@Data
public class User {
    private int id;
    private String name;
    private String pwd;
}
```



## 10 复杂查询，多对一，一对多

首先在数据库建教师和学生的表， tid

```java
//首先建接口
public interface StudentMapper {
}
public interface TeacherMapper {

    @Select("select * from teacher where id = #{tid}")
    Teacher getTeacher(@Param("tid") int id);
}
//mapper.xml配置
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--核心配置文件-->
<mapper namespace="com.kuang.dao.StudentMapper">

</mapper>
    
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--核心配置文件-->
<mapper namespace="com.kuang.dao.TeacherMapper">

</mapper>
    
//config配置
  <mappers>
        <mapper class="com.kuang.dao.TeacherMapper"/>
        <mapper class="com.kuang.dao.StudentMapper"/>
    </mappers>
            
//test
public class MyTest {
    public static void main(String[] args) {

        SqlSession sqlSession = MybatisUtils.getSqlSession();
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);
        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher);

        sqlSession.close();

    }
}            
```

- 出了个bug

```java
Cause: org.apache.ibatis.builder.BuilderException: Error parsing SQL Mapper Configuration. Cause: org.apache.ibatis.builder.BuilderException: Error parsing Mapper XML. The XML location is 'com/kuang/dao/StudentMapper.xml'. Cause: org.apache.ibatis.builder.BuilderException: Error resolving class. Cause: org.apache.ibatis.type.TypeException: Could not resolve type alias 'Student'.  Cause: java.lang.ClassNotFoundException: Cannot find class: Student
	at org.apache.ibatis.exceptions.ExceptionFactory.wrapException(ExceptionFactory.java:30)
```

修改：

```java
<select id="getStudent" resultType="Student">

        select * from student;
    </select>
        
//改为
<select id="getStudent" resultType="com.kuang.pojo.Student">
        select * from mybatis.student;
    </select>        
//花了一个多小时才改好    
//期间将resultType改为list等其他操作，就像套娃一样报错     
```

### 10.2 一对多

报错

```java
org.apache.ibatis.exceptions.PersistenceException: 
### Error querying database.  Cause: java.lang.IndexOutOfBoundsException: Index: 2, Size: 2
### The error may exist in com/kuang/dao/TeacherMapper.xml
### The error may involve com.kuang.dao.TeacherMapper.getTeacher
### The error occurred while handling results
### SQL: select * from mybatis.teacher;
### Cause: java.lang.IndexOutOfBoundsException: Index: 2, Size: 2
```

在实体类加个无参构造即可，不懂为啥

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Teacher {
    private int id;
    private String name;
    private List<Student> students;
}
```

又出现了错误

```java
Mapped Statements collection already contains value for
```

![1634040464091](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1634040464091.png)

![1634040479987](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1634040479987.png)

有两个getTeacher,重了，注释原先的即可

此时，将id通过@Param起别名为tid

需要将mapper中的t.id=#{id}改为t.id=#{tid}才可正常执行

**子查询学的太晕，用嵌套查询进行来联表查询**



**按照结果嵌套查询**

```xml
<!--按结果嵌套查询-->
    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid, s.name sname, t.name tname, t.id tid
        from mybatis.student s,mybatis.teacher t
        where s.tid=t.id and t.id = #{tid}
    </select>

    <resultMap id="TeacherStudent" type="com.kuang.pojo.Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
<!--复杂的属性需要单独处理， 对象：association 集合：collection javaType=""指定属性的类型，集合中的泛型信息要用ofType获取-->
        <collection property="students" ofType="com.kuang.pojo.Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>

```

按照查询嵌套处理

```xml
<select id="getTeacher2" resultMap="TeacherStudent2">
        select * from mybatis.teacher where id = #{tid}
    </select>
    <resultMap id="TeacherStudent2" type="com.kuang.pojo.Teacher">
        <collection property="students" javaType="ArrayList" ofType="com.kuang.pojo.Student" select="getStudentByTeacherId" column="id"/>

    </resultMap>
    <select id="getStudentByTeacherId" resultType="com.kuang.pojo.Student">
        select * from mybatis.student where tid = #{tid}
    </select>
```

小结：

association:一对多

collection:多对一

![1634042216095](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1634042216095.png)

**区分javaType和ofType的区别**

**Mysql引擎、InnoDB底层原理、索引、索引优化**

## 12 动态SQL: if choose trim foreach

**动态SQL就是指根据不同的条件生成不同的SQL语句**

```java
//环境配置
 <mappers>
        <mapper class="com.du.dao.BlogMapper"/>
    </mappers>
//实体类参数            
@Data
public class Blog {
    private String id;
    private String title;
    private String author;
    private Date create_time;
    private int views;

    public void setCreateTime(Date date) {
    }
}            
//接口
public interface BlogMapper {

    int addBlog(Blog blog);
}
//mapper配置
<!--核心配置文件-->
<mapper namespace="com.du.dao.BlogMapper">

    <insert id="addBlog" parameterType="com.du.pojo.Blog">
        insert into mybatis.blog(id, title, author, create_time, views)
        VALUES (#{id},#{title},#{author},#{create_time},#{views});
    </insert>


</mapper>
//getID
public class IdUtils {
    public static String getId(){
        return UUID.randomUUID().toString().replaceAll("-"," ");
    }

    @Test
    public void test() {
        System.out.println(IdUtils.getId());
    }
} 
//插入参数
public class MyTest {
    @Test
    public void addBlogTest() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        Blog blog = new Blog();
        blog.setId(IdUtils.getId());
        blog.setTitle("Mybatis");
        blog.setAuthor("狂神说");
        blog.setCreate_time(new Date());
        blog.setViews(9999);

        mapper.addBlog(blog);

        blog.setId(IdUtils.getId());
        blog.setTitle("Java");
        mapper.addBlog(blog);

        blog.setId(IdUtils.getId());
        blog.setTitle("Spring");
        mapper.addBlog(blog);

        blog.setId(IdUtils.getId());
        blog.setTitle("微服务");
        mapper.addBlog(blog);

        sqlSession.commit();

        sqlSession.close();
    }
}
```

```java
//if
HashMap map = new HashMap();
        map.put("Title","Java");
        map.put("author","狂神说");
<select id="queryBlogIf" parameterType="map" resultType="com.du.pojo.Blog">
        select * from mybatis.blog
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author != author">
            and author = #{author}
        </if>
    </select>
//并没有实现单个查询   
            
//因为map里的Title里的首字母大写，xml里的文件小写，无法传参，不能进行过滤，就全部查出 了
    map.put("Title","Java");
<if test="title != null">
             title = #{title}
        </if>
```

choose

```java
<select id="queryBlogChoose" parameterType="map" resultType="com.du.pojo.Blog">
        select * from mybatis.blog
            <where>
            <choose>
                <when test="title != null">  //不满足第一个就看第二个条件
                    title = #{title}
                </when>
                <when test="author != null">
                    and author = #{author}
                </when>
                <otherwise>
                    views = #{views}
                </otherwise>
            </choose>
            </where>
    </select>
                    
                    
    @Test
    public void queryBlogChoose(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
        HashMap map = new HashMap();
        map.put("views","1000 ");

        List<Blog> blogs = mapper.queryBlogChoose(map);
        for (Blog blog : blogs){
            System.out.println(blog);
        }
        sqlSession.commit();

        sqlSession.close();
    }                    
```

### SQL片段,可以实现代码的复用

![1634106063258](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1634106063258.png)

- 使用SQL标签抽取公共的部分
- 在需要使用的地方使用include标签引用即可

![1634106138941](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1634106138941.png)

### foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT * FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```

![1634110073025](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1634110073025.png)

where A and B,当只有一个条件的时候不需要open和close来连接，删掉

## 13 缓存

只有读可以缓存，写不可以缓存



## 14 总结

**流程**

1. Resources 获取加载全局配置文件
2. 实例化SqlSessionFactoryBuilder构造器
3. 解析配置文件流XMLConfigBuilder
4. Configuration所有的配置信息
5. SqlSessionFactory实例化
6. transactional事务管理
7. 创建executor执行器
8. 创建sqlSession
9. 实现CRUD
10. 查看是否执行成功
11. 提交事务
12. 关闭

### 代码生成器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="application-dev.yml"/>



    <context id="MysqlContext" targetRuntime="MyBatis3" defaultModelType="flat">
        <!-- 配置pojo的序列化 -->
        <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin" />
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:33060/rems"
                        userId="root"
                        password="DST773344">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="com.du.pojo"
                            targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="com.du.mapper"
                         targetProject="src/main/resources">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.du.mapper"
                             targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <!-- 指定数据库表 -->
        <table tableName="record"/>
        <table tableName="account"/>
        <table tableName="user"/>

    </context>
</generatorConfiguration>
```

