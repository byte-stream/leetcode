# Question:  Longest Common Prefix

> Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:

``` txt
    Input: ["flower","flow","flight"]
    Output: "fl"
```

Example 2:

``` txt
    Input: ["dog","racecar","car"]
    Output: ""
    Explanation: There is no common prefix among the input strings.
```

Note:

All given inputs are in lowercase letters a-z.

## Solution

This problem can be said to be the super-simplified longest common subsequence problem. Because it fixed the common substring to the prefix. On leetcode.com 's solution, there are many approaches. Many of them I feel overused. This is just an EASY question. The method I use is to first use the first string in the data as the prefix. Then compare the prefix length with other string to get the common part for these two string. Then repeat it until all data traversed. The final common part is our answer. During traversing the data the common prefix length will get shorter and shorter.

### Approach

``` python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if strs:  # check strs is empty or not
            common = strs[0]
        else:
            return ""  # if empty then common part is ''
        for s in strs:
            length = len(common)
            while s[:length] != common[:length]:  # loop util find the common
                length -= 1  # try short prefix
            common = common[:length] # assign the value only once for each str to reduce runtime cost

        return common

```

#### Run result

Runtime: 24 ms, faster than 96.88% of Python3 online submissions for Longest Common Prefix.

Memory Usage: 12.8 MB, less than 100.00% of Python3 online submissions for Longest Common Prefix.

#### Complexity Analysis

Time complexity : $O(n)$

Space complexity : $O(1)$
