1.判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:

输入: 121
输出: true
示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

//第一次刷题,看到这个题的时候，卧槽，这个题居然也懵逼。然后看了官方题解。卧槽，简单粗暴。
故，下面的代码算是将官方的代码默写了一遍，尴尬得雅痞

class Solution {
public:
    bool isPalindrome(int x) {
        if (x<0 || (x%10==0 && x!=0))
        {
            return false;
        }
        int temp=0;
        while (x>temp)
        {
            temp=temp*10+x%10;
            x/=10;
        }
        return x==temp || x==temp/10;
    }
};

实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

示例 1:

输入: haystack = "hello", needle = "ll"
输出: 2
示例 2:

输入: haystack = "aaaaa", needle = "bba"
输出: -1

//看到这个题，给我的第一感觉也是似曾相识，这些简单题，之前确实刷过,之前用C语言实现过，下面给出两种思路
//暴力遍历法
class Solution {
public:
    int strStr(string haystack, string needle) {
       int i=0,j=0;
     
       while (i<haystack.size() && j<needle.size())
       {
           if (haystack[i]==needle[j])
           {
               ++i;
               ++j;
           }
           else
           {
               i=i-j+1;
               j=0;
           }
       }
       if (j==needle.size())
       {
           return i-j;
       }
       else
       {
           return -1;
       }

    }
};

//利用C++现有库函数,这种方法，内存开销比较大
class Solution {
public:
    int strStr(string haystack, string needle) {
        if (haystack.size()==0&&needle.size()==0)
        {
            return 0;
        }
        for (int i=0;i<haystack.size();++i)
        {
            if (haystack.substr(i,needle.size())==needle)
            {
                return i;
            }
        }
        return -1;
    }
};
