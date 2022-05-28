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
## 141. Linked List Cycle
Given head, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter. Return true if there is a cycle in the linked list. Otherwise, return false.
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        li = []
        if not head:
            return False
        li.append(head)
        nextnode = head.next
        while nextnode and nextnode not in li:
            li.append(nextnode)
            if not nextnode:
                return False
            nextnode = nextnode.next
        if nextnode:
            return True
        else:
            return False
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
## 203. Remove Linked List Elements
Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # In this case, we need to check the next value rather than current value because current value means we have already added the current node, so we will begin with prevhead, in this case it would be 1. Then we can take another variable called nextnode = prevhead.next. Then check if nextnode.val is equal to val, if it is then go to next node by nextnode = nextnode.next and check again. Until we reach a solution where nextnode.val is not equal to val. In that case we would assign prevhead.next = nextnode and then prevhead = prevhead.next to keep iterating through while loop.
        prevhead = ListNode(-1) # This seems to be essential so that logic is applied right from first node. In return statement we can send prevhead as well but ideal way is to send prevhead.next.
        prevhead.next = head # To append existing header to prevhead
        header = prevhead # To preserve prevhead and use it in return statement.
        while header:# While loop is essential for linked list
            nextnode = header.next
            while nextnode and nextnode.val == val: # Check for all consecutive nodes if val is matching
                nextnode = nextnode.next
            header.next = nextnode # Assign the value of next node
            # Above step is fine if nextnode is None because that is how LinkedNode id defined.
            # Next step iteration to keep the outer while loop going and assign next val to head
            header = header.next
        return prevhead.next # Because prevhead is ours and prevhead.next is input as explained in line 10.
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
## 83. Remove Duplicates from Sorted List
Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # for this we will start with -1 and check next node with nextnode = currentnode.next if it is equal to current one or the first next node. if it is equal then keep iterating else assign the current node with last node available currentnode.next = lastnode
        prevhead = ListNode(-1) # basic step for ListNode
        prevhead.next = head # Assign input head to append input linked node to prevhead
        header = prevhead # keep a separate variable to loop through
        while header:
            nextnode = header.next
            while nextnode and nextnode.next and nextnode.next.val == nextnode.val:
                nextnode = nextnode.next
            header.next = nextnode # Assign next val to header.next
            header = header.next # Main logic to keep while loop flowing
        return prevhead.next
```
## 20. Valid Parentheses
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if:
Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
```python
class Solution:
    def isValid(self, s: str) -> bool:
        # This logic will be implemented by stack. Iterate through given list and check if it is opening and closing. If it is opening then append it to stack , if it is closing , pop last element and check if it is corresponding openning element , if it's not return False , if it is continue.
        opening_bracket = '[{('
        closing_bracket = ')}]'
        dict_bracket = {'}':'{', ']':'[',')':'('}
        stacks = []
        if len(s) == 1:
            return False
        for i in s:
            if i in opening_bracket:
                stacks.append(i)
            else:
                if (len(stacks) == 0) or not stacks.pop() == dict_bracket[i]: 
                    return False
        if stacks:
            return False
        else :
            return True
```
## 232. Implement Queue using Stacks
Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

void push(int x) Pushes element x to the back of the queue.
int pop() Removes the element from the front of the queue and returns it.
int peek() Returns the element at the front of the queue.
boolean empty() Returns true if the queue is empty, false otherwise.
```python
class MyQueue:

    def __init__(self):
        self.li = []
        

    def push(self, x: int) -> None:
        self.li.append(x)

    def pop(self) -> int:
        x = self.li[0]
        print(self.li)
        self.li[:] = self.li[1:]
        print(self.li)
        return x

    def peek(self) -> int:
        return self.li[0]
    
    def empty(self) -> bool:
        return not bool(self.li)
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```
## 144. Binary Tree Preorder Traversal
Given the root of a binary tree, return the preorder traversal of its nodes' values.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        li = []
        def preorder(root):
            if root:
                li.append(root.val)
                preorder(root.left)
                preorder(root.right)
        preorder(root)
        return li
```
## 94. Binary Tree Inorder Traversal
Given the root of a binary tree, return the inorder traversal of its nodes' values.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        li = []
        def inorder(root):
            if root: # if no left and right node present then func should not go ahead
                inorder(root.left)
                li.append(root.val)
                inorder(root.right)
        inorder(root)
        return li
