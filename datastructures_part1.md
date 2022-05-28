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
## 88. Merge Sorted Array

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.Merge nums1 and nums2 into a single array sorted in non-decreasing order.The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i, j, k  = 0, 0, 0
        nums3 = []
        while k< m + n and i<= m:
        # for pp in range(10):
            if i == m and j != n:
                nums3.append(nums2[j])
                j = j + 1
                k = k + 1
            elif j == n and i != m:
                nums3.append(nums1[i])
                i = i + 1
                k = k + 1
            elif nums1[i] <= nums2[j]:
                nums3.append(nums1[i])
                i = i + 1
                k = k + 1
            elif nums2[j] <= nums1[i]:
                nums3.append(nums2[j])
                j = j + 1
                k = k + 1
        nums1[:]=nums3[:]
```
## 350. Intersection of Two Arrays II
Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.
```python
def defaultvaluelist():
    return [0,0]
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        from collections import defaultdict
        di = defaultdict(defaultvaluelist)
        for i in nums1:
            di[i][0] = di[i][0] + 1
        for j in nums2:
            di[j][1] = di[j][1] + 1
        final_list = []
        for key, val in di.items():
            if val[0] != 0 and val[1] != 0:
                for j in range(min(val[0], val[1])):
                    final_list.append(key)
        return final_list
```
## 121. Best Time to Buy and Sell Stock
You are given an array prices where prices[i] is the price of a given stock on the ith day.You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.
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
    def maxProfit(self, prices: List[int]) -> int:
        nums = []
        for i in range(len(prices)):
            if i != len(prices) - 1:
                nums.append(prices[i+1] - prices[i])
        if len(prices) == 1:
            sums=0
        else:
            (lows, highs, sums) = max_sub_array(nums,0, len(nums) - 1)
        if sums < 0:
            sums = 0
        return sums
```
## 566. Reshape the Matrix
In MATLAB, there is a handy function called reshape which can reshape an m x n matrix into a new one with a different size r x c keeping its original data. You are given an m x n matrix mat and two integers r and c representing the number of rows and the number of columns of the wanted reshaped matrix. The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were. If the reshape operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.
```python
class Solution:
    def matrixReshape(self, mat: List[List[int]], r: int, c: int) -> List[List[int]]:
        li = []
        for i in mat:
            for j in i:
                li.append(j)
        new_list = []
        k = 0
        print(len(li))
        print(r*c)
        if len(li) == r * c:
            for i in range(r):
                new_list.append([])
                for j in range(c):
                    new_list[i].append(li[k])
                    k = k + 1
        else:
            return mat
        return new_list
```
## 118. Pascal's Triangle
Given an integer numRows, return the first numRows of Pascal's triangle. In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:
```python
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        li = []
        for i in range(numRows):
            li.append(list(str(int(math.pow(11,i)))))
        print(li[:numRows])
        if numRows <= 5:
            return li[:numRows]
        li[:] = li[:5]
        for i in range(numRows - 5):
            last_arr = li[-1]
            print(last_arr)
            li.append([1])
            for j in range(len(last_arr)-1):
                li[-1].append(int(last_arr[j]) + int(last_arr[j+1]))
                print(li[-1])
            li[-1].append(1)
            print(li)
        return li
```
## 36. Valid Sudoku
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.
```python
def box_classify(i,j):
    if i <= 2 and j <= 2:
        return 1
    elif i <= 2 and j <= 5:
        return 2
    elif i <= 2 and j <= 8:
        return 3
    elif i <= 5 and j <= 2:
        return 4
    elif i <= 5 and j <= 5:
        return 5
    elif i <= 5 and j <= 8:
        return 6
    elif i <= 8 and j <= 2:
        return 7
    elif i <= 8 and j <= 5:
        return 8
    elif i <= 8 and j <= 8:
        return 9
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        elem_dict = {}
        for i in range(len(board)):
            for j in range(len(board)):
                if board[i][j] not in elem_dict and board[i][j] != ".":
                    elem_dict[board[i][j]] = [[i],[j], [box_classify(i,j)]]
                elif board[i][j] != ".":
                    if i not in elem_dict[board[i][j]][0] and j not in elem_dict[board[i][j]][1] and box_classify(i,j) not in elem_dict[board[i][j]][2]:
                        elem_dict[board[i][j]][0].append(i)
                        elem_dict[board[i][j]][1].append(j)
                        elem_dict[board[i][j]][2].append(box_classify(i,j))
                    else:
                        return False
        # print(elem_dict)
        return True
    # sub box 1 - (0,0),(0,2),(2,0)(2,2)
    # sub box 2 - (0,3),(0,5),(2,3),(2,5)
    # sub box 3 - (0,6),(0,8),(2,6),(2,8)
    # sub box 4 - (3,0),(3,2),(5,0),(5,2)
    # sub box 5 - (3,3),(3,5),(5,3),(5,5)
    # sub box 6 - (3,6),(3,8),(5,6),(5,8)
    # sub box 7 - (6,0),(6,2),(8,0)(8,2)
    # sub box 8 - (6,3),(6,5),(8,3),(8,5)
    # sub box 9 - (6,6),(6,8),(8,6),(8,8)
```
## 74. Search a 2D Matrix
Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:
Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        for row in matrix:
            if target >= row[0] and target <= row[-1]:
                for elem in row:
                    if target == elem:
                        return True
        return False        
```
## 387. First Unique Character in a String
Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.
```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        counter_dict = {}
        for i in range(len(s)):
            if s[i] in counter_dict:
                counter_dict[s[i]][0] = counter_dict[s[i]][0] + 1
            else:
                counter_dict[s[i]] = [1, i]
        print(counter_dict)
        for key, val in counter_dict.items():
            if val[0] == 1:
                return val[1]
        return -1
```
## 383. Ransom Note
Given two strings ransomNote and magazine, return true if ransomNote can be constructed from magazine and false otherwise. Each letter in magazine can only be used once in ransomNote.
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        from collections import defaultdict
        magazine_dict = defaultdict(int)
        ransomnote_dict = defaultdict(int)
        for elem in ransomNote:
            ransomnote_dict[elem] = ransomnote_dict[elem] + 1
        for elem in magazine:
            magazine_dict[elem] = magazine_dict[elem] + 1
        for key, val in ransomnote_dict.items():
            if key not in magazine_dict or val > magazine_dict[key]:
                return False
        return True
```
## 242. Valid Anagram
Given two strings s and t, return true if t is an anagram of s, and false otherwise. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import defaultdict
        s_dict = defaultdict(int)
        t_dict = defaultdict(int)
        for elem in s:
            s_dict[elem] = s_dict[elem] + 1
        for elem in t:
            t_dict[elem] = t_dict[elem] + 1
        for key, val in s_dict.items():
            if key not in t_dict or val != t_dict[key]:
                return False
        for key, val in t_dict.items():
            if key not in s_dict or val != s_dict[key]:
                return False
        return True
```


