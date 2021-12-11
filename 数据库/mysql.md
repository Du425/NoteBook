## 概念

1. 关系型数据库（SQL），表和表之间
2. 非关系型数据库（No SQL, mongDB, Redis)
3. DBMS：数据库管理系统

## 基本的命令行操作

1. 链接数据库： mysql -u root  -p
2. flush privileges  --刷新权限
3. --所有的语句都是用；结尾
4. ctrl+c强行终止
5. show tables --显示所有的表

```sql
--注释
```

6. 语言：DDL, DML, DCL, DQL（CRUD增删改查）

## 操作数据库的语句

- 创建数据库

1. MySQL的语句不区分大小写

2. create database if not exsits westos

   ```sql
   CREATE TABLE `grade`(
   	`gradeid` INT(10) NOT NULL auto_increment comment '年级id',
   	`gradename` varchar(50) not null comment '年级名称',
   	primary key(`gradeid`)
   	)engine=innodb default charset=utf8
   ```

   

- 删除

1. drop database if exsits westos

- 使用

1. use ‘school’

- 查

1. show database

## 数据库的类型

> 数值

tinyint  1个字节

smallint 2个字节

mediumint 3个字节

- int 标准整数 4个字节
- Bigint 8个字节
- float    4个字节
- decimal 字符串形式的浮点数 
- double 8个字节  精度问题

> 字符串

- char 字符串固定大小， 0-255
- varchar 可变字符串 ， string, 0-65535
- tinyint 微型文本 2^8-1
- text 文本串， 保存大文本

> 时间日期

- ### java.util.Date

- data YYYY-MM-DD ,日期格式

- time HH:mm:ss,时间格式

- datatime  YYYY-MM-DD HH:mm:ss 最常用的时间格式

- timestamp  时间戳， 1970.1.1 到现在的毫秒数

- Year 年份表示

## 数据库的字段属性

- Unsigned, 无符号的整数，声明该列不能为负数
- zerofill, 0填充，不足的位数用0填充
- 自增，必须是整数类型，在上一条记录的基础上+1
- 非空（null ， not null)，如果为NOT NULL,不赋值就会报错
- 默认，设置默认的值
- 字符串都要用单引号，所有语句用，连接

## 数据表的类型

- INNODB
- MYISAM

|              | MYISAM       | INNODB             |
| ------------ | ------------ | ------------------ |
| 事务支持     | 不支持       | 支持               |
| 数据行锁定   | 不支持       | 支持               |
| 外键约束     | 不支持       | 支持               |
| 全文索引     | 支持         | 不支持             |
| 表空间的大小 | 较小         | 较大，约为2倍      |
|              | 只有*frm文件 | *.frm  *.MYD *.MYI |

- 物理位置：C:\Users\86159\Documents\Navicat\MySQL\Servers\1234\rings
- CHARSET=utf8:不设置的话，回事mysql默认的， MySQL的默认编码是Latin1，不支持中文，那么如何修改MySQL的默认编码呢，下面以设置UTF-8为例来说明. 在my.ini中配置默认的编码为  character-set-server=utf8
- SHOW CREATE DATABASE school
- SHOW CREATE TABLE student
- DESC student

## 修改删除表

- alter table teacher rename as teacher1 ,修改表名
- alter table teacher1 add age int(11), 增加表的字段
- alter table teacher1 modify age varchar(11), 修改表的字段，修改表的约束， Modify
- alter table teacher1 change age age1 ,字段重命名 ，change
-  modify只能改字段数据类型完整约束，不能改字段名，但是change可以。 
- alter table teacher1 drop age1, 删除表的字段
- drop table if exists teacher1

# Mysql数据管理

## 外键

- 学生的grade列，引用年级表的id
- KEY `FK_gradeid(`gradeid`)
- 方式一，在创建表的时候，增加约束，索引,referrences，删除有外键的表的时候，需要先删除从表在删除主表
- 方式二，后期修改

```sql 
alter table `student`
add constraint `fk_gradeid` foreign key(`gradeid`) references `grade`(`gradeid`)
```

- 用程序实现外键？
- 阿里规定：不得使用外键和级联

## DML语言

- 数据库意义：数据存储，数据管理

## 添加

- insert into 表名([字段名1，字段2，字段3]) values(‘值1’，‘值2’，，，)
- 自增的数据不需要管理，会自增
- 插入多个字段

```sql
insert into `grade`(`gradename`)
values('大二'),('大一')