```
## 145. Binary Tree Postorder Traversal
Given the root of a binary tree, return the postorder traversal of its nodes' values.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        li = []
        def postorder(root):
            if root:
                postorder(root.left)
                postorder(root.right)
                li.append(root.val)
        postorder(root)
        return li
```
## 102. Binary Tree Level Order Traversal
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # the method is to create two lists, one main lists and another nested list. create a nest list and append 3 to it. pop 3 and then derive its left and append to nested list and right and append to nested list. add this nested list to main list. pop last element of nested list then loop through it and derive each ones left and right child.
        li = [] # main queue
        li.append([root]) # This is essential else it wont go in while loop
        main_list = []
        if root is None:
            return []
        main_list.append([root.val])
        while li:# loop through till all levels are reached
        # for i in range(2):
            # print('List value is')
            # print(li)
            nested_li = [] # create nested list
            nested_val_li = []
            first_elem = li.pop(0)
            for i in first_elem:
                if i.left is not None:
                    nested_li.append(i.left)
                    nested_val_li.append(i.left.val)
                if i.right is not None:
                    nested_li.append(i.right)
                    nested_val_li.append(i.right.val)
            if nested_li:
                li.append(nested_li)
                main_list.append(nested_val_li)
            # print('New list')
            # print(li)
            # print(li)
            # print(main_list)
        # print(li)
        # print(main_list)
        return main_list
```
## 104. Maximum Depth of Binary Tree
Given the root of a binary tree, return its maximum depth. A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right      
def height(root):   
    if root:
        lheight = height(root.left)
        rheight = height(root.right)
        if lheight >= rheight:
            return lheight + 1
        else:
            return rheight + 1
    else:
        return 0
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        return height(root)
```
## 101. Symmetric Tree
Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def solve(root1, root2):
    # basically we will solve for two sub trees. These two can be any two nodes. both nodes should be empty then return true and if one of them is empty then return false and last combination is that the corresponding root should have same value and their left and right subtree root node if put into solve function would return True.
    if not root1 and not root2:
        return True
    elif not root1 or not root2:
        return False
    return root1.val == root2.val and solve(root1.left, root2.right) and solve(root1.right, root2.left)
        
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        return solve(root.left, root.right)
```
## 226. Invert Binary Tree
Given the root of a binary tree, invert the tree, and return its root.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
  #    0 -> 2 to power 0, starting 2 to power 0 - 1, (ending 2 to power 1) - 2
  #   1 2 -> 2 to power 1, starting 2 to power 1 - 1, (ending 2 to power 2) - 2
  #  3 4 5 6 -> 2 to power 2, starting 2 to power 2 - 1, (ending 2 to power 3) - 2
  # 7 8 9 10 11 12 13 14 -> 2 to power 3, starting 2 to power 3 - 1, (ending 2 to power 4 ) - 2
# above list approaach is not correct as in return we need Treenode.
# start with a simple approach of swapping left and right.
# emphasising the need for previous solution.
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```
## 112. Path Sum
Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum. A leaf is a node with no children.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right    
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        if root.left is None and root.right is None:
            if root.val == targetSum:
                return True
            else:
                return False
        if_in_left,if_in_right = False, False
        if root.left:
            if_in_left = self.hasPathSum(root.left, targetSum - root.val)
        if root.right:
            if_in_right = self.hasPathSum(root.right, targetSum - root.val)
        return if_in_left or if_in_right
```
## 700. Search in a Binary Search Tree
You are given the root of a binary search tree (BST) and an integer val. Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return None
        if root.val == val:
            # print('when equal')
            # print(root.val)
            return root
        elif root.val < val:
            # print('In right')
            # print(root.val)
            return self.searchBST(root.right, val)
        elif root.val > val:
            # print('In left')
            # print(root.val)
            return self.searchBST(root.left, val)        
```
## 701. Insert into a Binary Search Tree
You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST. Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if root is None:
            return TreeNode(val)
        sacred_root = root
        parent = None
        while root:
            parent = root 
            if root.val < val:
                root = root.right
            elif root.val > val:
                root = root.left
        if parent.val > val:
            parent.left = TreeNode(val)
        else:
            parent.right = TreeNode(val)
        return sacred_root
```
## 98. Validate Binary Search Tree
Given the root of a binary tree, determine if it is a valid binary search tree (BST).
A valid BST is defined as follows:
The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    di = {}
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def inorder(root):
            if not root:
                return True
            if not inorder(root.left):
                return False
            if root.val <= self.prev:
                return False
            self.prev = root.val
            return inorder(root.right)

        self.prev = -math.inf
        return inorder(root)
```
 ## 653. Two Sum IV - Input is a BST
 Given the root of a Binary Search Tree and a target number k, return true if there exist two elements in the BST such that their sum is equal to the given target.
 ```python
 # Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findTarget(self, root: Optional[TreeNode], k: int) -> bool:
        di = {}
        def traverse(root):
            if root is None:
                return None
            traverse(root.left)
            di[root.val] = root
            traverse(root.right)
        traverse(root)
        # print(di)
        for i, j in di.items():
            if k - i in di and j != di[k - i]:
                return True
        return False
 ```
 ## 235. Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.
According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while root:
            if root.val < p.val and root.val < q.val:
                root = root.right
            elif root.val > p.val and root.val > q.val:
                root = root.left
            else:
                return root
```
