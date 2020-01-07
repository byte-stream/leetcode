# Question: Valid Parentheses

> Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

    1. Open brackets must be closed by the same type of brackets.
    2. Open brackets must be closed in the correct order.

Example 1:

        Input: "()"
        Output: true

Example 2:

        Input: "()[]{}"
        Output: true

Example 3:

        Input: "(]"
        Output: false

Example 4:

        Input: "([)]"
        Output: false

Example 5:

        Input: "{[]}"
        Output: true

Note:
    That an empty string is also considered valid.

## Solution

The [website](https://leetcode.com/problems/valid-parentheses/solution/) gave very well solution and explanation. So I don't want to duplicate it here.

### Approach  

``` python
class Solution:
    def isValid(self, s: str) -> bool:
        pending = []
        parenthesis_match = {')':'(','}':'{',']':'['}
        for c in s:
            if c not in parenthesis_match:
                pending.append(c)
            elif pending and parenthesis_match[c] == pending[-1]:
                pending.pop()
            else:
                return False
        return not bool(pending)
```

#### Run result

Runtime: 16 ms, faster than 99.61% of Python3 online submissions for Valid Parentheses.

Memory Usage: 12.7 MB, less than 100.00% of Python3 online submissions for Valid Parentheses.

#### Complexity Analysis

Time complexity : $O(n)$

Space complexity : $O(n)$
