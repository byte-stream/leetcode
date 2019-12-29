# [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

## 题目

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



## 解题代码及思路

**双层for循环**

我最开始就是这种思路.遍历每个元素,找到一个元素满足 nums[j] == target - nums[i] 这是可行解.

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[] { i, j };
                }
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```



**一遍哈希表**

思路见代码注解

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        //逐个取出nums[]中的元素.
        for(int i = 0; i<nums.length; i++){
            //算出符合条件的值.
            int temp = target - nums[i];
            if(map.containsKey(temp)){
                //map中如果有则找到可行解.
                return new int[]{i,map.get(temp)};
            }
            //map中没有可行解,则把该元素存入map.
            map.put(nums[i],i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