insert into `student`(`name`,`pwd`,`sex`)
values('lisi','aa','male'),('wang','bb','female')
--字段可以省略，但是值必须一一对应
```



## 修改

> update  修改谁（条件）  set

```sql
update `student` set `name`=`kuang` where id =1;

--不指定条件的情况下，会改动所有表
update `student` set `name` = 'changjiang'

--修改多个属性，逗号隔开
update `student` set `name`=`kuang`,`email`='1910@qq.com' where id = 1;


```

## 操作符

- between···and···，在某个范围内
- and, 多个条件定位范围， &&
- or, ||
- value是一个具体的值，也可以是一个变量

```
update `student` set `birthday`=current_time where `name`='changjiang' and sex='male'
```

## 删除

> delete from `student` where id=1



> truncate`student`,清空表

delete与truncate的区别：

相同：都能删除数据，都不回删除表结构

不同：

# DQL

```sql
select * from student

select `studentno`,`studentname` from student

--别名--
select `studentno` as 学号,`studentname` from student

--concat(a,b)--
select concat('name:',studentname) as new name from student

--去重--
select distinct `studentno` from result

select 可以查询函数 select version()
select 100*3-1 as 计算结果  --计算表达式
select @@auto_increment_increment  --查询自增的步长

--学院考试分数+1分后查看
select `studentno`,`studentresult`+1 as `提分后` from result

```

### 模糊查询

- 运算符： is null,  is not null,  a between b and c,  like,  in

```sq
select `studentno`,`studentname` from `student`
where studentname like '刘%'

select `studentno`,`studentname` from `student`
where studentname like '刘_'

select `studentno`,`studentname` from `student`
where studentname like '%嘉%'

select `studentno`,`studentname` from `student`
where adress in '安徽'  --条件具体到引号里的值--

```

## 联表查询

>left join   inner join   right join,  七种join理论

 ![é¢è¯ä¸­å¦ä½åé¤ é±¼ç®æ··ç ç¨åºå ](http://www.uml.org.cn/itnews/images/201409300802.jpg) 

```sql
--inner join
select studentno, studentname, subjectno, studentresult
from student as s
inner join result as r
where s.studentno = r.studentno
报错：column `studentno` in field list is ambiguous
修正：
select s.studentno, studentname, subjectno, studentresult
from student as s
inner join result as r
where s.studentno = r.studentno

--right join(以右边的表为查询基础，返回所有右表所有符合条件的结果)
select s.studentno, studentname, subjectno, studentresult
from student s
right join result r
on  s.studentno = r.studentno

--left join
select s.studentno, studentname, subjectno, studentresult
from `student` s
left join result r
on  s.studentno = r.studentno

--查询缺考的同学
select s.studentno, studentname, subjectno, studentresult
from `student` s
left join result r
on  s.studentno = r.studentno
where studentresult is null

/*
1.分析需求，分析查询的字段来自哪些表
2.确定使用哪种连接查询
3.判断条件
*/
--从哪几个表中查  FROM表  XXXjoin 连接的表  on  交叉条件
```

> 自连接

核心：自己的表和自己的表连接，一张表拆为两张一样的表即可

子类与父类，查询父子关系

```sql
select a.`category` as '父栏目', b.'categoryname' as '子栏目'
from `category` as a, `category` as b
where a.`categoryid` = b.`pid`

