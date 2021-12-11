# Mybatis-Plus

## 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑

- **损耗小**：启动即会**自动注入基本 CURD**，性能基本无损耗，直接面向对象操作

- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求

- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错

- ```java
  public class Java8Tester {
     public static void main(String args[]){
        Java8Tester tester = new Java8Tester();
          
        // 类型声明
        MathOperation addition = (int a, int b) -> a + b;
          
        // 不用类型声明
        MathOperation subtraction = (a, b) -> a - b;
          
        // 大括号中的返回语句
        MathOperation multiplication = (int a, int b) -> { return a * b; };
          
        // 没有大括号及返回语句
        MathOperation division = (int a, int b) -> a / b;
          
        System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
        System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
        System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
        System.out.println("10 / 5 = " + tester.operate(10, 5, division));
          
        // 不用括号
        GreetingService greetService1 = message ->
        System.out.println("Hello " + message);
          
        // 用括号
        GreetingService greetService2 = (message) ->
        System.out.println("Hello " + message);
          
        greetService1.sayMessage("Runoob");
        greetService2.sayMessage("Google");
     }
     interface MathOperation {
        int operation(int a, int b);
     }
      
     interface GreetingService {
        void sayMessage(String message);
     }
      
     private int operate(int a, int b, MathOperation mathOperation){
        return mathOperation.operation(a, b);
     }
  }
  ```
  
  
  
- **支持主键自动生成**：支持多达 **4 种主键策略**（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题

|    值     |                             描述                             |
| :-------: | :----------------------------------------------------------: |
|   AUTO    |                         数据库ID自增                         |
|   NONE    | 无状态,该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT) |
|   INPUT   |                    insert前自行set主键值                     |
| ASSIGN_ID | 分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextId`(默认实现类为`DefaultIdentifierGenerator`雪花算法) |

- **支持 Active Record 模式**：支持 Active Record 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用

```java
// 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中
public class CodeGenerator {
    /**
     * 数据源配置
     */
    private static final DataSourceConfig.Builder DATA_SOURCE_CONFIG = new DataSourceConfig
            .Builder("jdbc","rootname","password");


