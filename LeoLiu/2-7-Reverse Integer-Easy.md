# Question: Reverse Interger

> Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

``` txt
    Input: 123
    Output: 321
```

Example 2:

``` txt
    Input: -123
    Output: -321
```

Example 3:

``` txt
    Input: 120
    Output: 21
```

Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Solution

First, let's investigate the requirement of this problem. The problem is very friendly because of three useful examples and helpful note(Note is a very important part of the problem. In any case you shouldn't skip it). with this given information, we know that we solution should include the following cases: the negative integer, the heading zero of the reversed integer, reversed integer overflows.

Normally, for this problem, there are two ways to solve it. The first one is to convert it to string then do the reverse. The second one is to use modulus to get one digit from the bottom at a time then append it to the result.

### Approach  1 Modulus and append

``` python
class Solution:
    def reverse(self, x: int) -> int:
        return self._check(self._rev(x))

    def _rev(self, x: int) -> int:
        flag = -1 if x < 0 else 1  # check x in negative or not
        result = 0
        x = x * fla  # make the x as positive
        while x:
            result = result * 10 + x % 10  # get each digital
            x= x//10
        return result * flag  # append the digital we get

    def _check(self, x: int) -> int:
        return x if abs(x) < 2**31 -1 else 0  # check is overflowed or not
```

#### Run result

Runtime: 28 ms, faster than 89.75% of Python3 online submissions for Reverse Integer.

Memory Usage: 12.7 MB, less than 100.00% of Python3 online submissions for Reverse Integer.

#### Complexity Analysis

Time complexity : O(log(N))

Space complexity : O(1)

### Approach 2 Convert to string

```Python
class Solution:
    def reverse(self, x: int) -> int:
        if x > 0 :
            ans = int(str(x)[::-1])
        else:
            ans = int("-"+ str(abs(x))[::-1])  ## if the integer is negative number skip the first char(-) and add it to head after reverse

        return ans if abs(ans) < 2**31 - 1 else 0  # overflow check
```

#### Run result

Runtime: 16 ms, faster than 99.94% of Python3 online submissions for Reverse Integer.
Memory Usage: 12.7 MB, less than 100.00% of Python3 online submissions for Reverse Integer.

#### Complexity Analtsis

Time complexity : O(log(N))

Space complexity : O(N)