```

## 分页和排序

- 排序：升序ASC, 降序

```sql
select s.studentno, studentname, subjectno, studentresult
from `student` s
inner join result r
on  s.studentno = r.studentno
inner join `subject` sub
on r.`subjectno` = sub.`subjectno`
where studentname = '数据库结构'
order by subjectresult asc
```

- 分页，缓解数据库压力，抖音：瀑布流
- 每页只显示5页，limit起始的位置，页面的大小
- 网页应用：当前，总的页数
- 第n页：limit (n-1)*pagesize,  pagesize

--查询JAV第一学年，课程成绩排名前十的学生，并且分数要大于80的学生信息（学号，姓名，课程名称，分数）

```sql
select s.studentno, studentname, subjectno, studentresult
from `student` s
inner join `result` r
on  s.studentno = r.studentno
inner join `subject` sub
on r.`subjectno` = sub.`subjectno`
where subjectname = 'JAV第一学年' and studentresult >=80
order by subjectresult desc
limit 0,10
```

### 子查询

本质：在where语句中嵌套一个子查询语句, 由里及外

```sql
select studentno, subjectno, studentresult
from `result`
where subjectno = (
	select subjectno from `subject`
    where subjectname = '数据库结构'
)
order by studentresult desc
limit 0,10
```

```sql
select distinct s.studentno, subjectname
from student s
inner join result r
on r.studentno = s.studentno
where `studentresult`>=80 and `subjectno` = (
	select subjectno from `subject`
    where subjectname = '数据库结构'
)
=
select distinct s.studentno, subjectname
from student s
inner join result r
on r.studentno = s.studentno
inner join subject sub
on r.subjectno = sub.subjectno
where `studentresult`>=80 and `subjectname` = '数据库结构'

```

## mysql常用函数

```sql
select [distinct] ,,, from ,,,
left/right/inner join,,
where --指定结果需满足的条件
group by --指定结果按照哪几个字段来分组
having --过滤分组的记录必须满足的次要条件
order by --记录按照条件排序
limit --记录从哪条到哪条
```



- 数学函数

```sql
select abs(-5)  --绝对值
select ceiling(9.4)  --向上取整
select floor(9.4)  --向下取整
select rand()  --返回一个0-1之间的随机数
select sign(10)  --判断一个数的符号（正负，0）
```

- 字符串函数

```sql
select replace(studebtname,'zhou','zou') from student
where studentname like 'zhou'
```

### 聚合函数

```sql
count()
select count(studentname) from student;
select count(*) from student;
select count(1) from student;
sum()
avg()
select avg('studentresult') as 平均分 from resultmk,
min()
```

#### 淘宝，千人千面，强大的数据库

## 事务

### 什么是事务

- 要么都成功，要么都失败
- 将一组SQL放到一个批次中去执行

> 事务原则： 原子性，一致性，隔离性， 持久性

- 原子性（Atomicity）：要么都成功，要么都失败
- 一致性（Consistency）：事务前后的数据完整性要保持一致

- 脏读（Durability）：事务一旦提交则不可逆，被持久化到数据库中
- 隔离性（Isolation）:多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰
- 不可重复读
- 幻读
- 虚读

### 测试事务实现转账

```sql
--mysql 是默认开启事务自动提交的
set autocommit = 0 关闭
set autocommit = 1 开启

--手动处理
set autocommit = 0 

--标记一个事务的开启，从这个之后的sql都在同一个事务内
start transaction
insert ,,,
insert ,,,  --一旦一个操作失败，整个事务失败

--提交：持久化（执行成功）
commit

--回滚：回到原来的样子（执行失败）
rollback

--事务结束
set autocommit = 1

--回滚到保存点
savepoint  --保存
rollback to savepoint  
release savepoint  --撤销保存点
```

```sql
#模拟转账
create table `account`(
	`id` int(3) not null auto_increment,
    `name` varchar(30) not null,
    `money` decimal(9,2) not null,
    primary key(`id`)
)engine = innodb default charset=utf8

insert into account(`name`,`money`)
values('A',2000.00),('B',10000.00)

set autocommit = 0;
start transaction;
update `account` set money=money-500 where `name`='A';
update `account` set money=money-500 where `name`='B';
ROLLBACK;
commit;

set autocommit = 1;

select * from `account`;

