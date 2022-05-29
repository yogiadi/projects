# Algorithms Part - I

## 704. Binary Search
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1. You must write an algorithm with O(log n) runtime complexity.

```python
def binarysearch(li, low, high, target):
    if low <= high:
        # print(low)
        # print(high)
        mid = (low + high)//2
        # print(mid)
        if li[mid] == target:
            # print('first')
            return mid
        elif li[low] == target:
            # print('second')
            return low
        elif li[high] == target:
            # print('third')
            return high
        elif li[mid] < target and target < li[high] and low < mid:
            # print('fourth')
            return binarysearch(li, mid, high, target)
        elif li[mid] > target and target > li[low] and high > mid:
            # print('fifth')
            return binarysearch(li, low, mid, target)
        else:
            return -1
    else:
        return -1
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        return binarysearch(nums,0,len(nums) -  1, target )
```
## 278. First Bad Version
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad. Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad. You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

```python
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:
def badversions(low, high):
    if not isBadVersion(low) and isBadVersion(high) and low == high - 1:
        return high
    elif not isBadVersion(low) and isBadVersion(high):
        # print('in first case ' + str(low) + ' ' + str(high))
        return badversions((low+high)//2, high)
    elif isBadVersion(low) and isBadVersion(high) and low != high:
        # print('in fourth case ' + str(low) + ' ' + str(high))
        return badversions(1,low)
    elif isBadVersion(low) and isBadVersion(high):
        # print('in second case ' + str(low) + ' ' + str(high))
        return low
class Solution:
    def firstBadVersion(self, n: int) -> int:
        return badversions(1, n)
```

## 35. Search Insert Position
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order. You must write an algorithm with O(log n) runtime complexity.

```python
def binarysearch(nums, low, high, target):
    mid = (low+high)//2
    if target > nums[high]:
        return high + 1
    elif target < nums[low]:
        return 0
    elif nums[low] == target:
        return low
    elif nums[high] == target:
        return high
    elif nums[mid] == target:
        return mid
    elif low < high and low != high - 1:
        if nums[mid] < target and nums[high] > target:
            return binarysearch(nums,mid, high, target)
        elif nums[mid] > target and nums[low] < target:
            return binarysearch(nums, low, mid, target)   
    else:
        return low + 1
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        return binarysearch(nums, 0 , len(nums) - 1, target)
```

## 977. Squares of a Sorted Array
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        nums_squared = []
        for i in nums:
            nums_squared.append(i*i)
        nums_squared.sort()
        return nums_squared
```
## 189. Rotate Array
Given an array, rotate the array to the right by k steps, where k is non-negative.
```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
         # 0,1,2,3,4,5,6
        # [1,2,3,4,5,6,7] 
        # [5,6,7,1,2,3,4]
        # nums[0] = nums[4] #(7 - 3) 
        # nums[1] = nums[5] #(7 - 3 + 1)
        # nums[2] = nums[6] #(7 - 3 + 2)
        # nums[3] = nums[0] # 3 - (7 - 3 - 1)
        # nums[4] = nums[1] # 4 - (7 - 3 - 1)
        # nums[5] = nums[2] # 5 - (7 - 3 - 1) 
        # nums[6] = nums[3] # 6 - (7 - 3 - 1)
        # for i in range(len(nums)):
        #     print(i)
        #     if i < len(nums) - k - 1:
        #         print(len(nums) - k + i)
        #         nums[i] = nums[len(nums) - k + i]
        #     else:
        #         nums[i] = nums[i - (len(nums) - k - 1)]
        # print(nums)
        for i in range(k):
            k = nums.pop(-1)
            print(k)
            nums.insert(0,k)
        print(nums)
```

## 283. Move Zeroes

Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements. Note that you must do this in-place without making a copy of the array.

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        new_list = []
        zero_list = []
        for i in range(len(nums)):
            # print(nums[i])
            # zero_list = []
            if nums[i] == 0:
                # print('in if')
                zero_list.append(0)
            else:
                new_list.append(nums[i])
        nums[:] = new_list + zero_list
```

## 167. Two Sum II - Input Array Is Sorted

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length. Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2. The tests are generated such that there is exactly one solution. You may not use the same element twice. Your solution must use only constant extra space.

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        hashmap = {}
        for i in range(len(numbers)):
            complement = target - numbers[i]
            if complement in hashmap:
                return [hashmap[complement] + 1 , i+ 1]
            hashmap[numbers[i]] = i
```

## 344. Reverse String

Write a function that reverses a string. The input string is given as an array of characters s. You must do this by modifying the input array in-place with O(1) extra memory.

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        li = []
        for i in reversed(s):
            k = s.pop(-1)
            # print(k)
            li.append(k)
            # print(i)
        s[:] = li[:]
```

## 557. Reverse Words in a String III
Given a string s, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s1 = ""
        str_list = s.split(' ')
        for i in range(len(str_list)):
            each_str = ""
            for j in reversed(list(str_list[i])):
                each_str = each_str + j
            if i == len(str_list) - 1:
                s1 = s1 + each_str
            else:
                s1 = s1 + each_str + ' '
        return s1
```

## 876. Middle of the Linked List

Given the head of a singly linked list, return the middle node of the linked list.If there are two middle nodes, return the second middle node.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prevhead = head
        count = 0
        while prevhead:
            count = count + 1
            prevhead = prevhead.next
        print(count)
        cnt = 0
        prevhead = head
        while prevhead:
            cnt = cnt + 1
            if cnt == count//2 + 1:
                return prevhead
            prevhead = prevhead.next
```
