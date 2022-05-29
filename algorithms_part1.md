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

## 19. Remove Nth Node From End of List

Given the head of a linked list, remove the nth node from the end of the list and return its head.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        # print('New execution')
        if not head.next:
            return None
        prevhead = head
        count = 0
        while prevhead:
            count = count + 1
            prevhead = prevhead.next
        # print(count)
        prevhead = head
        cnt = 0
        while prevhead:
            if ((count - n) - 1 == cnt):
                # print(prevhead.val)
                thirdhead = prevhead.next
                if thirdhead:
                    prevhead.next = thirdhead.next
                else:
                    prevhead.next = None
            cnt = cnt + 1
            prevhead = prevhead.next
        if count == n:
            prevhead = head
            return head.next
        return head
```

## 3. Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without repeating characters.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        # d v d f 
        # start = 0, end = 0, sum = 1
        # start = 0, end = 1, sum = 2
        # start = 1, end = 2, sum = 2
        # start = 1, end = 3, sum = 3
        if len(s) == 1:
            return 1
        letter_dict = {}
        max_sum = 0
        start = 0
        for end in range(len(s)):
            # print('end is ' + str(end))
            
            if s[end] in letter_dict:
                start = max(start, letter_dict[s[end]] + 1)
            letter_dict[s[end]] = end
            max_sum = max(max_sum, end - start + 1)
            # print('start is ' + str(start))
            # print('max_sum is ' + str(max_sum))
        return max_sum
```

## 567. Permutation in String

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise. In other words, return true if one of s1's permutations is the substring of s2.

```python
from collections import defaultdict
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s1) > len(s2):
            return False
        dict_s1, dict_s2 = {},{}
        for i in range(97,123):
            dict_s1[chr(i)],dict_s2[chr(i)] = 0,0
        for i in range(len(s1)):
            dict_s1[s1[i]] =   dict_s1[s1[i]] + 1
            dict_s2[s2[i]] =   dict_s2[s2[i]] + 1
        for i in range(len(s2) - len(s1)):
            if dict_s1 == dict_s2:
                return True
            # print(s2[i])
            # print(s2[i + len(s1)])
            dict_s2[s2[i + len(s1)]] = dict_s2[s2[i + len(s1)]] + 1
            dict_s2[s2[i]] = dict_s2[s2[i]] - 1
            # print(dict_s2)
        if dict_s1 == dict_s2:
            return True
        return False
```

## 733. Flood Fill

An image is represented by an m x n integer grid image where image[i][j] represents the pixel value of the image. You are also given three integers sr, sc, and newColor. You should perform a flood fill on the image starting from the pixel image[sr][sc].To perform a flood fill, consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with newColor. Return the modified image after performing the flood fill.

```python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
        rows , cols = len(image), len(image[0])
        color = image[sr][sc]
        if color == newColor:
            return image
        def dfs(r, c):
            if image[r][c] == color:
                image[r][c] = newColor
                if r >= 1 : # top
                    dfs(r - 1, c)
                if r < rows - 1 : # bottom
                    dfs(r + 1, c)
                if c >= 1 : # left
                    dfs(r, c -1)
                if c < cols - 1 : # right
                    dfs(r, c + 1)
        dfs(sr, sc)
        return image
```

