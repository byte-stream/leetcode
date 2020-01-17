## 题干：
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

## 解法：

1. O(n^2) / O(n)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        char[] array = s.toCharArray();
        List<Character> temp = new ArrayList<>();
        int ret = 0;
        for (int i = 0; i < array.length; i++){
            int max = 0;
            for (int j = i; j < array.length; j++){
                if(temp.contains(array[j])){
                    temp.clear();
                    ret = (max > ret) ? max : ret;
                    break;
                }else {
                    temp.add(array[j]);
                    max++;
                    ret = (max > ret) ? max : ret;
                }
            }
        }
        return ret;
    }
}
```

通过i指针来遍历字符数组，移动j指针，通过j指向的值来判断子字符串中是否出现重复字符。

2. O(n)? / O(n)

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
       List<Character> temp = new ArrayList<>();
        int j = 0;
        int max = 0;
        while (i < s.length() && j < s.length()){
            if (temp.contains(s.charAt(j))){
                temp.remove(0);
            }else {
                temp.add(s.charAt(j));
                max = Math.max(max, temp.size());
                j++;
            }
        }
        return max;
    }
}
```
在第一种方法上做了改进，在第一种方法中，两个指针是单独移动的，每移动一次来进行判断子字符串中是否出现重复字符，所以需要O(n^2)。

在这种方法中，使用了“滑动窗口”，简单来说就是同时移动两个指针。题解中使用的是HashSet这种库数据结构，然后通过HashSet的contains来判断当前字符是否存在子字符串中，这样时间复杂度就变为了O(1)。

真的是这样吗？



需要注意的是，ArrayList的contains方法，ArrayList的contains方法的实现如下：

```
public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
```

indexOf的实现如下：

```
public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```
还是遍历。

所以时间复杂度还是O(n^2)。

. O(n) / O(n)

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```
使用HashSet作为滑动窗口，才能到达O(1)的效果。

HashSet的底层实现是HashMap，contains方法也是调用的HashMap的containsKey方法：

```
 public boolean containsKey(Object key) {
        return getNode(hash(key), key) != null;
    }
```
getNode方法：

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

可以看到需要对key进行hash，然后再getNode，在getNode中

```
if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
```
直接通过hash值直接找到对应元素。

