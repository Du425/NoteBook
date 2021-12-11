## 跨域：CorsConfig

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry corsRegistry){
        corsRegistry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET","POST","HEAD","PUT","DELETE","OPTIONS")
                .allowCredentials(true)
                .maxAge(3600) //60min
                .allowedHeaders("*");
    }
}
```

![image-20211201172927707](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211201172927707.png)



## private static final long serialVersionUID = 1L

Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。

在进行反序列化时，JVM会把**传来的字节流中的serialVersionUID与本地相应实体（类）的serialVersionUID进行比较**，如果相同就认为是**一致的，可以进行反序列化**，否则就会出现序列化版本不一致的异常。**(InvalidClassException)**

serialVersionUID有两种显示的生成方式：       

 一个是默认的1L，比如：private static final long serialVersionUID = 1L;      

 一个是根据类名、接口名、[成员方法](https://www.zhihu.com/search?q=成员方法&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A117314768})及属性等来生成一个64位的[哈希字段](https://www.zhihu.com/search?q=哈希字段&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A117314768})，

比如：private static final   long  serialVersionUID = xxxxL;

**问题一:假设有A端和B端,如果2处的serialVersionUID不一致,会产生什么错误呢?**

**1)先执行测试类SerialTest,然后修改serialVersion值(或注释掉serialVersion并编译),再执行测试类DeserialTest,报错:**

java.io.InvalidClassException: com.test.Serial; local class incompatible: stream classdesc serialVersionUID = 6977402643848374753, local class serialVersionUID = 6977402643848374752

**2)A端和B端都没显示的写serialVersionUID,实体类没有改动(如果class文件(类名,方法明等)没有发生变化(增加空格,换行,增加注释,等等),).序列化,反序列化正常.**

**问题二:假设2处serialVersionUID一致,如果A端增加一个字段,B端不变,会是什么情况呢?**

答案: **序列化,反序列化正常,A端增加的字段丢失(被B端忽略).**

A>B被忽略，A<B:赋予新字段默认值为0

假设2处serialVersionUID一致,如果B端减少一个字段,A端不变,会是什么情况呢?

答案:与问题二类似,序列化,反序列化正常,B端字段少于A端,A端多的字段值丢失(被B端忽略).

问题三:假设2处serialVersionUID一致,如果B段增加一个字段,A端不变,会是什么情况呢?

答案三: 序列化,反序列化正常,B端新增加的int字段被赋予了默认值0.



### @EqualsAndHashCode

从对象的字段生成`hashCode`和`equals`实现。

可以使用`@EqualsAndHashCode`lombok**生成`equals(Object other)`和`hashCode()`方法的实现来注释任何类定义**。**默认情况下，它将使用所有非静态，非瞬态字段**，但您可以通过使用`@EqualsAndHashCode.Include`或标记类型成员来修改使用哪些字段（甚至指定要使用各种方法的输出）`@EqualsAndHashCode.Exclude`。或者，您可以准确地指定哪些字段，或者您希望通过与标记它们被使用的方法`@EqualsAndHashCode.Include`和使用`@EqualsAndHashCode(onlyExplicitlyIncluded = true)`。。



```java
@TableField(fill = FieldFill.INSERT)
    private Long findTime;
```



```java
 @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
```



```java
@TableLogic
    private Boolean isDelete;
```



```java
@Data
@EqualsAndHashCode(callSuper = false)
@ApiModel(value="UserPermission对象", description="")
public class TUserPermission implements Serializable {

    private static final long serialVersionUID = 1L;
```



## Java8使用stream().map()提取List对象的某一列值及排重

```java
//提取前输出
StudentInfo.printStudents(studentList);
//从对象列表中提取age并排重
List<Integer> ageList = studentList.stream().map(StudentInfo::getAge).distinct().collect(Collectors.toList());
ageList.forEach(a-> System.out.println(a));
```

![image-20211204213714037](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204213714037.png)



```java
@Override
    public List<TFoundThingVO> converToLossVO(List<TFoundThing> list) {
        final List<TFoundThingVO> thingVOList = list.stream().map(e -> {
            final TFoundThingVO tFoundThingVO = new TFoundThingVO();
            BeanUtils.copyProperties(e, tFoundThingVO);
            return tFoundThingVO;
        }).collect(Collectors.toList());
        return thingVOList;
    }
```

为什么是两个双引号

![image-20211204214301049](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211204214301049.png)
