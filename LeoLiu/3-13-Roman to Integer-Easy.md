# Question: Roman to Integer

> Roman numerals are represented by seven different symbols: $I$, $V$, $X$, $L$, $C$, $D$ and $M$.

<center>

| Symbol | Value |
| :----: | :---: |
|   $I$    |   1   |
|   $V$    |   5   |
|   $X$    |  10   |
|   $L$    |  50   |
|   $C$   |  100  |
|   $D$    |  500  |
|   $M$    | 1000  |

</center>

> For example, two is written as $II$ in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply $X$ + $II$. The number twenty seven is written as $XXVII$, which is $XX$ + $V$ + $II$.
>
> Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:
>
> * $I$ can be placed before $V$ (5) and $X$ (10) to make 4 and 9.
> * $X$ can be placed before $L$ (50) and $C$ (100) to make 40 and 90.
> * $C$ can be placed before $D$ (500) and $M$ (1000) to make 400 and 900.
>
> Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

Example 1:

``` txt
    Input: "III"
    Output: 3
```

Example 2:

``` txt
    Input: "IV"
    Output: 4
```

Example 3:

``` txt
    Input: "IX"
    Output: 9
```

Example 4:

``` txt
    Input: "LVIII"
    Output: 58
    Explanation: L = 50, V= 5, III = 3.
```

Example 5:

``` txt
    Input: "MCMXCIV"
    Output: 1994
    Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## Solution

Before we start coding, let's analyze the problem first.

The first part introduces the Roman numerals symbols system. The rest part gives us **FIVE** very helpful examples.

To solve the problem we need instore the roman numerals symbols and related values. In the description, this information was given in a two-column table.

Of course, we can store it with a two-dimensional array. But don't you feel this data structure is the symbol of another base data type -- dictionary.

By store symbol and value in the dictionary, we can easily get the value by the symbol. ( instead of traverse the array compare the symbol then get the value)

So now what we need to do is to traverse the given roman numerals string and get related value by the symbols we store. Oh, wait a minute, we missed these part:

> Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX.

The following two approachs are two ways to solve it.

### Approach  1

We know that for the problem there are six instances where subtaction is bsed. `IV IX XL XC CD CM`

Can we add these six cases to the dictionary as separate symbols and process the remaining seven symbols together？

The answer is Yes. The answer is Yes. But we need to spend some effort to solve the two chars symbol.

``` python
class Solution:
    def romanToInt(self, s: str) -> int:
        result = 0
        dictionary = {'CM':900, 'CD':400, 'M':1000, 'D':500, 'C':100, 'XC': 90, 'XL':40, 'L':50, 'X':10, 'IX': 9, 'IV': 4, 'V':5, 'I':1} # add six instances where subtraction is used as a symbol
        index = 0
        length = len(s)
        while index < length: # exit while all symbol has traversed
            if index + 1 < length and s[index:index+2] in dictionary: # index + 1 check to avoid out of index error when index equal to the length - 1
                result += dictionary[s[index:index+2]] # use the two char symbol(subtraction case)
                index += 1 # because it's two char symbol move one more step
            else:
                result += dictionary[s[index]] # normal symbol

            index += 1 # to traverse next symbol

        return result
```

#### Run result

Runtime: 36 ms, faster than 97.53% of Python3 online submissions for Roman to Integer.

Memory Usage: 12.9 MB, less than 100.00% of Python3 online submissions for Roman to Integer.

#### Complexity Analysis

Time complexity : $O(n)$

Space complexity : $O(1)$

### Approach  2

In the above approach, we put the six subtraction cases into the symbol dictionary to solve the problem. But it also increases the difficulty.(we need to check if the next two symbol in the dictionary first)

Here is another way you can take to solve the problem.

I don't know did you notice this sentence or not.
> Roman numerals are usually written largest to smallest from left to right.

And this sentence also.
> The number four is written as $IV$. Because the one is before the five we subtract it making four.

By these two important sentences, we can know that when a symbol is smaller than the symbol follow it, It means the value which this symbol related to should be subtracted from the total.

``` python
class Solution:
    def romanToInt(self, s: str) -> int:
        result = 0
        pending = 0
        dictionary = {'M': 1000, 'D': 500, 'C': 100, 'L': 50, 'X': 10, 'V': 5, 'I': 1}  # we no longer need to store that six instances where subtraction is used
        for ch in s:
            current = dictionary[ch]  # get value from dictionary
            if pending != 0:  # skip the begining case
                result += pending if pending >= current else - pending  # the key: when following symbol's related value is larger the value should be subtracted from total sum
            pending = current   # put value in to pend we need the next symbol to tell us this value should be added or subtracted.

        result += pending # add last symbol's value

        return result

```

#### Run result

Runtime: 36 ms, faster than 97.53% of Python3 online submissions for Roman to Integer.

Memory Usage: 12.9 MB, less than 100.00% of Python3 online submissions for Roman to Integer.

#### Complexity Analysis

Time complexity : $O(n)$

Space complexity : $O(1)$

### Approach  3

This is a quite special approach. let's rethink about the instances where subtraction is used, what will happen if we ignore this rule.

I will use the Example 5 's value:
> Input: "MCMXCIV"
>
> The right result is 1994.
>
> Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

If we ignore the subtraction case.

> The result will be 2216
>
>Explanation: M = 1000, C= 100, M = 1000, X = 10 ,C = 100 and I = 1 V = 5.
>
> The diff would be 2216 - 1994 = 222
 222 in roman numeral: C= 100, C=100, X =10, X=10, I = 1, I =1.

Now, everyone must have found that if we ignore the subtraction, we will add the symbol's value( first symbol of six instances where subtraction is used.) which we should actually subtract.

So in this approach, the idea is we when we traverse the data we ignore the subtraction case to simplify the question. After we get the sum result, we reduce the values which we added by mistake  twice(For right case we should subtract that value but we add it in the case, so we need reduce twice) from the result.

``` python
class Solution:
    def romanToInt(self, s: str) -> int:
        result = 0
        dictionary = {'M': 1000, 'D': 500, 'C': 100, 'L': 50, 'X': 10, 'V': 5, 'I': 1}
        for ch in s:
            result += dictionary[ch]  # ignore the subtraction case 

        if 'CM' in s or 'CD' in s: result -= 200  # 200 mean twice of symbol C's value
        if 'XC' in s or 'XL' in s: result -= 20  # 20 mean twice of symbol X's value
        if 'IX' in s or 'IV' in s: result -= 2  # 2 mean twice of symbol I's value

        return result

```

#### Run result

Runtime: 36 ms, faster than 97.53% of Python3 online submissions for Roman to Integer.

Memory Usage: 12.8 MB, less than 100.00% of Python3 online submissions for Roman to Integer.

#### Complexity Analysis

Time complexity : $O(n)$

Space complexity : $O(1)$