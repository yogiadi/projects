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

## 695. Max Area of Island

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water. The area of an island is the number of cells with a value 1 in the island. Return the maximum area of an island in grid. If there is no island, return 0.

```python
class Solution(object):
    def maxAreaOfIsland(self, grid):
        seen = set()
        def area(r, c):
            if not (0 <= r < len(grid) and 0 <= c < len(grid[0])
                    and (r, c) not in seen and grid[r][c]):
                return 0
            seen.add((r, c))
            return (1 + area(r+1, c) + area(r-1, c) +
                    area(r, c-1) + area(r, c+1))

        return max(area(r, c)
                   for r in range(len(grid))
                   for c in range(len(grid[0])))
```

## 617. Merge Two Binary Trees

You are given two binary trees root1 and root2. Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree. Return the merged tree.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if root1 is None:
            return root2
        if root2 is None:
            return root1
        root1.val += root2.val
        root1.left = self.mergeTrees(root1.left, root2.left)
        root1.right = self.mergeTrees(root1.right, root2.right)
        return root1
```

## 116. Populating Next Right Pointers in Each Node

You are given a perfect binary tree where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.
Initially, all next pointers are set to NULL.

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        def dfs(root):
            if root is None or root.left is None:
                return None
            root.left.next = root.right
            if root.next:
                root.right.next = root.next.left
            dfs(root.left)
            dfs(root.right)
        dfs(root)
        return root
```

## 542. 01 Matrix

Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell. The distance between two adjacent cells is 1.

```python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        import math
        dist = []
        for i in range(len(mat)):
            dist.append([])
            for j in range(len(mat[i])):
                dist[i].append(math.inf)
        for i in range(len(mat)):
            for j in range(len(mat[i])):
                if mat[i][j] == 0:
                    dist[i][j] = 0
                else:
                    if i > 0: # check top
                        dist[i][j] = min(dist[i][j], dist[i-1][j] + 1)
                    if j > 0: # check left
                        dist[i][j] = min(dist[i][j], dist[i][j - 1] + 1)
        # print(dist)
        # Bottom and right are done in separate passes because left and top are precomputed
        for i in range(len(mat) - 1, -1,-1):
            # print(i)
            # print(len(mat[i]))
            for j in range(len(mat[i]) - 1, -1,-1):
                if i < len(mat) - 1: # check bottom
                    dist[i][j] = min(dist[i][j], dist[i + 1][j] + 1)
                if j < len(mat[i]) - 1: # check right
                    dist[i][j] = min(dist[i][j], dist[i][j + 1] + 1)
        return dist
```

## 994. Rotting Oranges

You are given an m x n grid where each cell can have one of three values:
0 representing an empty cell,
1 representing a fresh orange, or
2 representing a rotten orange.
Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten. Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        R = len(grid)
        C = len(grid[0])
        
        Q = collections.deque()
        
        for i in range(R):
            for j in range(C):
                if grid[i][j] == 2:
                    Q.append((i, j))
        
        time = 0
        while(Q):
            time -= 1
            size = len(Q)
            for s in range(size):
                (i, j) = Q.popleft()
                if i>0 and grid[i-1][j]==1:
                    Q.append((i-1, j))
                    grid[i-1][j] = time
                if j>0 and grid[i][j-1]==1:
                    Q.append((i, j-1))
                    grid[i][j-1] = time
                if i+1<R and grid[i+1][j]==1:
                    Q.append((i+1, j))
                    grid[i+1][j] = time
                if j+1<C and grid[i][j+1]==1:
                    Q.append((i, j+1))
                    grid[i][j+1] = time
        for i in range(R):
            for j in range(C):
                if grid[i][j]==1:
                    return -1
        return max(0, -time-1)
```

## 21. Merge Two Sorted Lists

You are given the heads of two sorted linked lists list1 and list2. Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists. Return the head of the merged linked list.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        prevhead = ListNode(-1)
        currnode3 = prevhead
        if not list1 and not list2:
            return None
        elif not list1:
            return list2
        elif not list2:
            return list1
        currnode1 = list1
        currnode2 = list2
        ## We are supposed to return prevhead.next , so even if we store prevhead with -1 it should not matter. currnode3 will be assigned a value of prevhead but in the subsequent steps it will be assigned currnode3.next = currnode1. then at the end treat it same as l1 and l2 where the next value is assigned to same variable currnode3. At the end we would simply return prevhead.next So it wouldn't return the value of prehead, it will start from its next value.
        while currnode1 and currnode2:
            if currnode1.val <= currnode2.val:
                currnode3.next = currnode1
                currnode1 = currnode1.next
            else:
                currnode3.next = currnode2
                currnode2 = currnode2.next
            currnode3 = currnode3.next
        if currnode1 is None:
            currnode3.next = currnode2
        else:
            currnode3.next = currnode1
        return prevhead.next
```

## 206. Reverse Linked List

Given the head of a singly linked list, reverse the list, and return the reversed list.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Begin with iterating through all the nodes , brute way seems to be insert all of this in  a list and then create a new Linked list by iterating through given list.
        li = [] # Define a list to store all data of linked list.
        while head:# While loop to iterate through linked list
            li.append(head.val) # append in list the next head val
            head = head.next # Keep iteration going by assigning next value to head itself.
        li.reverse()
        prevhead = ListNode(-1)
        header = prevhead
        for i in range(len(li)):
            currentnode = ListNode(li[i])
            header.next = currentnode
            header = header.next
        return prevhead.next # As mentioned earlier return with prevhead.next
```

## 77. Combinations

Given two integers n and k, return all possible combinations of k numbers out of the range [1, n]. You may return the answer in any order.

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        def dfs(idx, arr):
            if len(arr) == k:
                ret.append(arr)
                return
            
            for v in range(idx,n+1):
                dfs(v+1, arr + [v]) 
        
        ret = []
        dfs(1,[])
        return ret
```
