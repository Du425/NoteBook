![image-20211107163434163](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211107163434163.png)

1.如图所示，修改的时候只修改一个数据的时候，其他属性之前的值都无法保存，返回为空。

![image-20211107164058342](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211107164058342.png)

2.没有对password,telephone这些格式做规定限制

![image-20211107164706926](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211107164706926.png)

3.已经存了这条数据，但是 显示找不到

4.依赖有问题

![image-20211124195519666](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124195519666.png)

5.org.apache.ibatis.binding.BindingException: Invalid bound statement (not found):

![image-20211124200947492](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211124200947492.png)

6.

![image-20211125201016120](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211125201016120.png)

7. 统一结果封装

![image-20211125201615680](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211125201615680.png)

**类似于搬家，将柜子拆成零件就是序列化，组装就是反序列化**

To *serialize* an object means to **convert its state to a byte stream so that the byte stream can be reverted back into a copy of the object**. A Java object is *serializable* if its class or any of its superclasses implements either the `java.io.Serializable` interface or its subinterface, `java.io.Externalizable`. ***Deserialization* is the process of converting the serialized form of an object back into a copy of the object.**

序列化：对象的寿命通常随着生成该对象的程序的终止而终止，有时候需要把在内存中的各种对象的状态（也就是实例变量，不是方法）保存下来，并且可以在需要时再将对象恢复。虽然你可以用你自己的各种各样的方法来保存对象的状态，但是Java给你提供一种应该比你自己的好的保存对象状态的机制，那就是序列化。

总结：Java 序列化技术可以使你将一个对象的状态写入一个Byte 流里（系列化），并且可以从其它地方把该Byte 流里的数据读出来（反序列化）。

用途：把内存中的对象状态保存到一个文件中或者数据库中/ 把对象通过网络进行传播

![image-20211125204323694](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211125204323694.png)

![image-20211125204339874](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211125204339874.png)

在ObjectMapper时的Responsebody中用到JSON



**登录逻辑**

![image-20211125205921200](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211125205921200.png)

![image-20211125211814957](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211125211814957.png)



JSON Web Token (JWT)是一个开放标准(RFC 7519)，它定义了一种紧凑的、自包含的方式，用于作为JSON对象在各方之间安全地传输信息。该信息可以被验证和信任，因为它是数字签名的。

应用场景：

- **Authorization (授权)** : 这是使用JWT的最常见场景。一旦用户登录，后续每个请求都将包含JWT，允许用户访问该令牌允许的路由、服务和资源。单点登录是现在广泛使用的JWT的一个特性，因为它的开销很小，并且可以轻松地跨域使用。
- I**nformation Exchange (信息交换)** : 对于安全的在各方之间传输信息而言，JSON Web Tokens无疑是一种很好的方式。因为JWT可以被签名，例如，用公钥/私钥对，你可以确定发送人就是它们所说的那个人。另外，由于签名是使用头和有效负载计算的，您还可以验证内容没有被篡改。

JSON Web Token由三部分组成，它们之间用圆点(.)连接。这三部分分别是：

- Header
- Payload
- Signature

因此，一个典型的JWT看起来是这个样子的：xxxxx.yyyyy.zzzzz

JWT与Session的差异 相同点是，它们都是存储用户信息；然而，S**ession是在服务器端的，而JWT是在客户端的。**Session方式存储用户信息的最大问题在于要占用大量服务器内存，增加服务器的开销。而**JWT方式将用户状态分散到了客户端中，可以明显减轻服务端的内存压力**。**Session的状态是存储在服务器端，客户端只有session id**；而Token的状态是存储在客户端。

**不懂**

jwt也是可以的，每个用户一个sha256哈希的密码，某个用户密码被破了，就改掉这个密码。 jwt和传统session(包括说的token)，唯一区别就是 服务端保存不保存这个token。然后是，jwt带的身份标识到了服务端后，需要获取权限。session的那个id 也可以存到redis里。redis key就是session的id，value就是鉴权信息。jwt的话，想要增加速度，鉴权信息得查出来后 也可以把鉴权的信息存储到redis里，那么问题来了，到底会不会节省服务器的内存？ 估计只会省掉存储身份id这个空间。
jwt还有个好处，就是服务器，redis炸了，session id的话就会丢失，jwt的话 还在。

![image-20211126113152368](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211126113152368.png)



![image-20211126142305660](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211126142305660.png)







![image-20211126190237524](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211126190237524.png)

![image-20211127162210610](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127162210610.png)



## 全局异常处理



![image-20211127162602681](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127162602681.png)



![image-20211127190632489](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127190632489.png)



![image-20211127193200036](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127193200036.png)





![image-20211127201748840](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127201748840.png)

![image-20211127201759093](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127201759093.png)

然后报错：

![image-20211127201932423](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127201932423.png)

@NotBlank不生效，这才是正确的依赖

![image-20211127203312861](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211127203312861.png)



## 解决跨域问题

版本冲突

![image-20211128001814185](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211128001814185.png)

![image-20211128001735146](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211128001735146.png)



输入的密码是正确的，但是显示密码不正确

![image-20211129172458125](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211129172458125.png)

```java
@PostMapping("/login")
public CommonResult login(@Validated @RequestBody LoginDto loginDto, HttpServletResponse response){

    MUser user = imUserService.getOne(new UpdateWrapper<MUser>().eq("username", loginDto.getUsername()));
    Assert.notNull(user,"用户不存在");

    if (!user.getPassword().equals(SecureUtil.md5(loginDto.getPassword()))){
        return CommonResult.failed("密码不正确");
    }
```

**因为 密码要md5加密**，在存的时候就把密码加密一遍

```java
@PostMapping("/save")
public CommonResult save(@Validated @RequestBody MUser mUser){
    String rawPassword = mUser.getPassword();
    String password = SecureUtil.md5(rawPassword);
    SecureUtil.md5(password);
    mUser.setPassword(password);
    int insert = mUserMapper.insert(mUser);
    if (insert==1){
        return CommonResult.success("插入成功",mUser);
    }else {
        return CommonResult.failed("插入失败");
    }
}
```





![image-20211129213946447](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211129213946447.png)

![image-20211129220058801](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211129220058801.png)

