# Question: Palindrome Number

> Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

Example 1:

``` txt
    Input: 121
    Output: true
```

Example 2:

``` txt
    Input: -121
    Output: false
    Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

Example 3:

```txt
    Input: 10
    Output: false
    Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

Follow up:

Coud you solve it without converting the integer to a string?

## Solution

From the description, we may know that converting the integer to a string would probably be the easiest way to solve this problem.

But how simple it could be? As I know you can solve it with one line code if you are using python. Code shows bellw:

``` python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```

Problem solved. But why not try something a bit more difficult. Let's solve it as the problem asks us to solve it without converting the integer to a string.

### Approach  1

Do you remember what I said before examples are very important? By the given case We can even write pseudocode directly.

By Example 2 we know that all negative integer can be ignored and directly return false.

By Example 3 we know that all integer end with zero can also be ignored.

Without these exclude cases. Normally we can get each digit of an integer with a loop which contains % 10 and / 10. then we can easily compare the first and last then the rest.

``` python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0 or x % 10 == 0 and x != 0:  # Notive don't forgot that integer 0 is a palindrome number
            return False
        digits = []  # data structure to store each digit

        while x: # loop to get each digits
            digits.append(x%10)
            x = x // 10

        # compare the head and tail then next util not matched return False
        for i in range(len(digits)//2):
            if digits[i] != digits[len(digits)-1-i]:
                return False

        # all digits matched is palindrome 
        return True
```

#### Run result

Runtime: 80 ms, faster than 27.17% of Python3 online submissions for Palindrome Number.

Memory Usage: 12.9 MB, less than 100.00% of Python3 online submissions for Palindrome Number.

#### Complexity Analysis

Time complexity : $O(log(n))$

Space complexity : $O(n)$

here is O(n) because I used an array to store each digits you also can create the revertedNumber while get each digits. Then the Space complexity would be $O(1)$
