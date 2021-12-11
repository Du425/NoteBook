题目要求：给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 **修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

说明：为什么返回数值是整数，但输出的答案是数组呢?

​			输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。引用是根据index找到对应的元素？

个人疑惑：原地删除用什么实现？栈？ **双指针**

曾经用python写的：

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0

        n = len(nums)
        fast = slow =1
        while(fast < n):
            if nums[fast] != nums[fast-1]:
                nums[slow] = nums[fast]
                slow +=1
            fast +=1

        return slow
```



为何排列无效**：因为数组是有序的，需要提前排好序才能起作用！！！**

```java
public class RemoveDuplicates {
    public int removeDuplicates(int[] nums){
        int n = nums.length;
        if (n == 0){
            return 0;
        }
        int fast = 1;
        int slow = 0;
        while (fast<n){
            if (nums[fast] != nums[fast-1]) {
                nums[slow+1] = nums[fast];
                slow ++;
            }
            fast ++;
        }
//        for (int i=0; i<n; i++){
//            if (fast == n){
//                break;
//            }
//            if (nums[fast] == nums[slow]){
//                fast++;
//            }else {
//                nums[slow+1] = nums[fast];
//                slow++;
//                fast++;
//            }
//        }
        return slow+1;
    }
    public static void main(String[] args){
        int[] a = {0,0,1,1,3,4,5,5};
        RemoveDuplicates removeDuplicates1 = new RemoveDuplicates();
        int count = removeDuplicates1.removeDuplicates(a);
        System.out.println(count);
        int i = 0;
        while (i < count){
            System.out.println(a[i]+" ");
            i++;
        }
    }
}
```















