```

## 索引(index, Key)

- 是帮助Mysql高效获取数据的数据结构

- 索引的分类

>主键索引：primary key
>
>- 唯一的索引，主键不可重复，只能有一个列作为主键
>
>唯一索引：unique key
>
>- 避免重复的列出现，唯一索引可以重复，多个列都可以成为唯一索引
>
>常规索引：key/index
>
>- 默认的，用index/key关键字来设置
>
>全文索引：fulltext

- 索引的使用：在创建表的时候给字段增加索引或者创建完毕后，增加索引
- 显示所有的索引信息：show index from student
- 增加索引：alter table `account` add fulltext index `money`(列名);

- explain：分析sql执行的状况 explain select* from student;

### 创建索引

- 插入一百万条数据

```sql
delimiter $$  --写函数之前必须写，是标志
create function mock_data()
return int
begin
	declare num int default 1000000;
	declare i int default 0;
	while i < num DO
		set i = i+1;
	end while
end;
insert into app_user(`name`,`email`,phone`,`gender`,`password`,`age`)
values(count('user',i),'191067@qq.com',concat('18',floor(rand()*((999999999-1000000000)+100000000))),floor(rand()*2),unnid(),floor(rand()*100))
```

###  索引原则

- 

> java方法：
>
> try
>
> catch

### 数据库用户管理

- grant
- delete

```sql
--创建用户
create user kuangshen identified by '123456'
--修改当前用户密码
set password = password '11111'
--修改指定用户密码
set password for kuangsen = password '1111'
--重命名用户
rename user kuangshen to kuangshen2

--用户授权给全部的库，全部的表
grant all privileges on *.* to kuangshen2

--查询权限
show grants for kuangshen2
show grants for root@localhost

--撤销权限,撤销哪些权限，在哪个库撤销，给谁撤销
revoke all privileges on *.* from kuangshen2

--删除用户
drop user kuangshen2

```

### 数据库备份

- 为什么要备份：保证重要的数据不丢失，可以做数据转移

mysql数据库备份方式

- 直接拷贝物理文件
- 在navicat可视化工具中手动备份/到处

![1633507846259](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633507846259.png)

- 使用命令行  mysqldump（cmd)

> mysqldump -hlocalhost -uroot -p123456 schoo; student >D:/a.sql

导入

- source  d:/a.sql
- mysqldump -hlocalhost -uroot -p123456 schoo; student <D:/a.sql

## 数据库设计



### 三大范式

为什么需要数据规范化

- 信息重复

- 更新异常
- 插入异常
- 删除异常

第一范式

- 原子性：保证每一列不可再分

第二范式

- 满足第一范式且每张表只描述一件事

第三范式

- 满足第一范式和第二范式且数据表中每列数据都和主键直接相关，不能间接相关

，，

第N范式 

规范性和性能的问题

关联查询的表不能超过三张表

- 考虑商业化的需求和目标，数据库的性能
- 规范性

## JDBC

> 数据库驱动：声卡、显卡、数据库

SUN公司为了简化开发人员对数据库的统一操作，提供了一个规范JDBC，这些规范的实现由具体的厂商去做，对开发人员来说，只需掌握JDBC的接口即可

java.sql

javax.sql

还需导入一个数据库驱动包 mysql-connector-java-5.1.47.jar



### 第一个JDBC程序

![1633513484917](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\1633513484917.png)

```java
package com.kuang.lesson01;

import java.sql.*;

//第一个JDBC程序
public class JDBC01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
        Class.forName("com.mysql.cj.jdbc.Driver"); //固定写法

        //2.用户信息和url
        String url = "jdbc:mysql://localhost:33060/jdbcstudy?useUnicode=true&characterEncoding=utf-8&useSSL=true";
        String username = "root";
        String password = "DST773344";

        //3.连接成功，数据库对象
        Connection connection = DriverManager.getConnection(url,username,password);

        //4.执行SQL的对象
        Statement statement = connection.createStatement();

        //5.执行SQL的对象 去 执行SQL
        String sql = "select * from users";

        ResultSet resultSet = statement.executeQuery(sql); //返回结果集

        while (resultSet.next()){
            System.out.println("id=" +resultSet.getObject("id"));
            System.out.println("name=" +resultSet.getObject("name"));
            System.out.println("pwd=" +resultSet.getObject("password"));
            System.out.println("email=" +resultSet.getObject("email"));
            System.out.println("birth=" +resultSet.getObject("birthday"));
        }

        //6.释放连接
        resultSet.close();
        statement.close();
        connection.close();

    }
}

```

