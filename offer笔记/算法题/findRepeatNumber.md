数组中重复的数字

**在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内**。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        HashSet<Integer> integers = new HashSet<>();
        for (int num : nums) {
            if (integers.contains(num)) return num;
            integers.add(num);
        }
        return -1;
    }
}
```

HashSet 基于 HashMap 来实现的，是一个**不允许有重复元素的集合**。

HashSet 允许有 null 值。

HashSet 是**无序的**，即不会记录插入的顺序。

HashSet **不是线程安全的**， 如果多个线程尝试同时修改 HashSet，则最终结果是不确定的。 您必须在多线程访问时显式同步对 HashSet 的并发访问。

HashSet 实现了 **Set 接口**。 **Set 接口**能干吗：HashSet继承Set及成果Collection继承Iterable

HashSet 类提供类很多有用的方法，添加元素可以使用 add() 方法:

我们可以使用 **contains() 方法来判断元素是否存在于集合**当中:

我们可以使用 remove() 方法来删除集合中的元素:

删除集合中所有元素可以使用 clear 方法：

如果要计算 HashSet 中的元素数量可以使用 size() 方法：

可以使用 **for-each 来迭代 HashSet 中的元素**。

```java
for (String i : sites) {
            System.out.println(i);
        }
```



#### 方法二：原地交换

因为长度为 n 的数组 nums 里的所有**数字都在 0～n-1 的范围内**

`在一个长度为 n 的数组 nums 里的所有数字都在 0 ~ n-1 的范围内` 。 此说明含义：数组元素的 **索引** 和 **值** 是 **一对多** 的关系。因而，就能通过索引映射对应的值，起到与字典等价的作用。

算法流程：
遍历数组 nums ，设索引初始值为 i = 0:

若 nums[i] = i： 说明此数字已在对应索引位置，无需交换，因此跳过；
若 nums[nums[i]] = nums[i]： **代表索引 nums[i]处和索引 i 处的元素值都为 nums[i]**，即找到一组重复值，返回此值 nums[i]；
否则： 交换索引为 i 和 nums[i]的元素值，将此数字交换至对应索引位置。
若遍历完毕尚未返回，则返回 −1 。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int i = 0;
        while(i < nums.length) {
            if(nums[i] == i) {
                i++;
                continue;
            }
            if(nums[nums[i]] == nums[i]) return nums[i];
            int tmp = nums[i];
            nums[i] = nums[tmp];
            nums[tmp] = tmp;
        }
        return -1;
    }
}
```



































