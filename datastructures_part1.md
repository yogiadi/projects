# Data Structures - Part I

## 217. Contains Duplicate
<em>Hash Table</em>
<em>Sorting</em>
<em>Array</em>

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        from collections import defaultdict
        di = defaultdict(int)
        for i in nums:
            di[i] = di[i] + 1
        for key, val in di.items():
            if val >= 2:
                return True
        return False   
 ```
 
 ## 53. Maximum Subarray
<em>Divide and Conquer</em> 
<em>Array</em>

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum. A subarray is a contiguous part of an array.

```python
import math
def max_cross_sub_array(li, low, mid, high):
    left_sum = -math.inf
    sum = 0
    for i in range(mid, low - 1, -1 ):
        sum = sum + li[i]
        if sum > left_sum:
            left_sum = sum
            max_left = i
    right_sum = -math.inf
    sum = 0
    for i in range(mid + 1, high + 1):
        sum = sum + li[i]
        if sum > right_sum:
            right_sum = sum
            max_right = i
    overall_sum = left_sum + right_sum
    return (max_left, max_right, left_sum + right_sum)
def max_sub_array(li, low, high):
    if high == low:
        return (low, high, li[low])
    else:
        mid = (low + high)//2
        (left_low, left_high, left_sum) = max_sub_array(li, low, mid)
        (right_low, right_high, right_sum) = max_sub_array(li, mid + 1, high)
        (cross_low, cross_high, cross_sum) = max_cross_sub_array(li, low, mid, high)
        if left_sum >= right_sum and left_sum >= cross_sum:
            return (left_low, left_high, left_sum)
        elif right_sum >= left_sum and right_sum >= cross_sum:
            return (right_low, right_high, right_sum)
        else:
            return (cross_low, cross_high, cross_sum)
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        (lows, highs, sums) = max_sub_array(nums,0, len(nums) - 1)
        return sums

```

## 1. Two Sum
<em>Hash Table</em>
<em>Array</em>

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement in hashmap:
                return [i, hashmap[complement]]
            hashmap[nums[i]] = i
```