### CRUD 操作

 CRUD是指在做计算处理时的增加(Create)、读取查询(Retrieve)、更新(Update)和删除(Delete)几个单词的首字母简写。主要被用在描述软件系统中DataBase或者持久层的基本操作功能。 

### statement对象

JDBC中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。



statement对象的executeUpdate方法，用于向数据库发送增、删、改的sql语句，executeUpdate执行完后，将会返回一个整数反应几行数据发生了变化



statement.executeQuery方法用于向数据库发送查询语句，返回代表查询结果的ResultSet对象。

> CRUD操作-create

使用executeUpdate(String sql) 方法完成数据添加操作

```java
statement st = connection.createstatement();
Strinf sql = "insert into user(,,,) values(,,,)";
int num = st.executeUpdate(sql);
if(num>0){
    System.out.println("插入成功")
}


public class TestUpdate {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try{
            connection = JdbcUtils.getConnection();//获取数据库连接
            statement = connection.createStatement();//获得SQL的执行对象
            String sql = "update users set name ='dst' where id = 7";

            int i = statement.executeUpdate(sql);
            if (i>0){
                System.out.println("修改成功");
            }
            String sql1 = "select * from users";

            ResultSet resultSet1 = statement.executeQuery(sql1); //返回结果集

            while (resultSet1.next()){
                System.out.println("id=" +resultSet1.getObject("id"));
                System.out.println("name=" +resultSet1.getObject("name"));
                System.out.println("pwd=" +resultSet1.getObject("password"));
                System.out.println("email=" +resultSet1.getObject("email"));
                System.out.println("birth=" +resultSet1.getObject("birthday"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(connection,statement,resultSet);

        }
    }
}

```

> CRUD操作-delete

```java
statement st = connection.createstatement();
Strinf sql = "delete from user where id=1";
int num = st.executeUpdate(sql);
if(num>0){
    System.out.println("删除成功")
}
```

> CRUD操作-update

```java
statement st = connection.createstatement();
Strinf sql = "update user set name ='' where name ='' ";
int num = st.executeUpdate(sql);
if(num>0){
    System.out.println("修改成功")
}
```

> CRUD-query

```java
statement st = connection.createstatement();
Strinf sql = "select* from user where id=1";
ResultSet rs = st.executeQuery(sql);
while(rs.next()){
    //根据获取列的数据类型，分别调用rs的相应方法
}

public class TestQuery {
    private static Object ResultSet;

    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try{
            connection = JdbcUtils.getConnection();//获取数据库连接
            statement = connection.createStatement();//获得SQL的执行对象
            String sql = "select * from users where id = 7 ";

            resultSet = statement.executeQuery(sql);

            while (resultSet.next()){
                System.out.println("id=" +resultSet.getObject("id"));
                System.out.println("name=" +resultSet.getObject("name"));
                System.out.println("pwd=" +resultSet.getObject("password"));
                System.out.println("email=" +resultSet.getObject("email"));
                System.out.println("birth=" +resultSet.getObject("birthday"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(connection,statement,resultSet);

        }
    }
}
```

### SQL注入

本质：网站存在漏洞会被攻击，拼接一个违法的字符串

==SQL会被拼接==

### preparedStatement 对象

- 也是增删改查，但是可以防止SQL注入
- 先传参预编译，在执行sql

### 使用IDEA连接数据库



### 事务

> 代码实现

1. 开启事务：connection.setAutoCommitment(false);

2. 一组业务执行完毕，提交事务
3. 可以在catch语句中显示的定义 回滚语句，但默认失败就会回滚

## 数据库连接池

### 为什么要连接池

1. 数据库连接--执行完毕--释放，这期间十分浪费资源
2. ==池化技术：预先准备资源，过来直接使用准备好的资源==
3. 最小连接数：根据业务调整
4. 最大连接数：业务最高承载上限
5. 排队等待
6. 等待超时
7. 编写连接池，实现一个接口，==Datesource==，
8. 开源数据源实现：==DBCP, C3P0, Druid==

### DBCP

1. 需要用的 jar包：commons-dbcp-1.4   commons-pool-1.6