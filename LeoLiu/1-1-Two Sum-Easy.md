# Question: Two Sum

> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
> You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

``` 
  Given nums = [2, 7, 11, 15], target = 9,
  Because nums[0] + nums[1] = 2 + 7 = 9,
  return [0, 1].
```

# Solution

## Approach  1
For every question, we will try using Brute Force first. (if it's good enough aka AC why use any complex algorithms)
From the question descriptions, we can know that all we have to do is traverse the data and get one pair of numbers which add up to the target number.

So to traverse the data we need a for-loop. And to get the matched number we need another for-loop inside.

Here is the code write in Python. By the way all code in this folder will be Python
``` python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)): # traverse the data
            for j in range(i + 1, len(nums)): # traverse the rest of data to get matched num; i + 1 to not use the same element twice
                if nums[i] + nums[j] == target: # add up to the target
                    return [i, j]
```
### Run result
Runtime: 6020 ms, faster than 7.88% of Python3 online submissions for Two Sum.

Memory Usage: 13.6 MB, less than 95.81% of Python3 online submissions for Two Sum.

### Complexity Analysis

Time complexity : O(N^2)

Space complexity : O(1)

## Approach 2
The memory usage for approach 1 is good. but about runtime I think it could never be worse. So let's try to improve it.
From the basic knowledge of algorithms, we know that the time complexity O(N^2) is because of the nested loop. So to make the time complexity to O(N).  we need to find the matched number when traversing the data.
For this purpose, we need extra space to store matched numbers information, which is what we usually say " More space complexity, less time complexity".

Here is the code:
```Python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dictionary = {} # create dictionary
        for i,element in enumerate(nums):  # traverse the data
            if element in dictionary:  # check the element already be the key in dictionary or not
                return [d[element], i]  # if found return the matched element pair index
            dictionary[target-element]=i  # store the matched number infomation
```
The changes from the last time code to this time may be a bit too big. Let me explain briefly what we did this time.
While we traverse the data we put the elements into a dictionary. And to store that matched number information when we put elements into the dictionary.
we use target value minus element value which is equal to the matched numbers for the element as the key, index as value.
So during we traverse the data if one element is already a key in the dictionary, It means that we found the matched pair.
then we just return the matched pairs indexes. (one is the value stored in the dictionary using current data as the key, one is the current traverse data index)


### Run result
Runtime: 40 ms, faster than 99.18% of Python3 online submissions for Two Sum.

Memory Usage: 14 MB, less than 65.35% of Python3 online submissions for Two Sum.

### Complexity Analtsis

Time complexity : O(N)

Space complexity : O(N)

Could we get better?
Because to find the matched pair we at least need to traverse all the data. which mean that the Time complexity couldn't be less than O(N). 