    /**
     * 执行 run
     */
    public static void main(String[] args) throws SQLException {
        FastAutoGenerator.create(DATA_SOURCE_CONFIG)
                // 全局配置
                .globalConfig(builder ->
                        builder.author("Du425")
                                .fileOverride()
                                .enableSwagger()
                                .dateType(DateType.TIME_PACK)
                                .commentDate("yyyy-MM-dd")
                                .outputDir("D://java-code/REMS/src/main/java"))
                // 包配置
                .packageConfig(builder ->
                        builder.parent("com.du.rems")
                                .service("service")
                                .serviceImpl("service.impl")
                                .mapper("mapper")
                                .controller("controller")
                                .other("other")
                                .pathInfo(Collections.singletonMap(OutputFile.mapperXml, "D://java-code/REMS/src/main/resources/mapper")))
                // 策略配置
                .strategyConfig(builder -> builder.addInclude("account","record","user"))

//

                .execute();
    }

}
```

- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询

```java
@Configuration
public class MybatisPlusConfig {
    /**
     *   mybatis-plus分页插件
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor page = new PaginationInterceptor();
        page.setDialectType("mysql");
        return page;
    }

}
```



- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 配置好对数据表进行增删改查

配置： 添加依赖、配置yml、 在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹、 编写实体类 `User.java` 、编写Mapper类 `UserMapper.java`

添加测试类：

```java
@SpringBootTest
public class SampleTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelect() {
        System.out.println(("----- selectAll method test ------"));
        List<User> userList = userMapper.selectList(null);
        Assert.assertEquals(5, userList.size());
        userList.forEach(System.out::println);
    }

}
```

![image-20211204131930324](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204131930324.png)

所谓`utf8_unicode_ci`，其实是用来**排序的规则**。对于mysql中那些字符类型的列，如`VARCHAR`，`CHAR`，`TEXT`类型的列，都需要有一个`COLLATE`类型来告知mysql如何对该列进行排序和比较。简而言之，**COLLATE会影响到ORDER BY语句的顺序，会影响到WHERE条件中大于小于号筛选出来的结果，会影响**`**DISTINCT**`**、**`**GROUP BY**`**、**`**HAVING**`**语句的查询结果**。另外，mysql建索引的时候，如果索引列是字符类型，也**会影响索引创建**，只不过这种影响我们感知不到。总之，**凡是涉及到字符类型比较或排序的地方，都会和COLLATE有关**。

`COLLATE`通常是和数据编码（`CHARSET`）相关的，一般来说每种`CHARSET`都有多种它所支持的`COLLATE`，并且每种`CHARSET`都指定一种`COLLATE`为默认值。例如`Latin1`编码的默认`COLLATE`为`latin1_swedish_ci`，`GBK`编码的默认`COLLATE`为`gbk_chinese_ci`，`utf8mb4`编码的默认值为`utf8mb4_general_ci`。

mysql中有`utf8`和`utf8mb4`两种编码，**在mysql中请大家忘记**`**utf8**`**，永远使用**`**utf8mb4**`。这是mysql的一个遗留问题，mysql中的`utf8`最多只能支持3bytes长度的字符编码，对于一些需要占据4bytes的文字，mysql的`utf8`就不支持了，要使用`utf8mb4`才行。

```sql
CREATE DATABASE <db_name> DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
 `sex` varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '性别'
```

关于mysql的`COLLATE`相关知识。不过，在系统设计中，我们还是要**尽量避免让系统严重依赖中文字段的排序结果，**在mysql的查询中也应该尽量避免使用中文做查询条件。

### create function

```sql
    CREATE  
        [DEFINER = { user | CURRENT_USER }]
        FUNCTION functionName ( varName varType [, ... ] )
        RETURNS returnVarType
        [characteristic ...] 
        routine_body
        
       CREATE FUNCTION `func_updateUserName`(`userId` int,`userName` varchar(100))
     RETURNS int(11)
    BEGIN
     declare tname varchar(100);
     update `user` set `name`=userName where user_id=userId;
     select `name` into tname from `user` where user_id=userId;
     if tname=userName then
      return 1;
     else
      return 0;
     end if;
    END     
```

![image-20211204144456642](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204144456642.png)

全部继承basemapper

![image-20211204144524622](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204144524622.png)



## select 查询和 Wrapper

- selectById：根据 ID 查询
- selectBatchIds：根据 ID 批量查询，即一次传递多个 ID
- selectOne：根据构建的 Wrapper 条件查询数据，且只返回一个结果对象
- selectCount：根据构建的 Wrapper 条件对象查询数据条数



## select 复杂和动态查询

```java
// 根据 entity 条件，查询全部记录
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 查询（根据 columnMap 条件）
List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

// 根据 Wrapper 条件，查询全部记录
List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询全部记录。注意： 只返回第一个字段的值
List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

map

```java
private void query(int userId, String name, Integer age) {
        System.out.println("\n查询数据：");
        Map<String,Object> map = new HashMap<>();
        map.put("user_id", userId);
 
        // 第一个参数为是否执行条件，为true则执行该条件
        // 下面实现根据 name 和 age 动态查询
        if(StringUtils.isNotEmpty(name)) {
            map.put("name", name);
        }
        if(null != age) {
            map.put("age", age);
        }
 
        List<UserBean> userBeanList = simpleMapper.selectByMap(map);
        for(UserBean userBean : userBeanList) {
            System.out.println(userBean);
        }
    }

@Test
    void contextLoads() {
        query(1, null, null);
        query(1, "赫仑", null);
        query(1, "赫仑", 27);
    }
```



## 什么是 Service CRUD？

MyBatis Plus 提供了通用的 Mapper 接口（即 BaseMapper 接口），该接口对应我们的 DAO 层。在该接口中，**定义了我们常见的方法签名**，这样就可以方便我们对表进行操作。例如：查询（select）、插入（insert）、更新（update）和删除（delete）操作。

除了 BaseMapper 接口，**MyBatis Plus 还提供了 IService 接口**，**该接口对应 Service 层**。MyBatis Plus 的通用 Service CRUD 实现了 IService 接口，进一步封装 CRUD。**为了避免与 BaseMapper 中定义的方法混淆，该接口使用 get（查询单行）、remove（删除）、list（查询集合）和 page（分页）前缀命名的方式进行区别。**

![image-20211204151551903](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204151551903.png)

#### 链式查询（chain query）

```JAVA
// 链式查询 普通
QueryChainWrapper<T> query();
// 链式查询 lambda 式。注意：不支持 Kotlin
LambdaQueryChainWrapper<T> lambdaQuery(); 
 
// 示例：
query().eq("column", value).one();
lambdaQuery().eq(Entity::getId, value).list();
```

保存数据

![image-20211204151807165](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204151807165.png)	



## Service 链式查询

在 IService 中提供了一个 query 方法，该方法返回 QueryChainWrapper 对象。我们可以使用该对象实现链式查询，避免每次都创建 QueryWrapper 对象。query 方法定义如下：

```java
// 链式查询 普通
QueryChainWrapper<T> query();

@RunWith(SpringRunner.class)
@SpringBootTest
class Chain1Test {
    @Autowired
    private UserService userService; 
    @Test
    void contextLoads() {
        List<UserBean> userBeanList = userService.query()
                .eq("sex", "男")
                .gt("salary", 7000)
                .lt("age", 30).list();
        for(UserBean userBean : userBeanList) {
            System.out.println(userBean);
        }
    } 
}
```



## Wrapper

### eq（等于 =）

### ne（不等于 != 或 <>）

### gt（大于 >）

```java
QueryWrapper<UserBean> wrapper = ``new` `QueryWrapper<>();
wrapper.gt(``"age"``, ``18``); ``// 等价 SQL 语句：age > 18
```

### ge（大于等于 >=）

### lt（小于 <）

### le（小于等于 <=）

### between（BETWEEN 值1 AND 值2）

```java
QueryWrapper<UserBean> wrapper = ``new` `QueryWrapper<>();
wrapper.between(``"age"``, ``18``, ``30``); ``// 等价于 SQL 语句：age between 18 and 30
```

### like（完全模糊，即“like '%val%'”）

```java
QueryWrapper<UserBean> wrapper = ``new` `QueryWrapper<>();
wrapper.like(``"name"``, ``"王"``); ``// 等价 SQL 语句：name like '%王%'
```

### likeLeft（仅左边模糊，即“like '%val'”）

### isNull（字段 IS NULL）

```java
QueryWrapper<UserBean> wrapper = new QueryWrapper<>();
wrapper.isNotNull("name"); // 等价 SQL 语句：name is not null
```

### in（对应SQL中的 in 操作符）

```java
List<Integer> sexList= new ArrayList<>();
sexList.add("男");
sexList.add("女");
QueryWrapper<UserBean> wrapper = new QueryWrapper<>();
wrapper.in("sex", sexList); // 等价 SQL 语句：sex not in("男", "女")
```

### inSql（对应SQL中的 in 操作符）

```java
QueryWrapper<UserBean> wrapper = new QueryWrapper<>();
wrapper.inSql("user_id", "select user_id from user where user_id<10");
```

### orderByAsc（实现递增排序）

实例：根据用户 ID 和 年龄递增排序。

```java
QueryWrapper<UserBean> wrapper = new QueryWrapper<>();
wrapper.orderByAsc("user_id", "age");
```

## group by 分组

```java
private void totalSalary() {
        QueryWrapper<UserBean> wrapper = new QueryWrapper<>();
        wrapper.groupBy("sex", "age");
        wrapper.select("sex, age, sum(salary) as total_salary");
 
        List<UserBean> userBeanList = simpleMapper.selectList(wrapper);
        for(UserBean userBean : userBeanList) {
            System.out.println("sex=" + userBean.getSex() + ", age=" + userBean.getAge()
                    + ", totalSalary=" + userBean.getTotalSalary());
        }
    }
```

## @TableName

@TableName 注解用来将指定的数据库表和 JavaBean 进行映射。 

属性有： value, shcema 属性用来指定模式名称,  是否保持使用全局的 tablePrefix 的值，如果设置了全局 tablePrefix 且自行设置了 value 的值。  对应 Mapper XML 文件中 <resultMap> 标签的 id 属性的值。用法见 “autoResultMap” 属性

@TableName 注解中使用 resultMap 配置映射名称。例如，“resultMap = "annotationUser3BeanMap"

```java
@TableName(value = "user", resultMap = "annotationUser3BeanMap")
public class AnnotationUser3Bean {
   @TableId(value = "user_id", type = IdType.AUTO)
   private String userId;
   // 忽略其他代码
}
```

## @TableId

该注解用于将某个成员变量**指定为数据表主键**。

```java
@TableName("user")
public class UserBean {
   @TableId(value = "user_id", type = IdType.AUTO)
   private Integer userId;
}
```

## @TableField

@TableField 字段注解，该注解用于标识非主键的字段。将数据库列与 JavaBean 中的属性进行映射

value：指定映射的数据库字段名

exist：是否为数据库表字段，默认为 true。

```java
@TableField("sex")
private String sex;
```

## @TableLogic

在 MyBatis Plus 中，@TableLogic 注解用于实现数据库数据逻辑删除。注意，该注解只对自动注入的 sql 起效：

value: 用来指定逻辑未删除值，默认为空字符串。

deval: 用来指定逻辑删除值，默认为空字符串。

```sql
-- 添加一个 deleted 字段，实现逻辑删除
ALTER TABLE `user`
ADD COLUMN `deleted`  varchar(1) NULL DEFAULT 0 COMMENT '是否删除（1-删除；0-未删除）';
```

```java
@TableLogic(value = "0", delval = "1")
   private String deleted;
```



## @Version

### 什么是乐观锁？ 

乐观锁（Optimistic Locking）是相对悲观锁而言的，**乐观锁假设数据一般情况下不会造成冲突**，所以**在数据进行提交更新的时候，才会正式对数据的冲突进行检测**。如果发现冲突了，则**返回给用户错误的信息，让用户决定如何去做**。乐观锁适用于**读操作多的场景，这样可以提高程序的吞吐量。**

![image-20211204164320089](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204164320089.png)

![image-20211204164303790](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204164303790.png)

```java
	 @Version
   private int version;

@Configuration
public class MybatisPlusConfig { 
    @Bean
    public MybatisPlusInterceptor paginationInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    } 
}
```

## @EnumValue

该注解用来**将数据库定义的枚举类型和Java的枚举进行映射**。注意**、该注解需要添加到枚举类的某个字段上面，而不是添加到 JavaBean 实体的某个字段上面。**

```java
public enum WeekEnum {
    SUNDAY("Sunday", "星期日"),
    SATURDAY("Saturday", "星期六"),
    FRIDAY("Friday", "星期五"),
    THURSDAY("Thursday", "星期四"),
    WEDNESDAY("Wednesday", "星期三"),
    TUESDAY("Tuesday", "星期二"),
    MONDAY("Monday", "星期一");
 
    @EnumValue
    private String value;
    private String name;
    // ...
}
```

上面代码将 @EnumValue 注解标注在 value 字段，**表示该字段的值和数据库表中定义的枚举值一一对应**。在 JavaBean 中就可以直接引用该枚举了

```java
public class User {
    private int id;
    private String name;
    private String sex;
    private WeekEnum week;
}
```

```java
import com.baomidou.mybatisplus.annotation.*;
import com.hxstrive.mybatis_plus.enums.WeekEnum;
 
@TableName(value = "user")
public class AnnotationUser6Bean {
   @TableId(value = "user_id", type = IdType.AUTO)
   private int userId;
 
   @TableField("name")
   private String name;
 
   @TableField("sex")
   private String sex;
 
   @TableField("age")
   private Integer age;
 
   @TableField("week")
   private WeekEnum week;
 
   // 忽略 getter 和 setter
 
   @Override
   public String toString() {
      String weekStr = "IS NULL";
      if(this.week != null) {
         weekStr = "[" + this.week.getValue() + "] " + this.week.getName();
      }
      return "UserBean{" +
            "userId=" + userId +
            ", name='" + name + '\'' +
            ", sex='" + sex + '\'' +
            ", age=" + age +
            ", week=" + weekStr +
            '}';
   }
 
}
```

配置 MyBatis Plus 自动扫描我们定义的枚举类型，配置如下：

```xml
mybatis-plus:
  # 支持统配符 * 或者 ; 分割
  typeEnumsPackage: com.hxstrive.mybatis_plus.enums
```

如果你忘记了上面的配置，运行程序会抛出“No enum constant com.hxstrive.mybatis_plus.enums.WeekEnum.Friday”错误信息。

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class AnnotationDemo6 { 
    @Autowired
    private AnnotationUser6Mapper userMapper;
    @Test
    void contextLoads() throws Exception {
        QueryWrapper<AnnotationUser6Bean> wrapper = new QueryWrapper<>();
        wrapper.lt("user_id", 10);
        for(AnnotationUser6Bean item : userMapper.selectList(wrapper)) {
            System.out.println(item);
        }
    }
}
```



## 多租户

多租户简单来说是指**一个单独的实例可以为多个组织服务**。多租户技术为共用的数据中心内如何以单一系统架构与服务提供多数客户端相同甚至可定制化的服务，并且仍然可以保障客户的数据隔离。一个支持多租户技术的系统需要**在设计上对它的数据和配置进行虚拟分区**，从而使系统的每个租户或称组织都能够使用**一个单独的系统实例**，并且每个租户都可以根据自己的需求对租用的系统实例进行个性化配置。

**例如**：在一台服务器上面部署一个学生系统，这套学生系统可以支持多个学校使用。这里的一个学校就是一个租户，每个登录系统的用户必然属于某个租户（超级管理员例外）。因此，登录用户也就只能看见自己租户下面的内容，其他租户的内容对他是不可见的。

### 独立数据库

### 共享数据库，隔离数据架构

多个或所有租户共享Database，但一个 Tenant 一个 Schema。

### 共享数据库，共享数据架构

租户共享同一个 Database、同一个 Schema，但在表中通过 TenantID 区分租户的数据。这是共享程度最高、隔离级别最低的模式。

## MyBatis Plus 多租户实现

在 MyBatis Plus 中，提供了 **TenantLineInnerInterceptor 插件和 TenantLineHandler 接口**。其中，TenantLineInnerInterceptor 插件用来自动**向每个 SQL 的 where 后面添加判断条件“tenant_id=用户的租户ID”**。而 TenantLineHandler 接口用来**给 TenantLineInnerInterceptor 插件提供租户ID、租户字段名**。

（1）向 user 表添加 tenant_id 字段，sql 如下：

```sql
-- 添加一个 tenant_id 字段，保存租户ID，用来实现多租户
ALTER TABLE `user`
ADD COLUMN `tenant_id`  varchar(1) NULL COMMENT '租户ID，用来实现多租户';
```

（2）**定义一个租户ID管理器**，即可以动态改变当前租户ID。便于我们测试时，动态修改租户ID。实际环境中，**租户ID是在用户登录成功后写入Session中**。代码如

```java
@Component
public class TenantIdManager {
    /** 当前用户租户 KEY */
    private static final String KEY_CURRENT_TENANT_ID = "KEY_CURRENT_PROVIDER_ID";
    /** 保存当前租户ID */
    private static final Map<String, Object> TENANT_MAP = new ConcurrentHashMap<>(); 
    /**
     * 设置租户
     * @param tenantId 租户ID
     */
    public void setCurrentTenantId(Long tenantId) {
        TENANT_MAP.put(KEY_CURRENT_TENANT_ID, tenantId);
    } 
    /**
     * 返回当前用户租户ID
     * @return
     */
    public Long getCurrentTenantId() {
        return (Long) TENANT_MAP.get(KEY_CURRENT_TENANT_ID);
    } 
}
```

（3）使用 @Configuration 和 @Bean 注解配置 MyBatis Plus 的多租户插件，代码如下：

```java
@Configuration
public class MybatisPlusConfig {
    /* 租户ID管理器 */
    @Autowired
    private TenantIdManager tenantIdManager;
    @Bean
    public MybatisPlusInterceptor paginationInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 多租户插件
        TenantLineInnerInterceptor tenantInterceptor = new TenantLineInnerInterceptor();
        tenantInterceptor.setTenantLineHandler(new TenantLineHandler() {
            @Override
            public Expression getTenantId() {
                // 返回当前用户的租户ID
                return new LongValue(tenantIdManager.getCurrentTenantId());
            }
        });
        interceptor.addInnerInterceptor(tenantInterceptor);
        return interceptor;
    } 
}
```

（4）单元测试，验证多租户是否生效。代码如下：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
class PluginDemo { 
    @Autowired
    private TenantMapper mapper;
    @Autowired
    private TenantIdManager tenantIdManager;
    @Test
    void contextLoads() {
        findUser(1); // 租户ID=1
        findUser(8); // 租户ID=8
    } 
    private void findUser(long tenantId) {
        // 设置租户ID
        tenantIdManager.setCurrentTenantId(tenantId);
        // 查询用户列表
        QueryWrapper<TenantUserBean> wrapper = new QueryWrapper<>();
        wrapper.lt("user_id", 20);
        List<TenantUserBean> userBeanList = mapper.selectList(wrapper);
        for(TenantUserBean userBean : userBeanList) {
            System.out.println(userBean);
        }
    }

}
```

