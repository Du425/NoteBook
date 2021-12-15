## 二进制加法

给定两个 01 字符串 `a` 和 `b` ，请计算它们的和，并以**二进制字符串的形式输**出。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`。

根据题解，需要学习两个知识点：**StringBuilder是什么**，为什么要用他， stringbuilder.append是什么？以及三目运算符的使用; **字符串转整数的方法**

### StringBuilder是什么

官方文档

```
A mutable sequence of characters.This class provides an API compatible * with {@code StringBuffer}, but with no guarantee of synchronization. This class is designed for use as a drop-in replacement for{@code StringBuffer} in places where the string buffer was being used by a single thread (as is generally the case).   Where possible, it is recommended that this class be used in preference to {@code StringBuffer} as it will be faster under most implementations.
```

为了能**高效拼接字符串**，Java标准库提供了`StringBuilder`，它是一个**可变对象**，可以预分配缓冲区，这样，往`StringBuilder`中**新增字符时，不会创建新的临时对象**

![image-20211215110632201](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20211215110632201.png)

Hello, Mr Bob!

如果我们查看`StringBuilder`的源码，可以发现，进行链式操作的关键是，定义的**`append()`方法会返回`this`**，这样，就可以**不断调用自身的其他方法。**

```java
    public StringBuilder append(StringBuffer sb) {
        super.append(sb);
        return this;
    }

    @Override
    public StringBuilder delete(int start, int end) {
        super.delete(start, end);
        return this;
    }

    @Override
    public StringBuilder insert(int index, char[] str, int offset,
                                int len)
    {
        super.insert(index, str, offset, len);
        return this;
    }
```



### 字符串转整数

将字符转换为整数等同于**查找给定字符的 ASCII 值**（这是一个整数）。

- [1 char到int隐式类型转换](https://geek-docs.com/java/java-examples/char-to-int.html#charint)

```java
char ch = 'A';
int num = ch;
```

- [2 使用Character.getNumericValue()将char转换为int](https://geek-docs.com/java/java-examples/char-to-int.html#CharactergetNumericValuecharint)

此方法接受`char`作为参数，并在转换后**返回等效的`int`（ASCII）值**。

```java
char ch = 'P';
int num = Character.getNumericValue(ch);
```

- [3 使用Integer.parseInt()方法将char转换为int](https://geek-docs.com/java/java-examples/char-to-int.html#IntegerparseIntcharint)

```java
char ch = '9';
int num = Integer.parseInt(String.valueOf(ch));
```



本题运用char到int的隐式变换

因为数字的ASCII码值与数字的值不对应，所以需要减去0的ASCII码值

```java
public class addBinary {

    public String addBinary(String a, String b){
        StringBuilder stringBuilder = new StringBuilder();
        int carry = 0;
        int l1 = a.length() - 1;
        int l2 = b.length() - 1;

        while (l1>=0 || l2 >=0){
//            if (l1 < 0){
//                return 0;
//            } b
            int x = l1 < 0? 0 : a.charAt(l1) - '0'; //字符转整数
            int y = l2 < 0? 0 : b.charAt(l2) - '0';

            int sum = x + y + carry;
            stringBuilder.append(sum % 2);
            carry = sum / 2;

            l1--;
            l2--;

        }
        if (carry != 0) stringBuilder.append(carry);
        return stringBuilder.reverse().toString();
    }
}
```









