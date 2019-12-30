## 题干：
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 解法：

1. O(n^2) / O(1)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int ret[] = new int[2]; 
        for(int i = 0; i < nums.length; i++){
            for(int j = i + 1; j < nums.length; j++){
                if (nums[i] + nums[j] == target) {
                    ret[0] = i;
                    ret[1] = j;
                }
            }
        }
        return ret;
    }
}

```
遇事不决，暴力破解。


2. O(n) / O(n)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap();
        for (int i = 0; i < nums.length; i++){
            int ret = target - nums[i];
            if(map.containsKey(ret)){
                return new int[] {i, map.get(ret)};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```
通过哈希函数将一轮查找优化为O(1),这里依赖了jdk的HashMap，不过我个人觉得这样不够纯粹，应该自己手写散列函数(**FLAG1**)

此外，主要依赖了containsKey这个内置方法，可以看到该方法的源码为：

```java
final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }


```
可以看到期间还调用了getTreeNode方法，内部又调用了find方法等，严格来说，若要使用该解法，这种都应该自己实现。什么时候读一下HashMap的源码（**FLAG2**）然后自己再仿照着实现一个。
