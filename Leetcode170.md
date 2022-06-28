## Questions:

#### 157. Read N Characters Given Read4
Given a file and assume that you can only read the file using a given method read4, implement a method to read n characters.

<details>
<summary>code</summary>
  
  ```
  class Solution:
    def read(self, buf: List[str], n: int) -> int:
        copied_chars = 0
        read_chars = 4
        buf4 = [''] * 4
        
        while copied_chars < n and read_chars == 4:
            read_chars = read4(buf4)
            
            for i in range(read_chars):
                if copied_chars == n:
                    return copied_chars
                buf[copied_chars] = buf4[i]
                copied_chars += 1
        
        return copied_chars
                              
 ```
 </details>

#### 158. Read N Characters Given read4 II - Call Multiple Times
  
<details>
<summary>code</summary>
  
```
class Solution:
    def __init__(self):
        self.q = []
    
    def read(self, buf: List[str], n: int) -> int:
        tot_char_read = 0
        nb_char_read = 4
        buf4 = [""]*4
        while tot_char_read < n and nb_char_read > 0:
            if len(self.q) > 0:
                buf[tot_char_read] = self.q.pop(0)
                tot_char_read += 1
            else:
                nb_char_read = read4(buf4)
                self.q += buf4[:nb_char_read]
        return tot_char_read  
  
```
</details>  

#### 159. Longest Substring with At Most Two Distinct Characters. 
Given a string s, return the length of the longest substring that contains at most two distinct characters.

Examples:
Input: s = "eceba"
Output: 3
Explanation: The substring is "ece" which its length is 3.
	
Input: s = "ccaabbb"
Output: 5
Explanation: The substring is "aabbb" which its length is 5.
	
  
<details>
<summary>code</summary>
  
```
from collections import defaultdict
class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: 'str') -> 'int':
        n = len(s)
        if n < 3:
            return n

        # sliding window left and right pointers
        left, right = 0, 0
        # hashmap character -> its rightmost position
        # in the sliding window
        hashmap = defaultdict()

        max_len = 2

        while right < n:
            # when the slidewindow contains less than 3 characters
            hashmap[s[right]] = right
            right += 1

            # slidewindow contains 3 characters
            if len(hashmap) == 3:
                # delete the leftmost character
                del_idx = min(hashmap.values())
                del hashmap[s[del_idx]]
                # move left pointer of the slidewindow
                left = del_idx + 1

            max_len = max(max_len, right - left)

        return max_len  
  
```
</details>    

#### 161. One Edit Distance

Given two strings s and t, return true if they are both one edit distance apart, otherwise return false.

A string s is said to be one distance apart from a string t if you can:

    Insert exactly one character into s to get t.
    Delete exactly one character from s to get t.
    Replace exactly one character of s with a different character to get t.

Examples:
Input: s = "ab", t = "acb"
Output: true
Explanation: We can insert 'c' into s to get t.
	
Input: s = "", t = ""
Output: false
Explanation: We cannot get t from s by only one step.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def isOneEditDistance(self, s: 'str', t: 'str') -> 'bool':
        ns, nt = len(s), len(t)

        # Ensure that s is shorter than t.
        if ns > nt:
            return self.isOneEditDistance(t, s)

        # The strings are NOT one edit away distance  
        # if the length diff is more than 1.
        if nt - ns > 1:
            return False

        for i in range(ns):
            if s[i] != t[i]:
                # if strings have the same length
                if ns == nt:
                    return s[i + 1:] == t[i + 1:]
                # if strings have different lengths
                else:
                    return s[i:] == t[i + 1:]
        
        # If there is no diffs on ns distance
        # the strings are one edit away only if
        # t has one more character. 
        return ns + 1 == nt  
  
```
</details>    

#### 163. Missing Ranges.  
You are given an inclusive range [lower, upper] and a sorted unique integer array nums, where all elements are in the inclusive range.

A number x is considered missing if x is in the range [lower, upper] and x is not in nums.

Return the smallest sorted list of ranges that cover every missing number exactly. That is, no element of nums is in any of the ranges, and each missing number is in one of the ranges.

Each range [a,b] in the list should be output as:

    "a->b" if a != b
    "a" if a == b

Input: nums = [0,1,3,50,75], lower = 0, upper = 99
Output: ["2","4->49","51->74","76->99"]
Explanation: The ranges are:
[2,2] --> "2"
[4,49] --> "4->49"
[51,74] --> "51->74"
[76,99] --> "76->99"
  
<details>
<summary>code</summary>
  
```
class Solution:
    def findMissingRanges(self, nums: List[int], lower: int, upper: int) -> List[str]:
        result = []

        def add(low, high):
            if low > high:
                return
            if low == high:
                result.append(str(low))
            else:
                result.append(str(low) + '->' + str(high))

        if not nums: # edge case
            add(lower, upper)
            return result

        add(lower, nums[0] - 1)

        for x in range(len(nums)):
            add(nums[x - 1] + 1, nums[x] - 1)

        add(nums[-1] + 1, upper)

        return result  
  
```
</details>    

#### 170. Two Sum III - Data structure design.
Design a data structure that accepts a stream of integers and checks if it has a pair of integers that sum up to a particular value.

Implement the TwoSum class:

    TwoSum() Initializes the TwoSum object, with an empty array initially.
    void add(int number) Adds number to the data structure.
    boolean find(int value) Returns true if there exists any pair of numbers whose sum is equal to value, otherwise, it returns false.

Input
["TwoSum", "add", "add", "add", "find", "find"]
[[], [1], [3], [5], [4], [7]]
Output
[null, null, null, null, true, false]

Explanation
TwoSum twoSum = new TwoSum();
twoSum.add(1);   // [] --> [1]
twoSum.add(3);   // [1] --> [1,3]
twoSum.add(5);   // [1,3] --> [1,3,5]
twoSum.find(4);  // 1 + 3 = 4, return true
twoSum.find(7);  // No two integers sum up to 7, return false
  
<details>
<summary>code</summary>
  
```
class TwoSum(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.nums = []
        self.is_sorted = False


    def add(self, number):
        """
        Add the number to an internal data structure..
        :type number: int
        :rtype: None
        """
        # Inserting while maintaining the ascending order.
        # for index, num in enumerate(self.nums):
        #     if number <= num:
        #         self.nums.insert(index, number)
        #         return
        ## larger than any number
        #self.nums.append(number)

        self.nums.append(number)
        self.is_sorted = False
    

    def find(self, value):
        """
        Find if there exists any pair of numbers which sum is equal to the value.
        :type value: int
        :rtype: bool
        """
        if not self.is_sorted:
            self.nums.sort()
            self.is_sorted = True

        low, high = 0, len(self.nums)-1
        while low < high:
            currSum = self.nums[low] + self.nums[high]
            if currSum < value:
                low += 1
            elif currSum > value:
                high -= 1
            else: # currSum == value
                return True
        
        return False
    
```
</details>    

#### 186. Reverse Words in a String II.
Given a character array s, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by a single space.

Your code must solve the problem in-place, i.e. without allocating extra space.
Input: s = ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]


  
<details>
<summary>code</summary>
  
```
class Solution:
    def reverse(self, l: list, left: int, right: int) -> None:
        while left < right:
            l[left], l[right] = l[right], l[left]
            left, right = left + 1, right - 1
            
    def reverse_each_word(self, l: list) -> None:
        n = len(l)
        start = end = 0
        
        while start < n:
            # go to the end of the word
            while end < n and l[end] != ' ':
                end += 1
            # reverse the word
            self.reverse(l, start, end - 1)
            # move to the next word
            start = end + 1
            end += 1
            
    def reverseWords(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        # reverse the whole string
        self.reverse(s, 0, len(s) - 1)
        
        # reverse each word
        self.reverse_each_word(s)  
  
```
</details>    

#### 243. Shortest Word Distance.
Given an array of strings wordsDict and two different strings that already exist in the array word1 and word2, return the shortest distance between these two words in the list.
Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "coding", word2 = "practice"
Output: 3
Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "coding"
Output: 1
  
<details>
<summary>code</summary>
  
```
class Solution:
    def shortestDistance(self, wordsDict: List[str], word1: str, word2: str) -> int:
        slow = fast = 0
        lastw1 = None
        lastw2 = None
        ans = float('inf')
        for i in range(len(wordsDict)):
            if wordsDict[i] == word1:
                lastw1 = i
            if wordsDict[i] == word2:
                lastw2 = i
            if lastw1 is not None and lastw2 is not None:
                ans = min(ans,abs(lastw1-lastw2))
        return ans  
  
```
</details>    

#### 244. Shortest Word Distance II.
Design a data structure that will be initialized with a string array, and then it should answer queries of the shortest distance between two different strings from the array.

Implement the WordDistance class:

    WordDistance(String[] wordsDict) initializes the object with the strings array wordsDict.
    int shortest(String word1, String word2) returns the shortest distance between word1 and word2 in the array wordsDict.

Input
["WordDistance", "shortest", "shortest"]
[[["practice", "makes", "perfect", "coding", "makes"]], ["coding", "practice"], ["makes", "coding"]]
Output
[null, 3, 1]

Explanation
WordDistance wordDistance = new WordDistance(["practice", "makes", "perfect", "coding", "makes"]);
wordDistance.shortest("coding", "practice"); // return 3
wordDistance.shortest("makes", "coding");    // return 1
  
<details>
<summary>code</summary>
  
```
from collections import defaultdict
class WordDistance:

    def __init__(self, words):
        """
        :type words: List[str]
        """
        self.locations = defaultdict(list)

        # Prepare a mapping from a word to all it's locations (indices).
        for i, w in enumerate(words):
            self.locations[w].append(i)

    def shortest(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        loc1, loc2 = self.locations[word1], self.locations[word2]
        l1, l2 = 0, 0
        min_diff = float("inf")

        # Until the shorter of the two lists is processed
        while l1 < len(loc1) and l2 < len(loc2):
            min_diff = min(min_diff, abs(loc1[l1] - loc2[l2]))
            if loc1[l1] < loc2[l2]:
                l1 += 1
            else:
                l2 += 1
        return min_diff  
  
```
</details>    

#### 245. Shortest Word Distance III
Given an array of strings wordsDict and two strings that already exist in the array word1 and word2, return the shortest distance between these two words in the list.

Note that word1 and word2 may be the same. It is guaranteed that they represent two individual words in the list.
Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "coding"
Output: 1
Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "makes"
Output: 3
  
<details>
<summary>code</summary>
  
```
class Solution:
    def shortestWordDistance(self, wordsDict: List[str], word1: str, word2: str) -> int:
        mindiff = len(wordsDict)
        if word1 != word2:
            idx1 = - mindiff
            idx2 = - mindiff
            for ite, w in enumerate(wordsDict):
                changed = False
                if w == word1:
                    idx1 = ite
                    changed = True
                elif w == word2:
                    idx2 = ite
                    changed = True
                if changed and abs(idx1 - idx2) < mindiff:
                    mindiff = abs(idx1 - idx2)
                    if mindiff == 1:
                        return 1
            return mindiff
        else:
            lastidx = - mindiff
            for ite, w in enumerate(wordsDict):
                if w == word1:
                    if ite - lastidx < mindiff:
                        mindiff = ite - lastidx
                    lastidx = ite
            return mindiff  
  
```
</details>  
  
#### 247. Strobogrammatic Number II
Given an integer n, return all the strobogrammatic numbers that are of length n. You may return the answer in any order.

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).
Input: n = 2
Output: ["11","69","88","96"]
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def __init__(self):
        self._pairs = {"0":"0", "1":"1", "8":"8", "6":"9", "9":"6"}
            
    
    def findStrobogrammatic(self, n):
		"""
        :type n: int
        :rtype: List[str]
        """
        return self.findStrobogrammaticRec(n , False)
        
    def findStrobogrammaticRec(self, n, leadingZeroesAllowed):
       
        out = []
        if n <= 0:
            return out
        elif n == 1:
            return ["0", "1", "8"]
        else:
            torso = self.findStrobogrammaticRec(n-2, True)
            for key  in self._pairs.keys():
                head = key
                tail = self._pairs[key]
                if head != "0" or leadingZeroesAllowed:
                    if torso:
                        _torso = torso[:]
                        for i in range(len(_torso)):
                            _torso[i] = head+_torso[i]+tail
                        out.extend(_torso)
                    else:
                        out.append(head+tail)
        return out  
  
```
</details>  
  
#### 249. Group Shifted Strings
  
We can shift a string by shifting each of its letters to its successive letter.

    For example, "abc" can be shifted to be "bcd".

We can keep shifting the string to form a sequence.

    For example, we can keep shifting "abc" to form the sequence: "abc" -> "bcd" -> ... -> "xyz".

Given an array of strings strings, group all strings[i] that belong to the same shifting sequence. You may return the answer in any order.

 

Example 1:

Input: strings = ["abc","bcd","acef","xyz","az","ba","a","z"]
Output: [["acef"],["a","z"],["abc","bcd","xyz"],["az","ba"]]

<details>
<summary>code</summary>
  
```
class Solution:
    def groupStrings(self, strings: List[str]) -> List[List[str]]:
        
        def shift_letter(letter: str, shift: int):
            return chr((ord(letter) - shift) % 26 + ord('a'))
        
        # Create a hash value
        def get_hash(string: str):
            # Calculate the number of shifts to make the first character to be 'a'
            shift = ord(string[0])
            return ''.join(shift_letter(letter, shift) for letter in string)
        
        # Create a hash_value (hashKey) for each string and append the string
        # to the list of hash values i.e. mapHashToList["abc"] = ["abc", "bcd"]
        groups = collections.defaultdict(list)
        for string in strings:
            hash_key = get_hash(string)
            groups[hash_key].append(string)
        
        # Return a list of all of the grouped strings
        return list(groups.values())  
  
```
</details>  
  
#### 250. Count Univalue Subtrees
Given the root of a binary tree, return the number of uni-value subtrees.

A uni-value subtree means all nodes of the subtree have the same value.
Input: root = [5,1,5,5,5,null,5]
Output: 4
<details>
<summary>code</summary>
  
```
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countUnivalSubtrees(self, root):
        if root is None: return 0
        self.count = 0
        self.is_uni(root)
        return self.count

    def is_uni(self, node):

        # base case - if the node has no children this is a univalue subtree
        if node.left is None and node.right is None:

            # found a univalue subtree - increment
            self.count += 1
            return True

        is_uni = True

        # check if all of the node's children are univalue subtrees and if they have the same value
        # also recursively call is_uni for children
        if node.left is not None:
            is_uni = self.is_uni(node.left) and is_uni and node.left.val == node.val

        if node.right is not None:
            is_uni = self.is_uni(node.right) and is_uni and node.right.val == node.val

        # increment self.res and return whether a univalue tree exists here
        self.count += is_uni
        return is_uni  
  
```
</details>  
  
#### 251. Flatten 2D Vector
Design an iterator to flatten a 2D vector. It should support the next and hasNext operations.

Implement the Vector2D class:

    Vector2D(int[][] vec) initializes the object with the 2D vector vec.
    next() returns the next element from the 2D vector and moves the pointer one step forward. You may assume that all the calls to next are valid.
    hasNext() returns true if there are still some elements in the vector, and false otherwise.
Input
["Vector2D", "next", "next", "next", "hasNext", "hasNext", "next", "hasNext"]
[[[[1, 2], [3], [4]]], [], [], [], [], [], [], []]
Output
[null, 1, 2, 3, true, true, 4, false]

Explanation
Vector2D vector2D = new Vector2D([[1, 2], [3], [4]]);
vector2D.next();    // return 1
vector2D.next();    // return 2
vector2D.next();    // return 3
vector2D.hasNext(); // return True
vector2D.hasNext(); // return True
vector2D.next();    // return 4
vector2D.hasNext(); // return False
  
<details>
<summary>code</summary>
  
```
class Vector2D:

    def __init__(self, v: List[List[int]]):
        # We need to iterate over the 2D vector, getting all the integers
        # out of it and putting them into the nums list.
        for (int[] innerVector : v) {
        self.nums = []
        for inner_list in v:
            for num in inner_list:
                self.nums.append(num)
        # We'll keep position 1 behind the next number to return.
        self.position = -1

    def next(self) -> int:
        # Move up to the current element and return it.
        self.position += 1
        return self.nums[self.position]

    def hasNext(self) -> bool:
        # If the next position is a valid index of nums, return True.
        return self.position + 1 < len(self.nums)  
  
```
</details>  
  
#### 252. Meeting Rooms
Given an array of meeting time intervals where intervals[i] = [starti, endi], determine if a person could attend all meetings.
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false
  
<details>
<summary>code</summary>
  
```
class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        def overlap(interval1: List[int], interval2: List[int]) -> bool:
            return (interval1[0] >= interval2[0] and interval1[0] < interval2[1]
                or interval2[0] >= interval1[0] and interval2[0] < interval1[1])

        for i in range(len(intervals)):
            for j in range(i + 1, len(intervals)):
                if overlap(intervals[i], intervals[j]):
                    return False
        return True  
  
```
</details>
  
#### 253. Meeting Rooms II
Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
  
<details>
<summary>code</summary>
  
```
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        
        # If there is no meeting to schedule then no room needs to be allocated.
        if not intervals:
            return 0

        # The heap initialization
        free_rooms = []

        # Sort the meetings in increasing order of their start time.
        intervals.sort(key= lambda x: x[0])

        # Add the first meeting. We have to give a new room to the first meeting.
        heapq.heappush(free_rooms, intervals[0][1])

        # For all the remaining meeting rooms
        for i in intervals[1:]:

            # If the room due to free up the earliest is free, assign that room to this meeting.
            if free_rooms[0] <= i[0]:
                heapq.heappop(free_rooms)

            # If a new room is to be assigned, then also we add to the heap,
            # If an old room is allocated, then also we have to add to the heap with updated end time.
            heapq.heappush(free_rooms, i[1])

        # The size of the heap tells us the minimum rooms required for all the meetings.
        return len(free_rooms)  
  
```
</details>  
  
#### 254. Factor Combinations
Numbers can be regarded as the product of their factors.

    For example, 8 = 2 x 2 x 2 = 2 x 4.

Given an integer n, return all possible combinations of its factors. You may return the answer in any order.

Note that the factors should be in the range [2, n - 1].
Input: n = 12
Output: [[2,6],[3,4],[2,2,3]]
  
<details>
<summary>code</summary>
  
```
import math
class Solution:
    def getFactors(self, n: int) -> List[List[int]]:
        if n <= 1:
            return []
        
        res = []
        self.dfs(res, [], n, 2)
        return res
    
    def dfs(self, res, cur, num, start):        
        if num == 1:
            if len(cur) > 1: # check by looking at length.
                res.append(list(cur))
            return
        
        
        # sqrt(32) = 5.66
        # 5 * 5 = 25, 6 * 6 = 36
        # so we only need to loop to 5.
        # 32 -> 2, 3, 4, 5
        for i in range(start, int(math.sqrt(num)) + 1):
            if num % i == 0:
                cur.append(i)
                self.dfs(res, cur, num // i, i)
                cur.pop()
        
        # the number can be a factor of itself, 
        # just need to make sure it is not equal to n.
        cur.append(num)
        self.dfs(res, cur, 1, -1)
        cur.pop()  
  
```
</details>  
  
#### 255. Verify Preorder Sequence in Binary Search Tree
Given an array of unique integers preorder, return true if it is the correct preorder traversal sequence of a binary search tree.
Input: preorder = [5,2,1,3,6]
Output: true
  
<details>
<summary>code</summary>
  
```
class Solution:
    def verifyPreorder(self, preorder: List[int]) -> bool:
        stck, lo, n = [], -float('inf'), len(preorder)
        for i in range(n):
            if preorder[i] < lo:
                return False
            if i > 0 and preorder[i] > preorder[i-1]:  # update min threshold
                while stck and stck[-1] < preorder[i]:
                    lo = stck.pop()
            stck.append(preorder[i])
        return True  
  
```
</details> 
  
#### 256. Paint House
There is a row of n houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x 3 cost matrix costs.

    For example, costs[0][0] is the cost of painting house 0 with the color red; costs[1][2] is the cost of painting house 1 with color green, and so on...

Return the minimum cost to paint all houses.
Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
  
<details>
<summary>code</summary>
  
```
def minCost(self, costs):
    """
    :type costs: List[List[int]]
    :rtype: int
    """

    def paint_cost(n, color):
        total_cost = costs[n][color]
        if n == len(costs) - 1:
            pass
        elif color == 0: # Red
            total_cost += min(paint_cost(n + 1, 1), paint_cost(n + 1, 2))
        elif color == 1: # Green
            total_cost += min(paint_cost(n + 1, 0), paint_cost(n + 1, 2))
        else: # Blue
            total_cost += min(paint_cost(n + 1, 0), paint_cost(n + 1, 1))
        return total_cost

    if costs == []:
        return 0
    return min(paint_cost(0, 0), paint_cost(0, 1), paint_cost(0, 2))  
  
```
</details>
  
#### 259. 3Sum Smaller
Given an array of n integers nums and an integer target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.
Input: nums = [-2,0,1,3], target = 2
Output: 2
Explanation: Because there are two triplets which sums are less than 2:
[-2,0,1]
[-2,0,3]
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def threeSumSmaller(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        n = len(nums)
        
        res = 0
        for i in range(n-2):
            k = n-1
            for j in range(i+1, n-1):
                while k > j and nums[i] + nums[j] + nums[k] >= target:
                    k -= 1
                if j >= k:
                    break                
                res += k-j
        
        return res  
  
```
</details>  
  
#### 261. Graph Valid Tree
You have a graph of n nodes labeled from 0 to n - 1. You are given an integer n and a list of edges where edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the graph.

Return true if the edges of the given graph make up a valid tree, and false otherwise.
Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true
  
<details>
<summary>code</summary>
  
```
 DFS

Simple extension of cycle finding algorithm for directed graphs. You need to include the parent as well in the DFS call.
Now if a nbr is in visited and the nbr is not the parent, then we have a cycle.
Notice how we build an undirected graph: include both edges.
O(V+E)
from collections import defaultdict
class Solution(object):
    def dfs(self, node, parent, graph, visited):
        visited.add(node)
        for nbr in graph[node]:
            if nbr not in visited:
                if self.dfs(nbr, node, graph, visited):
                    return True
            elif nbr in visited and nbr != parent:
                return True
        return False
    
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        graph = defaultdict(list)
        for edge in edges:
            u,v = edge[0], edge[1]
            graph[u].append(v)
            graph[v].append(u)
        visited = set([])
        if self.dfs(0, -1, graph, visited):
            return False
        if len(visited) != n:
            return False
        return True
Union Find: Quick Find

If an edge end points are already part of same component, then we have cycle. Also, we must make sure number of edges is equal to n-1
Quick Find implementation of Union Find is O(V * E) - For every edge, we might have to update the entire component array which is size n
class Solution(object):
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        components = [i for i in range(n)]
        for edge in edges:
            u, v = edge[0], edge[1]
            pid, qid = components[u], components[v]
            if pid == qid:  # If an edge end points are in the same component, we have a cycle.
                return False
            else:
                for idx, x in enumerate(components):
                    if x == pid:
                        components[idx] = qid
        return len(edges) == n - 1 # if the number of edges are not equal to n-1, then not a tree
Union Find: Quick Union

Quick Union is another implementation of Union Find where we build forests. For edge u,v, we find the root of u and root of v, then point root of u to root of v.
http://algs4.cs.princeton.edu/15uf/
class Solution(object):
    def root(self, i, components):
        while i != components[i]:
            i = components[i]
        return i
    
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        components = [i for i in range(n)]
        for edge in edges:
            u, v = edge[0], edge[1]
            root_u, root_v = self.root(u, components), self.root(v, components)
            if root_u == root_v:  # If an edge end points are in the same component, we have a cycle.
                return False
            else:
                components[root_u] = root_v
        return len(edges) == n - 1 # if the number of edges are not equal to n-1, then not a tree
Weighted Quick Union

Another optimization that prevents the trees from getting longer.
class Solution(object):
    def root(self, i, components):
        while i != components[i]:
            i = components[i]
        return i
    
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        parents = [i for i in range(n)]
        forest_size = [1]*n
        for edge in edges:
            u, v = edge[0], edge[1]
            root_u, root_v = self.root(u, parents), self.root(v, parents)
            if root_u == root_v:  # If an edge end points are in the same component, we have a cycle.
                return False
            else:
                if forest_size[root_u] < forest_size[root_v]:
                    parents[root_u] = root_v
                    forest_size[root_v] += forest_size[root_u]
                else:
                    parents[root_v] = root_u
                    forest_size[root_u] += forest_size[root_v]
        return len(edges) == n - 1 # if the number of edges are not equal to n-1, then not a tree 
  
```
</details>  
  
#### 265. Paint House II
There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x k cost matrix costs.

    For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on...

Return the minimum cost to paint all houses.
Input: costs = [[1,5,3],[2,9,4]]
Output: 5
Explanation:
Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def minCostII(self, costs: List[List[int]]) -> int:
        # Start by defining n and k to make the following code cleaner.
        n = len(costs)
        if n == 0: return 0 # No houses is a valid test case!
        k = len(costs[0])

        # If you're not familiar with lru_cache, look it up in the docs as it's
        # essential to know about.
        @lru_cache(maxsize=None)
        def memo_solve(house_num, color):

            # Base case.
            if house_num == n - 1:
                return costs[house_num][color]

            # Recursive case.
            cost = math.inf
            for next_color in range(k):
                if next_color == color:
                    continue # Can't paint adjacent houses the same color!
                cost = min(cost, memo_solve(house_num + 1, next_color))
            return costs[house_num][color] + cost

        # Consider all options for painting house 0 and find the minimum.
        cost = math.inf
        for color in range(k):
            cost = min(cost, memo_solve(0, color))
        return cost  
  
```
</details>  
  
#### 266. Palindrome Permutation
Given a string s, return true if a permutation of the string could form a palindrome.
Input: s = "code"
Output: false
  
<details>
<summary>code</summary>
  
```
def canPermutePalindrome(self, s):
    return sum(v % 2 for v in collections.Counter(s).values()) < 2  
  
```
</details> 
  
#### 267. Palindrome Permutation II
Given a string s, return all the palindromic permutations (without duplicates) of it.

You may return the answer in any order. If s has no palindromic permutation, return an empty list.
Input: s = "aabb"
Output: ["abba","baab"]
  
<details>
<summary>code</summary>
  
```
def generatePalindromes(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        ans = []
        n = len(s)
        counter = collections.Counter(s)
        
        def helper(tmp):
            if len(tmp) == n:
                ans.append(tmp)
                return 
            for k, v in counter.items():
                if v > 0:
                    counter[k] -= 2
                    helper(k + tmp + k)
                    counter[k] += 2
        
        odd = [key for key, value in counter.items() if value % 2 != 0]
        if len(odd) > 1:
            return []
        if len(odd) == 1:
            counter[odd[0]] -= 1
            helper(odd[0])
        else:
            helper('')
        return ans
				```  
  
```
</details>  
  
#### 269. Alien Dictionary
There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings words from the alien language's dictionary, where the strings in words are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return "". If there are multiple solutions, return any of them.

A string s is lexicographically smaller than a string t if at the first letter where they differ, the letter in s comes before the letter in t in the alien language. If the first min(s.length, t.length) letters are the same, then s is smaller if and only if s.length < t.length.

Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"
  
<details>
<summary>code</summary>
  
```

from collections import defaultdict, Counter, deque

def alienOrder(self, words: List[str]) -> str:
    
    # Step 0: create data structures + the in_degree of each unique letter to 0.
    adj_list = defaultdict(set)
    in_degree = Counter({c : 0 for word in words for c in word})
            
    # Step 1: We need to populate adj_list and in_degree.
    # For each pair of adjacent words...
    for first_word, second_word in zip(words, words[1:]):
        for c, d in zip(first_word, second_word):
            if c != d:
                if d not in adj_list[c]:
                    adj_list[c].add(d)
                    in_degree[d] += 1
                break
        else: # Check that second word isn't a prefix of first word.
            if len(second_word) < len(first_word): return ""
    
    # Step 2: We need to repeatedly pick off nodes with an indegree of 0.
    output = []
    queue = deque([c for c in in_degree if in_degree[c] == 0])
    while queue:
        c = queue.popleft()
        output.append(c)
        for d in adj_list[c]:
            in_degree[d] -= 1
            if in_degree[d] == 0:
                queue.append(d)
                
    # If not all letters are in output, that means there was a cycle and so
    # no valid ordering. Return "" as per the problem description.
    if len(output) < len(in_degree):
        return ""
    # Otherwise, convert the ordering we found into a string and return it.
    return "".join(output)  
  
```
</details>  
  
#### 270. Closest Binary Search Tree Value
Given the root of a binary search tree and a target value, return the value in the BST that is closest to the target.
Input: root = [4,2,5,1,3], target = 3.714286
Output: 4
  
<details>
<summary>code</summary>
  
```
class Solution:
    def closestValue(self, root: TreeNode, target: float) -> int:
        def inorder(r: TreeNode):
            return inorder(r.left) + [r.val] + inorder(r.right) if r else []
        
        return min(inorder(root), key = lambda x: abs(target - x))  
  
```
</details>  
  
#### 271. Encode and Decode Strings
Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
Machine 2 (receiver) has the function:
vector<string> decode(string s) {
  //... your code
  return strs;
}
  
<details>
<summary>code</summary>
  
```
class Codec:
    def encode(self, strs):
        """Encodes a list of strings to a single string.
        :type strs: List[str]
        :rtype: str
        """
        if len(strs) == 0: 
            return unichr(258)
        
        # encode here is a workaround to fix BE CodecDriver error
        return unichr(257).join(x.encode('utf-8') for x in strs)
        

    def decode(self, s):
        """Decodes a single string to a list of strings.
        :type s: str
        :rtype: List[str]
        """
        if s == unichr(258): 
            return []
        return s.split(unichr(257))  
  
```
</details>  
  
#### 272. Closest Binary Search Tree Value II
Given the root of a binary search tree, a target value, and an integer k, return the k values in the BST that are closest to the target. You may return the answer in any order.

You are guaranteed to have only one unique set of k values in the BST that are closest to the target.
Input: root = [4,2,5,1,3], target = 3.714286, k = 2
Output: [4,3]

  
<details>
<summary>code</summary>
  
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def closestKValues(self, root, target, k):

    # Helper, takes a path and makes it the path to the next node
    def nextpath(path, kid1, kid2):
        if path:
            if kid2(path):
                path += kid2(path),
                while kid1(path):
                    path += kid1(path),
            else:
                kid = path.pop()
                while path and kid is kid2(path):
                    kid = path.pop()

    # These customize nextpath as forward or backward iterator
    kidleft = lambda path: path[-1].left
    kidright = lambda path: path[-1].right

    # Build path to closest node
    path = []
    while root:
        path += root,
        root = root.left if target < root.val else root.right
    dist = lambda node: abs(node.val - target)
    path = path[:path.index(min(path, key=dist))+1]

    # Get the path to the next larger node
    path2 = path[:]
    nextpath(path2, kidleft, kidright)

    # Collect the closest k values by moving the two paths outwards
    vals = []
    for _ in range(k):
        if not path2 or path and dist(path[-1]) < dist(path2[-1]):
            vals += path[-1].val,
            nextpath(path, kidright, kidleft)
        else:
            vals += path2[-1].val,
            nextpath(path2, kidleft, kidright)
    return vals  
  
```
</details>  
  
#### 276. Paint Fence
You are painting a fence of n posts with k different colors. You must paint the posts following these rules:

    Every post must be painted exactly one color.
    There cannot be three or more consecutive posts with the same color.

Given the two integers n and k, return the number of ways you can paint the fence.
Input: n = 3, k = 2
Output: 6
Explanation: All the possibilities are shown.
Note that painting all the posts red or all the posts green is invalid because there cannot be three posts in a row with the same color.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def numWays(self, n: int, k: int) -> int:
        def total_ways(i):
            if i == 1:
                return k
            if i == 2:
                return k * k
            
            # Check if we have already calculated totalWays(i)
            if i in memo:
                return memo[i]
            
            # Use the recurrence relation to calculate total_ways(i)
            memo[i] = (k - 1) * (total_ways(i - 1) + total_ways(i - 2))
            return memo[i]

        memo = {}
        return total_ways(n)  
  
```
</details>  
  
#### 277. Find the Celebrity
Suppose you are at a party with n people labeled from 0 to n - 1 and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know the celebrity, but the celebrity does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is ask questions like: "Hi, A. Do you know B?" to get information about whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) that tells you whether A knows B. Implement a function int findCelebrity(n). There will be exactly one celebrity if they are at the party.

Return the celebrity's label if there is a celebrity at the party. If there is no celebrity, return -1.
Input: graph = [[1,1,0],[0,1,0],[1,1,1]]
Output: 1
Explanation: There are three persons labeled with 0, 1 and 2. graph[i][j] = 1 means person i knows person j, otherwise graph[i][j] = 0 means person i does not know person j. The celebrity is the person labeled as 1 because both 0 and 2 know him but 1 does not know anybody.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def findCelebrity(self, n: int) -> int:
        self.n = n
        for i in range(n):
            if self.is_celebrity(i):
                return i
        return -1
    
    def is_celebrity(self, i):
        for j in range(self.n):
            if i == j: continue # Don't ask if they know themselves.
            if knows(i, j) or not knows(j, i):
                return False
        return True  
  
```
</details>
  
#### 280. Wiggle Sort
Given an integer array nums, reorder it such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

You may assume the input array always has a valid answer.
Input: nums = [3,5,2,1,6,4]
Output: [3,5,1,6,2,4]
Explanation: [1,6,2,5,3,4] is also accepted.
<details>
<summary>code</summary>
  
```
def wiggleSort(self, nums):
    for i in range(len(nums)):
        nums[i:i+2] = sorted(nums[i:i+2], reverse=i%2)  
  
```
</details> 
  
#### 281. Zigzag Iterator
Given two vectors of integers v1 and v2, implement an iterator to return their elements alternately.

Implement the ZigzagIterator class:

    ZigzagIterator(List<int> v1, List<int> v2) initializes the object with the two vectors v1 and v2.
    boolean hasNext() returns true if the iterator still has elements, and false otherwise.
    int next() returns the current element of the iterator and moves the iterator to the next element.
Input: v1 = [1,2], v2 = [3,4,5,6]
Output: [1,3,2,4,5,6]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,3,2,4,5,6].
  
<details>
<summary>code</summary>
  
```
class ZigzagIterator:
    def __init__(self, v1: List[int], v2: List[int]):
        self.vectors = [v1, v2]
        self.p_elem = 0   # pointer to the index of element
        self.p_vec = 0    # pointer to the vector
        # variables for hasNext() function
        self.total_num = len(v1) + len(v2)
        self.output_count = 0

    def next(self) -> int:
        iter_num = 0
        ret = None

        # Iterate over the vectors
        while iter_num < len(self.vectors):
            curr_vec = self.vectors[self.p_vec]
            if self.p_elem < len(curr_vec):
                ret = curr_vec[self.p_elem]

            iter_num += 1
            self.p_vec = (self.p_vec + 1) % len(self.vectors)
            # increment the element pointer once iterating all vectors
            if self.p_vec == 0:
                self.p_elem += 1

            if ret is not None:
                self.output_count += 1
                return ret

        # no more element to output
        raise Exception


    def hasNext(self) -> bool:
        return self.output_count < self.total_num

# Your ZigzagIterator object will be instantiated and called as such:
# i, v = ZigzagIterator(v1, v2), []
# while i.hasNext(): v.append(i.next())  
  
```
</details>  
  
#### 285. Inorder Successor in BST
Given the root of a binary search tree and a node p in it, return the in-order successor of that node in the BST. If the given node has no in-order successor in the tree, return null.

The successor of a node p is the node with the smallest key greater than p.val.
Input: root = [2,1,3], p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
  
<details>
<summary>code</summary>
  
```
class Solution:
    
    previous = None
    inorder_successor_node = None
    
    def inorderSuccessor(self, root: 'TreeNode', p: 'TreeNode') -> 'TreeNode':
        
        self.previous, self.inorder_successor_node = None, None
        
        # Case 1: We simply need to find the leftmost node in the subtree rooted at p.right.
        if p.right:
            leftmost = p.right
            while leftmost.left:
                leftmost = leftmost.left
                
            self.inorder_successor_node = leftmost
        
        # Case 2: We need to perform the standard inorder traversal and keep track of the previous node.
        else:
            self.inorderCase2(root, p)
        
        return self.inorder_successor_node
        
        
    def inorderCase2(self, node: 'TreeNode', p: 'TreeNode'):
        
        if not node:
            return
        
        # Recurse on the left side
        self.inorderCase2(node.left, p)
        
        # Check if previous is the inorder predecessor of node
        if self.previous == p and not self.inorder_successor_node:
            self.inorder_successor_node = node
            return
        
        # Keeping previous up-to-date for further recursions
        self.previous = node
        
        # Recurse on the right side
        self.inorderCase2(node.right, p)  
  
```
</details>  
  
#### 286. Walls and Gates
You are given an m x n grid rooms initialized with these three possible values.

    -1 A wall or an obstacle.
    0 A gate.
    INF Infinity means an empty room. We use the value 231 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.
rooms = [[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
Output: [[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]
  
<details>
<summary>code</summary>
  
```
def wallsAndGates(self, rooms):
    q = [(i, j) for i, row in enumerate(rooms) for j, r in enumerate(row) if not r]
    for i, j in q:
        for I, J in (i+1, j), (i-1, j), (i, j+1), (i, j-1):
            if 0 <= I < len(rooms) and 0 <= J < len(rooms[0]) and rooms[I][J] > 2**30:
                rooms[I][J] = rooms[i][j] + 1
                q += (I, J),  
  
```
</details>
  
#### 291. Word Pattern II
Given a pattern and a string s, return true if s matches the pattern.

A string s matches a pattern if there is some bijective mapping of single characters to strings such that if each character in pattern is replaced by the string it maps to, then the resulting string is s. A bijective mapping means that no two characters map to the same string, and no character maps to two different strings.
Input: pattern = "abab", s = "redblueredblue"
Output: true
Explanation: One possible mapping is as follows:
'a' -> "red"
'b' -> "blue"
  
<details>
<summary>code</summary>
  
```
def wordPatternMatch(self, pattern, str):
    return self.dfs(pattern, str, {})

def dfs(self, pattern, str, dict):
    if len(pattern) == 0 and len(str) > 0:
        return False
    if len(pattern) == len(str) == 0:
        return True
    for end in range(1, len(str)-len(pattern)+2): # +2 because it is the "end of an end"
        if pattern[0] not in dict and str[:end] not in dict.values():
            dict[pattern[0]] = str[:end]
            if self.dfs(pattern[1:], str[end:], dict):
                return True
            del dict[pattern[0]]
        elif pattern[0] in dict and dict[pattern[0]] == str[:end]:
            if self.dfs(pattern[1:], str[end:], dict):
                return True
    return False  
  
```
</details>  
  
#### 296. Best Meeting Point
Given an m x n binary grid grid where each 1 marks the home of one friend, return the minimal total travel distance.

The total travel distance is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using Manhattan Distance, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.
Input: grid = [[1,0,0,0,1],[0,0,0,0,0],[0,0,1,0,0]]
Output: 6
Explanation: Given three friends living at (0,0), (0,4), and (2,2).
The point (0,2) is an ideal meeting point, as the total travel distance of 2 + 2 + 2 = 6 is minimal.
So return 6.
  
<details>
<summary>code</summary>
  
```
def minTotalDistance(self, grid):
    row_sum = map(sum, grid)
    col_sum = map(sum, zip(*grid)) # syntax sugar learned from stefan :-)

    def minTotalDistance1D(vec):
        i, j = -1, len(vec)
        d = left = right = 0
        while i != j:
            if left < right:
                d += left
                i += 1
                left += vec[i]
            else:
                d += right
                j -= 1
                right += vec[j]
        return d

    return minTotalDistance1D(row_sum) + minTotalDistance1D(col_sum)  
  
```
</details>  
  
#### 298. Binary Tree Longest Consecutive Sequence
Given the root of a binary tree, return the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path needs to be from parent to child (cannot be the reverse).
Input: root = [1,null,3,2,4,null,null,null,5]
Output: 3
Explanation: Longest consecutive sequence path is 3-4-5, so return 3.
  
<details>
<summary>code</summary>
  
```
class TreeNode(object):
    """ Definition of a binary tree node."""
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None  
  
  # Deserialization 
class Codec:

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        :type data: str
        :rtype: TreeNode
        """
        def rdeserialize(l):
            """ a recursive helper function for deserialization."""
            if l[0] == 'None':
                l.pop(0)
                return None
                
            root = TreeNode(l[0])
            l.pop(0)
            root.left = rdeserialize(l)
            root.right = rdeserialize(l)
            return root

        data_list = data.split(',')
        root = rdeserialize(data_list)
        return root
  
```
</details> 
  
#### 302. Smallest Rectangle Enclosing Black Pixels
You are given an m x n binary matrix image where 0 represents a white pixel and 1 represents a black pixel.

The black pixels are connected (i.e., there is only one black region). Pixels are connected horizontally and vertically.

Given two integers x and y that represents the location of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

You must write an algorithm with less than O(mn) runtime complexity
Input: image = [["0","0","1","0"],["0","1","1","0"],["0","1","0","0"]], x = 0, y = 2
Output: 6
  
<details>
<summary>code</summary>
  
```
def minArea(self, image, x, y):
    top = self.searchRows(image, 0, x, True)
    bottom = self.searchRows(image, x + 1, len(image), False)
    left = self.searchColumns(image, 0, y, top, bottom, True)
    right = self.searchColumns(image, y + 1, len(image[0]), top, bottom, False)
    return (right - left) * (bottom - top)

def searchRows(self, image, i, j, opt):
    while i != j:
        m = (i + j) / 2
        if ('1' in image[m]) == opt:
            j = m
        else:
            i = m + 1
    return i

def searchColumns(self, image, i, j, top, bottom, opt):
    while i != j:
        m = (i + j) / 2
        if any(image[k][m] == '1' for k in xrange(top, bottom)) == opt:
            j = m
        else:
            i = m + 1
    return i
# Runtime: 56 ms  
  
```
</details>  
	
#### 305. Number of Islands II
You are given an empty 2D binary grid grid of size m x n. The grid represents a map where 0's represent water and 1's represent land. Initially, all the cells of grid are water cells (i.e., all the cells are 0's).

We may perform an add land operation which turns the water at position into a land. You are given an array positions where positions[i] = [ri, ci] is the position (ri, ci) at which we should operate the ith operation.

Return an array of integers answer where answer[i] is the number of islands after turning the cell (ri, ci) into a land.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
Input: m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]
Output: [1,1,2,3]
Explanation:
Initially, the 2d grid is filled with water.
- Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land. We have 1 island.
- Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land. We still have 1 island.
- Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land. We have 2 islands.
- Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land. We have 3 islands.
  
<details>
<summary>code</summary>
  
```
 class Solution(object):
    def numIslands2(self, m, n, positions):
        ans = []
        islands = Union()
        for p in map(tuple, positions):
            islands.add(p)
            for dp in (0, 1), (0, -1), (1, 0), (-1, 0):
                q = (p[0] + dp[0], p[1] + dp[1])
                if q in islands.id:
                    islands.unite(p, q)
            ans += [islands.count]
        return ans

class Union(object):
    def __init__(self):
        self.id = {}
        self.sz = {}
        self.count = 0

    def add(self, p):
        self.id[p] = p
        self.sz[p] = 1
        self.count += 1

    def root(self, i):
        while i != self.id[i]:
            self.id[i] = self.id[self.id[i]]
            i = self.id[i]
        return i

    def unite(self, p, q):
        i, j = self.root(p), self.root(q)
        if i == j:
            return
        if self.sz[i] > self.sz[j]:
            i, j = j, i
        self.id[i] = j
        self.sz[j] += self.sz[i]
        self.count -= 1

#Runtime: 300 ms 
  
```
</details>  
	
#### 308. Range Sum Query 2D - Mutable
Given a 2D matrix matrix, handle multiple queries of the following types:

    Update the value of a cell in matrix.
    Calculate the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Implement the NumMatrix class:

    NumMatrix(int[][] matrix) Initializes the object with the integer matrix matrix.
    void update(int row, int col, int val) Updates the value of matrix[row][col] to be val.
    int sumRegion(int row1, int col1, int row2, int col2) Returns the sum of the elements of matrix inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

Input
["NumMatrix", "sumRegion", "update", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [3, 2, 2], [2, 1, 4, 3]]
Output
[null, 8, null, 10]

Explanation
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e. sum of the left red rectangle)
numMatrix.update(3, 2, 2);       // matrix changes from left image to right image
numMatrix.sumRegion(2, 1, 4, 3); // return 10 (i.e. sum of the right red rectangle)
  
<details>
<summary>code</summary>
  
```
class NumMatrix(object):
    def __init__(self, matrix):
        """
        initialize your data structure here.
        :type matrix: List[List[int]]
        """
        for row in matrix:
            for col in xrange(1, len(row)):
                row[col] += row[col-1]
        self.matrix = matrix
        

    def update(self, row, col, val):
        """
        update the element at matrix[row,col] to val.
        :type row: int
        :type col: int
        :type val: int
        :rtype: void
        """
        original = self.matrix[row][col]
        if col != 0:
            original -= self.matrix[row][col-1]
            
        diff = original - val
        
        for y in xrange(col, len(self.matrix[0])):
            self.matrix[row][y] -= diff

    def sumRegion(self, row1, col1, row2, col2):
        """
        sum of elements matrix[(row1,col1)..(row2,col2)], inclusive.
        :type row1: int
        :type col1: int
        :type row2: int
        :type col2: int
        :rtype: int
        """
        sum = 0
        for x in xrange(row1, row2+1):
            sum += self.matrix[x][col2]
            if col1 != 0:
                sum -= self.matrix[x][col1-1]
        return sum  
  
```
</details>  
	
#### 311. Sparse Matrix Multiplication
 
Given two sparse matrices mat1 of size m x k and mat2 of size k x n, return the result of mat1 x mat2. You may assume that multiplication is always possible.
Input: mat1 = [[1,0,0],[-1,0,3]], mat2 = [[7,0,0],[0,0,0],[0,0,1]]
Output: [[7,0,0],[-7,0,3]]

<details>
<summary>code</summary>
  
```
class NumMatrix(object):
    def __init__(self, matrix):
        """
        initialize your data structure here.
        :type matrix: List[List[int]]
        """
        for row in matrix:
            for col in xrange(1, len(row)):
                row[col] += row[col-1]
        self.matrix = matrix
        

    def update(self, row, col, val):
        """
        update the element at matrix[row,col] to val.
        :type row: int
        :type col: int
        :type val: int
        :rtype: void
        """
        original = self.matrix[row][col]
        if col != 0:
            original -= self.matrix[row][col-1]
            
        diff = original - val
        
        for y in xrange(col, len(self.matrix[0])):
            self.matrix[row][y] -= diff

    def sumRegion(self, row1, col1, row2, col2):
        """
        sum of elements matrix[(row1,col1)..(row2,col2)], inclusive.
        :type row1: int
        :type col1: int
        :type row2: int
        :type col2: int
        :rtype: int
        """
        sum = 0
        for x in xrange(row1, row2+1):
            sum += self.matrix[x][col2]
            if col1 != 0:
                sum -= self.matrix[x][col1-1]
        return sum  
  
```
</details>  
	
#### 314. Binary Tree Vertical Order Traversal
Given the root of a binary tree, return the vertical order traversal of its nodes' values. (i.e., from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
  
<details>
<summary>code</summary>
  
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import defaultdict
class Solution:
    def verticalOrder(self, root: TreeNode) -> List[List[int]]:
        columnTable = defaultdict(list)
        queue = deque([(root, 0)])

        while queue:
            node, column = queue.popleft()

            if node is not None:
                columnTable[column].append(node.val)
                
                queue.append((node.left, column - 1))
                queue.append((node.right, column + 1))
                        
        return [columnTable[x] for x in sorted(columnTable.keys())]  
  
```
</details>  
	
#### 317. Shortest Distance from All Buildings
You are given an m x n grid grid of values 0, 1, or 2, where:

    each 0 marks an empty land that you can pass by freely,
    each 1 marks a building that you cannot pass through, and
    each 2 marks an obstacle that you cannot pass through.

You want to build a house on an empty land that reaches all buildings in the shortest total travel distance. You can only move up, down, left, and right.

Return the shortest travel distance for such a house. If it is not possible to build such a house according to the above rules, return -1.

The total travel distance is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using Manhattan Distance, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.
Input: grid = [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]
Output: 7
Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2).
The point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal.
So return 7.
  
<details>
<summary>code</summary>
  
```
def shortestDistance(self, grid):
    if not grid or not grid[0]: return -1
    M, N, buildings = len(grid), len(grid[0]), sum(val for line in grid for val in line if val == 1)
    hit, distSum = [[0] * N for i in range(M)], [[0] * N for i in range(M)]
    
    def BFS(start_x, start_y):
        visited = [[False] * N for k in range(M)]
        visited[start_x][start_y], count1, queue = True, 1, collections.deque([(start_x, start_y, 0)])
        while queue:
            x, y, dist = queue.popleft()
            for i, j in ((x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1)):
                if 0 <= i < M and 0 <= j < N and not visited[i][j]:
                    visited[i][j] = True
                    if not grid[i][j]:
                        queue.append((i, j, dist + 1))
                        hit[i][j] += 1
                        distSum[i][j] += dist + 1
                    elif grid[i][j] == 1:
                        count1 += 1
        return count1 == buildings  
    
    for x in range(M):
        for y in range(N):
            if grid[x][y] == 1:
                if not BFS(x, y): return -1
    return min([distSum[i][j] for i in range(M) for j in range(N) if not grid[i][j] and hit[i][j] == buildings] or [-1])  
  
```
</details>  
	
#### 320. Generalized Abbreviation
A word's generalized abbreviation can be constructed by taking any number of non-overlapping and non-adjacent substrings and replacing them with their respective lengths.

    For example, "abcde" can be abbreviated into:
        "a3e" ("bcd" turned into "3")
        "1bcd1" ("a" and "e" both turned into "1")
        "5" ("abcde" turned into "5")
        "abcde" (no substrings replaced)
    However, these abbreviations are invalid:
        "23" ("ab" turned into "2" and "cde" turned into "3") is invalid as the substrings chosen are adjacent.
        "22de" ("ab" turned into "2" and "bc" turned into "2") is invalid as the substring chosen overlap.

Given a string word, return a list of all the possible generalized abbreviations of word. Return the answer in any order.
Input: word = "word"
Output: ["4","3d","2r1","2rd","1o2","1o1d","1or1","1ord","w3","w2d","w1r1","w1rd","wo2","wo1d","wor1","word"]
  
<details>
<summary>code</summary>
  
```
 class Solution(object):
    def generateAbbreviations(self, word):
        """
        :type word: str
        :rtype: List[str]
        """
        def helper(word, pos, cur, count, result):
            if len(word) == pos:
                # Once we reach the end, append current to the result
                result.append(cur + str(count) if count > 0 else cur)
            else:
                # Skip current position, and increment count
                helper(word, pos + 1, cur, count + 1, result)
                # Include current position, and zero-out count
                helper(word, pos + 1, cur + (str(count) if count > 0 else '') + word[pos], 0, result)

        result = []
        helper(word, 0, '', 0, result)
        return result 
  
```
</details>  
	
#### 323. Number of Connected Components in an Undirected Graph
You have a graph of n nodes. You are given an integer n and an array edges where edges[i] = [ai, bi] indicates that there is an edge between ai and bi in the graph.

Return the number of connected components in the graph.
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
  
<details>
<summary>code</summary>
  
```
DFS:

def countComponents(n, edges):
        def dfs(n, g, visited):
            if visited[n]:
                return
            visited[n] = 1
            for x in g[n]:
                dfs(x, g, visited)
                
        visited = [0] * n
        g = {x:[] for x in xrange(n)}
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)
            
        ret = 0
        for i in xrange(n):
            if not visited[i]:
                dfs(i, g, visited)
                ret += 1
                
        return ret
BFS:

def countComponents(n, edges):
        g = {x:[] for x in xrange(n)}
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)
            
        ret = 0
        for i in xrange(n):
            queue = [i]
            ret += 1 if i in g else 0
            for j in queue:
                if j in g:
                    queue += g[j]
                    del g[j]

        return ret
Union Find:

def countComponents(n, edges):
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
            
        def union(xy):
            x, y = map(find, xy)
            if rank[x] < rank[y]:
                parent[x] = y
            else:
                parent[y] = x
                if rank[x] == rank[y]:
                    rank[x] += 1
        
        parent, rank = range(n), [0] * n
        map(union, edges)
        return len({find(x) for x in parent})  
  
```
</details>  
	
#### 325. Maximum Size Subarray Sum Equals k
Given an integer array nums and an integer k, return the maximum length of a subarray that sums to k. If there is not one, return 0 instead.
Input: nums = [1,-1,5,-2,3], k = 3
Output: 4
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        prefix_sum = longest_subarray = 0
        indices = {}
        
        for i, num in enumerate(nums):
            prefix_sum += num
            
            # Check if all of the numbers seen so far sum to k.
            if prefix_sum == k:
                longest_subarray = i + 1
                
            # If any subarray seen so far sums to k, then
            # update the length of the longest_subarray. 
            if prefix_sum - k in indices:
                longest_subarray = max(longest_subarray, i - indices[prefix_sum - k])
                
            # Only add the current prefix_sum index pair to the 
            # map if the prefix_sum is not already in the map.
            if prefix_sum not in indices:
                indices[prefix_sum] = i
        
        return longest_subarray  
  
```
</details>  
	
#### 333. Largest BST Subtree
Given the root of a binary tree, find the largest subtree, which is also a Binary Search Tree (BST), where the largest means subtree has the largest number of nodes.

A Binary Search Tree (BST) is a tree in which all the nodes follow the below-mentioned properties:

    The left subtree values are less than the value of their parent (root) node's value.
    The right subtree values are greater than the value of their parent (root) node's value.

Note: A subtree must include all of its descendants.
Input: root = [10,5,15,1,8,null,7]
Output: 3
Explanation: The Largest BST Subtree in this case is the highlighted one. The return value is the subtree's size, which is 3.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def is_valid_bst(self, root: Optional[TreeNode]) -> bool:
        """Check if given tree is a valid Binary Search Tree."""
        # An empty tree is a valid Binary Search Tree.
        if not root:
            return True

        # Find the max node in the left subtree of current node.
        left_max = self.find_max(root.left)

        # If the left subtree has a node greater than or equal to the current node,
        # then it is not a valid Binary Search Tree.
        if left_max >= root.val:
            return False

        # Find the min node in the right subtree of current node.
        right_min = self.find_min(root.right)

        # If the right subtree has a value less than or equal to the current node,
        # then it is not a valid Binary Search Tree.
        if right_min <= root.val:
            return False

        # If the left and right subtrees of current tree are also valid BST.
        # then the whole tree is a BST.
        return self.is_valid_bst(root.left) and self.is_valid_bst(root.right)

    def find_max(self, root: Optional[TreeNode]) -> int:
        # Max node in a empty tree should be smaller than parent.
        if not root:
            return float('-inf')
    
        # Check the maximum node from the current node, left and right subtree of the current tree.
        return max(root.val, self.find_max(root.left), self.find_max(root.right))

    def find_min(self, root: Optional[TreeNode]) -> int:
        # Min node in a empty tree should be larger than parent.
        if not root:
            return float('inf')
        
        # Check the minimum node from the current node, left and right subtree of the current tree
        return min(root.val, self.find_min(root.left), self.find_min(root.right))

    def count_nodes(self, root: Optional[TreeNode]) -> int:
        if not root: 
            return 0

        # Add nodes in left and right subtree.
        # Add 1 and return total size.
        return 1 + self.count_nodes(root.left) + self.count_nodes(root.right) 

    def largestBSTSubtree(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        # If current subtree is a validBST, its children will have smaller size BST.
        if self.is_valid_bst(root):
            return self.count_nodes(root)
        
        # Find BST in left and right subtrees of current nodes.
        return max(self.largestBSTSubtree(root.left), self.largestBSTSubtree(root.right))  
  
```
</details> 
	
#### 339. Nested List Weight Sum
You are given a nested list of integers nestedList. Each element is either an integer or a list whose elements may also be integers or other lists.

The depth of an integer is the number of lists that it is inside of. For example, the nested list [1,[2,2],[[3],2],1] has each integer's value set to its depth.

Return the sum of each integer in nestedList multiplied by its depth.
Input: nestedList = [[1,1],2,[1,1]]
Output: 10
Explanation: Four 1's at depth 2, one 2 at depth 1. 1*2 + 1*2 + 2*1 + 1*2 + 1*2 = 10.
Input: nestedList = [1,[4,[6]]]
Output: 27
Explanation: One 1 at depth 1, one 4 at depth 2, and one 6 at depth 3. 1*1 + 4*2 + 6*3 = 27.
  
<details>
<summary>code</summary>
  
```
class Solution:
    def depthSum(self, nestedList: List[NestedInteger]) -> int:

        def dfs(nested_list, depth):
            total = 0
            for nested in nested_list:
                if nested.isInteger():
                    total += nested.getInteger() * depth
                else:
                    total += dfs(nested.getList(), depth + 1)
            return total

        return dfs(nestedList, 1)
  
  
```
</details>  
	
#### 340. Longest Substring with At Most K Distinct Characters
Given a string s and an integer k, return the length of the longest substring of s that contains at most k distinct characters.
Input: s = "eceba", k = 2
Output: 3
Explanation: The substring is "ece" with length 3.
  
<details>
<summary>code</summary>
  
```
from collections import OrderedDict
class Solution:
    def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        n = len(s)
        if k == 0 or n == 0:
            return 0

        # sliding window left and right pointers
        left, right = 0, 0
        # hashmap character -> its rightmost position
        # in the sliding window
        hashmap = OrderedDict()

        max_len = 1

        while right < n:
            character = s[right]
            # if character is already in the hashmap -
            # delete it, so that after insert it becomes
            # the rightmost element in the hashmap
            if character in hashmap:
                del hashmap[character]
            hashmap[character] = right
            right += 1

            # slidewindow contains k + 1 characters
            if len(hashmap) == k + 1:
                # delete the leftmost character
                _, del_idx = hashmap.popitem(last = False)
                # move left pointer of the slidewindow
                left = del_idx + 1

            max_len = max(max_len, right - left)

        return max_len  
  
```
</details>  
	
#### 346. Moving Average from Data Stream
Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

Implement the MovingAverage class:

    MovingAverage(int size) Initializes the object with the size of the window size.
    double next(int val) Returns the moving average of the last size values of the stream.
Input
["MovingAverage", "next", "next", "next", "next"]
[[3], [1], [10], [3], [5]]
Output
[null, 1.0, 5.5, 4.66667, 6.0]

Explanation
MovingAverage movingAverage = new MovingAverage(3);
movingAverage.next(1); // return 1.0 = 1 / 1
movingAverage.next(10); // return 5.5 = (1 + 10) / 2
movingAverage.next(3); // return 4.66667 = (1 + 10 + 3) / 3
movingAverage.next(5); // return 6.0 = (10 + 3 + 5) / 3

  
<details>
<summary>code</summary>
  
```python

class MovingAverage:
    def __init__(self, size: int):
        self.size = size
        self.queue = []

    def next(self, val: int) -> float:
        size, queue = self.size, self.queue
        queue.append(val)
        # calculate the sum of the moving window
        window_sum = sum(queue[-size:])

        return window_sum / min(len(queue), size)
  
```

</details>  
	
#### 348. Design Tic-Tac-Toe
Assume the following rules are for the tic-tac-toe game on an n x n board between two players:

    A move is guaranteed to be valid and is placed on an empty block.
    Once a winning condition is reached, no more moves are allowed.
    A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

Implement the TicTacToe class:

    TicTacToe(int n) Initializes the object the size of the board n.
    int move(int row, int col, int player) Indicates that the player with id player plays at the cell (row, col) of the board. The move is guaranteed to be a valid move.
Input
["TicTacToe", "move", "move", "move", "move", "move", "move", "move"]
[[3], [0, 0, 1], [0, 2, 2], [2, 2, 1], [1, 1, 2], [2, 0, 1], [1, 0, 2], [2, 1, 1]]
Output
[null, 0, 0, 0, 0, 0, 0, 1]

Explanation
TicTacToe ticTacToe = new TicTacToe(3);
Assume that player 1 is "X" and player 2 is "O" in the board.
ticTacToe.move(0, 0, 1); // return 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

ticTacToe.move(0, 2, 2); // return 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

ticTacToe.move(2, 2, 1); // return 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

ticTacToe.move(1, 1, 2); // return 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

ticTacToe.move(2, 0, 1); // return 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

ticTacToe.move(1, 0, 2); // return 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

ticTacToe.move(2, 1, 1); // return 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
  
<details>
<summary>code</summary>
  
```
class TicTacToe:
    def __init__(self, n: int):
        self.n=n
        self.horiz=[0]*n
        self.vert=[0]*n
        self.diag1=0
        self.diag2=0

    def move(self, row: int, col: int, player: int) -> int:
        n=self.n
        move=1
        if player == 2:
            move=-1
        
        self.horiz[col] += move
        self.vert[row] += move
        
        if row==col: 
            self.diag1 += move
        if row+col == (n-1):
            self.diag2 += move
            
        if abs(self.horiz[col])==n or abs(self.vert[row])==n or abs(self.diag1)==n or abs(self.diag2)==n:
            return player
        return 0

        


# Your TicTacToe object will be instantiated and called as such:
# obj = TicTacToe(n)
# param_1 = obj.move(row,col,player)
  
```
</details>  
	
#### 353. Design Snake Game
Design a Snake game that is played on a device with screen size height x width. Play the game online if you are not familiar with the game.

The snake is initially positioned at the top left corner (0, 0) with a length of 1 unit.

You are given an array food where food[i] = (ri, ci) is the row and column position of a piece of food that the snake can eat. When a snake eats a piece of food, its length and the game's score both increase by 1.

Each piece of food appears one by one on the screen, meaning the second piece of food will not appear until the snake eats the first piece of food.

When a piece of food appears on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

The game is over if the snake goes out of bounds (hits a wall) or if its head occupies a space that its body occupies after moving (i.e. a snake of length 4 cannot run into itself).

Implement the SnakeGame class:

    SnakeGame(int width, int height, int[][] food) Initializes the object with a screen of size height x width and the positions of the food.
    int move(String direction) Returns the score of the game after applying one direction move by the snake. If the game is over, return -1.

 Input
["SnakeGame", "move", "move", "move", "move", "move", "move"]
[[3, 2, [[1, 2], [0, 1]]], ["R"], ["D"], ["R"], ["U"], ["L"], ["U"]]
Output
[null, 0, 0, 1, 1, 2, -1]

Explanation
SnakeGame snakeGame = new SnakeGame(3, 2, [[1, 2], [0, 1]]);
snakeGame.move("R"); // return 0
snakeGame.move("D"); // return 0
snakeGame.move("R"); // return 1, snake eats the first piece of food. The second piece of food appears at (0, 1).
snakeGame.move("U"); // return 1
snakeGame.move("L"); // return 2, snake eats the second food. No more food appears.
snakeGame.move("U"); // return -1, game over because snake collides with border

<details>
<summary>code</summary>
  
```
class SnakeGame:

    def __init__(self, width: int, height: int, food: List[List[int]]):
        """
        Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0].
        """
        self.snake = collections.deque([(0,0)])    # snake head is at the front
        self.snake_set = {(0,0) : 1}
        self.width = width
        self.height = height
        self.food = food
        self.food_index = 0
        self.movement = {'U': [-1, 0], 'L': [0, -1], 'R': [0, 1], 'D': [1, 0]}
        

    def move(self, direction: str) -> int:
        """
        Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body.
        """
        
        newHead = (self.snake[0][0] + self.movement[direction][0],
                   self.snake[0][1] + self.movement[direction][1])
        
        # Boundary conditions.
        crosses_boundary1 = newHead[0] < 0 or newHead[0] >= self.height
        crosses_boundary2 = newHead[1] < 0 or newHead[1] >= self.width
        
        # Checking if the snake bites itself.
        bites_itself = newHead in self.snake_set and newHead != self.snake[-1]
     
        # If any of the terminal conditions are satisfied, then we exit with rcode -1.
        if crosses_boundary1 or crosses_boundary2 or bites_itself:
            return -1

        # Note the food list could be empty at this point.
        next_food_item = self.food[self.food_index] if self.food_index < len(self.food) else None
        
        # If there's an available food item and it is on the cell occupied by the snake after the move, eat it
        if self.food_index < len(self.food) and \
            next_food_item[0] == newHead[0] and \
                next_food_item[1] == newHead[1]:  # eat food
            self.food_index += 1
        else:    # not eating food: delete tail                 
            tail = self.snake.pop()  
            del self.snake_set[tail]
            
        # A new head always gets added
        self.snake.appendleft(newHead)
        
        # Also add the head to the set
        self.snake_set[newHead] = 1

        return len(self.snake) - 1
        


# Your SnakeGame object will be instantiated and called as such:
# obj = SnakeGame(width, height, food)
# param_1 = obj.move(direction)  
  
```
</details>  
	
#### 358. Rearrange String k Distance Apart
Given a string s and an integer k, rearrange s such that the same characters are at least distance k from each other. If it is not possible to rearrange the string, return an empty string "".
Input: s = "aabbcc", k = 3
Output: "abcabc"
Explanation: The same letters are at least a distance of 3 from each other.
  
<details>
<summary>code</summary>
  
```
def rearrangeString(self, s, k):
        n = len(s)
        if not k: return s
        count = collections.Counter(s)
        maxf = max(count.values())
        if (maxf - 1) * k + count.values().count(maxf) > len(s):
            return ""
        res = list(s)
        i = (n - 1) % k
        for c in sorted(count, key=lambda i: -count[i]):
            for j in range(count[c]):
                res[i] = c
                i += k
                if i >= n:
                    i = (i - 1) % k
        return "".join(res)  
  
```
</details>
		
#### 359. Logger Rate Limiter
  
<details>
<summary>code</summary>
  
```
from collections import deque

class Logger(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self._msg_set = set()
        self._msg_queue = deque()
    
    def shouldPrintMessage(self, timestamp, message):
        """
        Returns true if the message should be printed in the given timestamp, otherwise returns false.
        """
        while self._msg_queue:
            msg, ts = self._msg_queue[0]
            if timestamp - ts >= 10:
                self._msg_queue.popleft()
                self._msg_set.remove(msg)
            else:
                break
        
        if message not in self._msg_set:
            self._msg_set.add(message)
            self._msg_queue.append((message, timestamp))
            return True
        else:
            return False  
		
  Approach 2: Hashtable / Dictionary
		
	class Logger(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self._msg_dict = {}
    
    def shouldPrintMessage(self, timestamp, message):
        """
        Returns true if the message should be printed in the given timestamp, otherwise returns false.
        """
        if message not in self._msg_dict:
            # case 1). add the message to print
            self._msg_dict[message] = timestamp
            return True

        if timestamp - self._msg_dict[message] >= 10:
            # case 2). update the timestamp of the message
            self._msg_dict[message] = timestamp
            return True
        else:
            return False
	
  
```
</details>  
		
#### 360. Sort Transformed Array
  
<details>
<summary>code</summary>
  
```
def sortTransformedArray(self, nums, a, b, c):
    nums = [x*x*a + x*b + c for x in nums]
    ret = [0] * len(nums)
    p1, p2 = 0, len(nums) - 1
    i, d = (p1, 1) if a < 0 else (p2, -1)
    while p1 <= p2:
        if nums[p1] * -d > nums[p2] * -d:
            ret[i] = nums[p1]
            p1 += 1
        else:
            ret[i] = nums[p2]
            p2 -=1
        i += d
    return ret  
  
```
</details>  
		
#### 361. Bomb Enemy
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maxKilledEnemies(self, grid: List[List[str]]) -> int:
        if len(grid) == 0:
            return 0

        rows, cols = len(grid), len(grid[0])

        max_count = 0
        row_hits = 0
        col_hits = [0] * cols

        for row in range(0, rows):
            for col in range(0, cols):
                # reset the hits on the row, if necessary.
                if col == 0 or grid[row][col - 1] == 'W':
                    row_hits = 0
                    for k in range(col, cols):
                        if grid[row][k] == 'W':
                            # stop the scan when we hit the wall.
                            break
                        elif grid[row][k] == 'E':
                            row_hits += 1

                # reset the hits on the col, if necessary.
                if row == 0 or grid[row - 1][col] == 'W':
                    col_hits[col] = 0
                    for k in range(row, rows):
                        if grid[k][col] == 'W':
                            break
                        elif grid[k][col] == 'E':
                            col_hits[col] += 1

                # count the hits for each empty cell.
                if grid[row][col] == '0':
                    total_hits = row_hits + col_hits[col]
                    max_count = max(max_count, total_hits)

        return max_count  
  
```
</details>  
		
#### 362. Design Hit Counter
  
<details>
<summary>code</summary>
  
```
 class HitCounter(object):

def __init__(self):
    """
    Initialize your data structure here.
    """
    from collections import deque
    
    self.num_of_hits = 0
    self.time_hits = deque()
    

def hit(self, timestamp):
    """
    Record a hit.
    @param timestamp - The current timestamp (in seconds granularity).
    :type timestamp: int
    :rtype: void
    """
    if not self.time_hits or self.time_hits[-1][0] != timestamp:
        self.time_hits.append([timestamp, 1])
    else:
        self.time_hits[-1][1] += 1
    
    self.num_of_hits += 1
            
    

def getHits(self, timestamp):
    """
    Return the number of hits in the past 5 minutes.
    @param timestamp - The current timestamp (in seconds granularity).
    :type timestamp: int
    :rtype: int
    """
    while self.time_hits and self.time_hits[0][0] <= timestamp - 300:
        self.num_of_hits -= self.time_hits.popleft()[1]
    
    return self.num_of_hits 
  
```
</details> 
		
#### 364. Nested List Weight Sum II
  
<details>
<summary>code</summary>
  
```
 def depthSumInverse(self, nestedList: List[NestedInteger]) -> int:
        unweighted = 0
        weighted = 0
        while nestedList:
            nextLevel = []
            for item in nestedList:
                if item.isInteger():
                    unweighted += item.getInteger()
                else:
                    nextLevel.extend(item.getList())
            weighted += unweighted
            nestedList = nextLevel
        return weighted 
  
```
</details>  
		
#### 366. Find Leaves of Binary Tree
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def findLeaves(self, root):
        def order(root, dic):
            if not root:
                return 0
            left = order(root.left, dic)
            right = order(root.right, dic)
            lev = max(left, right) + 1
            dic[lev] += root.val,
            return lev
        dic, ret = collections.defaultdict(list), []
        order(root, dic)
        for i in range(1, len(dic) + 1):
            ret.append(dic[i])
        return ret  
  
```
</details> 
		
#### 369. Plus One Linked List
  
<details>
<summary>code</summary>
  
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def plusOne(self, head: ListNode) -> ListNode:
        # sentinel head
        sentinel = ListNode(0)
        sentinel.next = head
        not_nine = sentinel

        # find the rightmost not-nine digit
        while head:
            if head.val != 9:
                not_nine = head
            head = head.next

        # increase this rightmost not-nine digit by 1
        not_nine.val += 1
        not_nine = not_nine.next

        # set all the following nines to zeros
        while not_nine:
            not_nine.val = 0
            not_nine = not_nine.next

        return sentinel if sentinel.val else sentinel.next  
  
```
</details>  
		
#### 370. Range Addition
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def getModifiedArray(self, length, updates):
        """
        :type length: int
        :type updates: List[List[int]]
        :rtype: List[int]
        """
        res = [0] * length
        for update in updates:
            start, end, inc = update
            res[start] += inc
            
            if end + 1 <= length - 1:
                res[end+1] -= inc

        sum = 0
        for i in range(length):
            sum += res[i]
            res[i] = sum
        return res  
  
```
</details>  
		
#### 422. Valid Word Square
  
<details>
<summary>code</summary>
  
```
def validWordSquare(self, words):
    return map(None, *words) == map(None, *map(None, *words))
Or saving some work but taking two lines:

def validWordSquare(self, words):
    t = map(None, *words)
    return t == map(None, *t)  
  
```
</details>  
		
#### 425. Word Squares
  
<details>
<summary>code</summary>
  
```
class Solution {
  int N = 0;
  String[] words = null;
  HashMap<String, List<String>> prefixHashTable = null;

  public List<List<String>> wordSquares(String[] words) {
    this.words = words;
    this.N = words[0].length();

    List<List<String>> results = new ArrayList<List<String>>();
    this.buildPrefixHashTable(words);

    for (String word : words) {
      LinkedList<String> wordSquares = new LinkedList<String>();
      wordSquares.addLast(word);
      this.backtracking(1, wordSquares, results);
    }
    return results;
  }

  protected void backtracking(int step, LinkedList<String> wordSquares,
                              List<List<String>> results) {
    if (step == N) {
      results.add((List<String>) wordSquares.clone());
      return;
    }

    StringBuilder prefix = new StringBuilder();
    for (String word : wordSquares) {
      prefix.append(word.charAt(step));
    }

    for (String candidate : this.getWordsWithPrefix(prefix.toString())) {
      wordSquares.addLast(candidate);
      this.backtracking(step + 1, wordSquares, results);
      wordSquares.removeLast();
    }
  }

  protected void buildPrefixHashTable(String[] words) {
    this.prefixHashTable = new HashMap<String, List<String>>();

    for (String word : words) {
      for (int i = 1; i < this.N; ++i) {
        String prefix = word.substring(0, i);
        List<String> wordList = this.prefixHashTable.get(prefix);
        if (wordList == null) {
          wordList = new ArrayList<String>();
          wordList.add(word);
          this.prefixHashTable.put(prefix, wordList);
        } else {
          wordList.add(word);
        }
      }
    }
  }

  protected List<String> getWordsWithPrefix(String prefix) {
    List<String> wordList = this.prefixHashTable.get(prefix);
    return (wordList != null ? wordList : new ArrayList<String>());
  }
}  
  
```
</details>  
		
#### 426. Convert Binary Search Tree to Sorted Doubly Linked List

  
<details>
<summary>code</summary>
  
```
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        def helper(node):
            """
            Performs standard inorder traversal:
            left -> node -> right
            and links all nodes into DLL
            """
            nonlocal last, first
            if node:
                # left
                helper(node.left)
                # node 
                if last:
                    # link the previous node (last)
                    # with the current one (node)
                    last.right = node
                    node.left = last
                else:
                    # keep the smallest node
                    # to close DLL later on
                    first = node        
                last = node
                # right
                helper(node.right)
        
        if not root:
            return None
        
        # the smallest (first) and the largest (last) nodes
        first, last = None, None
        helper(root)
        # close DLL
        last.right = first
        first.left = last
        return first  
  
```
</details>  
		
#### 428. Serialize and Deserialize N-ary Tree
  
<details>
<summary>code</summary>
  
```
class WrappableInt:
        def __init__(self, x):
            self.value = x
        def getValue(self):
            return self.value
        def increment(self):
            self.value += 1
class Codec:
    def serialize(self, root):	
        serial = []

        def preorder(node):

            if not node:
                return

            serial.append(str(node.val))

            for child in node.children:
                preorder(child)

            serial.append("#")      # indicates no more children, continue serialization from parent

        preorder(root)
        return " ".join(serial)

    def deserialize(self, data):	
        if not data:
            return None

        tokens = deque(data.split())
        root = Node(int(tokens.popleft()), [])

        def helper(node):

            if not tokens:
                return

            while tokens[0] != "#": # add child nodes with subtrees
                value = tokens.popleft()
                child = Node(int(value), [])
                node.children.append(child)
                helper(child)

            tokens.popleft()        # discard the "#"

        helper(root)
        return root  
  
```
</details>  
		
#### 431. Encode N-ary Tree to Binary Tree
  
<details>
<summary>code</summary>
  
```
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""
"""
# Definition for a binary tree node.
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
"""
class Codec:
    def encode(self, root):
        """Encodes an n-ary tree to a binary tree.
        :type root: Node
        :rtype: TreeNode
        """
        if not root:
            return None

        rootNode = TreeNode(root.val)
        queue = deque([(rootNode, root)])

        while queue:
            parent, curr = queue.popleft()
            prevBNode = None
            headBNode = None
            # traverse each child one by one
            for child in curr.children:
                newBNode = TreeNode(child.val)
                if prevBNode:
                    prevBNode.right = newBNode
                else:
                    headBNode = newBNode
                prevBNode = newBNode
                queue.append((newBNode, child))

            # use the first child in the left node of parent
            parent.left = headBNode

        return rootNode


    def decode(self, data):
        """Decodes your binary tree to an n-ary tree.
        :type data: TreeNode
        :rtype: Node
        """
        if not data:
            return None

        # should set the default value to [] rather than None,
        # otherwise it wont pass the test cases.
        rootNode = Node(data.val, [])

        queue = deque([(rootNode, data)])

        while queue:
            parent, curr = queue.popleft()

            firstChild = curr.left
            sibling = firstChild

            while sibling:
                # Note: the initial value of the children list should not be None, which is assumed by the online judge.
                newNode = Node(sibling.val, [])
                parent.children.append(newNode)
                queue.append((newNode, sibling))
                sibling = sibling.right

        return rootNode  
  
```
</details>  
		
#### 439. Ternary Expression Parser
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def parseTernary(self, expression):
        """
        :type expression: str
        :rtype: str
        """
        stack = []
        d = {}
        for i in xrange(0, len(expression)):
            if expression[i] == "?":
                stack.append(("?", i))
            elif expression[i] == ":":
                _, pos = stack.pop()
                d[pos] = i

        def dfs(expr, start, end, d):
            if end - start + 1 < 5:
                return expr[start:end+1]
            iSep = d[start + 1]
            stmt = expr[start]
            return dfs(expr, start + 2, iSep - 1, d) if stmt == "T" else dfs(expr, iSep + 1, end, d)
        return dfs(expression, 0, len(expression), d)  
  
```
</details>  
		
#### 465. Optimal Account Balancing
  
<details>
<summary>code</summary>
  
```
 def minTransfers(self, transactions):
        """
        :type transactions: List[List[int]]
        :rtype: int
        """
        m = collections.defaultdict(int)
        
        for t in transactions:
            m[t[0]]-=t[2]
            m[t[1]]+=t[2]
        
        debt = m.values()
        
        def dfs(s):
            while(s<len(debt) and debt[s]==0):
                s+=1
            if s==len(debt): return 0
            
            r = float('inf')
            for i in range(s+1,len(debt)):
                if debt[i]*debt[s]<0:
                    # settle s with i
                    debt[i]+=debt[s]
                    r=min(r,1+dfs(s+1))
                    # backtrack
                    debt[i]-=debt[s]
            return r
        
        return dfs(0)  
  
```
</details>  
		
#### 471. Encode String with Shortest Length
  
<details>
<summary>code</summary>
  
```
def encode(self, s, memo={}):
    if s not in memo:
        n = len(s)
        i = (s + s).find(s, 1)
        one = '%d[%s]' % (n / i, self.encode(s[:i])) if i < n else s
        multi = [self.encode(s[:i]) + self.encode(s[i:]) for i in xrange(1, n)]
        memo[s] = min([s, one] + multi, key=len)
    return memo[s]  
  
```
</details>  
		
#### 484. Find Permutation
  
<details>
<summary>code</summary>
  
```
def findPermutation(self, s):
    ret = []
    for i in range(len(s)):
      if s[i] == 'I':
        ret.extend(range(i + 1, len(ret), -1))
    ret.extend(range(len(s) + 1, len(ret), -1))
    return ret 
  
```
</details>  
		
#### 487. Max Consecutive Ones II
  
<details>
<summary>code</summary>
  
```
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        longest_sequence = 0
        for left in range(len(nums)):
            num_zeroes = 0
            for right in range(left, len(nums)):   # check every consecutive sequence
                if num_zeroes == 2:
                    break
                if nums[right] == 0:               # count how many 0's
                    num_zeroes += 1
                if num_zeroes <= 1:                 # update answer if it's valid
                    longest_sequence = max(longest_sequence, right - left + 1)
        return longest_sequence  
  
```
</details>  
		
#### 489. Robot Room Cleaner
  
<details>
<summary>code</summary>
  
```
#class Robot:
#    def move(self):
#        """
#        Returns true if the cell in front is open and robot moves into the cell.
#        Returns false if the cell in front is blocked and robot stays in the current cell.
#        :rtype bool
#        """
#
#    def turnLeft(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def turnRight(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def clean(self):
#        """
#        Clean the current cell.
#        :rtype void
#        """
class Solution(object):       
    def cleanRoom(self, robot):
        """
        :type robot: Robot
        :rtype: None
        """
        def go_back():
            robot.turnRight()
            robot.turnRight()
            robot.move()
            robot.turnRight()
            robot.turnRight()
            
        def backtrack(cell = (0, 0), d = 0):
            visited.add(cell)
            robot.clean()
            # going clockwise : 0: 'up', 1: 'right', 2: 'down', 3: 'left'
            for i in range(4):
                new_d = (d + i) % 4
                new_cell = (cell[0] + directions[new_d][0], \
                            cell[1] + directions[new_d][1])
                
                if not new_cell in visited and robot.move():
                    backtrack(new_cell, new_d)
                    go_back()
                # turn the robot following chosen direction : clockwise
                robot.turnRight()
    
        # going clockwise : 0: 'up', 1: 'right', 2: 'down', 3: 'left'
        directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
        visited = set()
        backtrack()  
  
```
</details>  
	
#### 490. The Maze
  
<details>
<summary>code</summary>
  
```
 def hasPath(self, maze, start, destination):

        Q = [start]
        n = len(maze)
        m = len(maze[0])
        dirs = ((0, 1), (0, -1), (1, 0), (-1, 0))
        
        while Q:
            # Use Q.pop() as DFS or Q.popleft() with deque from collections library for better performance. Kudos to @whglamrock
            i, j = Q.pop(0)
            maze[i][j] = 2

            if i == destination[0] and j == destination[1]:
                return True
            
            for x, y in dirs:
                row = i + x
                col = j + y
                while 0 <= row < n and 0 <= col < m and maze[row][col] != 1:
                    row += x
                    col += y
                row -= x
                col -= y
                if maze[row][col] == 0:
                    Q.append([row, col])
        
        return False 
  
```
</details>  
	
#### 499. The Maze III
  
<details>
<summary>code</summary>
  
```
def findShortestWay(self, A, ball, hole):
    ball, hole = tuple(ball), tuple(hole)
    R, C = len(A), len(A[0])
    
    def neighbors(r, c):
        for dr, dc, di in [(-1, 0, 'u'), (0, 1, 'r'), 
                           (0, -1, 'l'), (1, 0, 'd')]:
            cr, cc, dist = r, c, 0
            while (0 <= cr + dr < R and 
                    0 <= cc + dc < C and
                    not A[cr+dr][cc+dc]):
                cr += dr
                cc += dc
                dist += 1
                if (cr, cc) == hole:
                    break
            yield (cr, cc), di, dist
    
    pq = [(0, '', ball)]
    seen = set()
    while pq:
        dist, path, node = heapq.heappop(pq)
        if node in seen: continue
        if node == hole: return path
        seen.add(node)
        for nei, di, nei_dist in neighbors(*node):
            heapq.heappush(pq, (dist+nei_dist, path+di, nei) )
        
    return "impossible"  
  
```
</details>  
	
#### 505. The Maze II
  
<details>
<summary>code</summary>
  
```
class Solution:
    def shortestDistance(self, maze, start, destination):
        m, n, q, stopped = len(maze), len(maze[0]), [(0, start[0], start[1])], {(start[0], start[1]):0}
        while q:
            dist, x, y = heapq.heappop(q)
            if [x, y] == destination:
                return dist
            for i, j in ((-1, 0), (1, 0), (0, -1), (0, 1)):
                newX, newY, d = x, y, 0
                while 0 <= newX + i < m and 0 <= newY + j < n and maze[newX + i][newY + j] != 1:
                    newX += i
                    newY += j
                    d += 1
                if (newX, newY) not in stopped or dist + d < stopped[(newX, newY)]:
                    stopped[(newX, newY)] = dist + d
                    heapq.heappush(q, (dist + d, newX, newY))
        return -1  
  
```
</details>  
	
#### 510. Inorder Successor in BST II
  
<details>
<summary>code</summary>
  
```
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None
class Solution:
    def inorderSuccessor(self, node: 'Node') -> 'Node':
        # the successor is somewhere lower in the right subtree
        if node.right:
            node = node.right
            while node.left:
                node = node.left
            return node
        
        # the successor is somewhere upper in the tree
        while node.parent and node == node.parent.right:
            node = node.parent
        return node.parent  
  
```
</details>  
	
#### 527. Word Abbreviation
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def wordsAbbreviation(self, words):
        def abbrev(word, i = 0):
            if (len(word) - i <= 3): return word
            return word[:i+1] + str(len(word) - i - 2) + word[-1]

        N = len(words)
        ans = map(abbrev, words)
        prefix = [0] * N

        for i in xrange(N):
            while True:
                dupes = set()
                for j in xrange(i+1, N):
                    if ans[i] == ans[j]:
                        dupes.add(j)

                if not dupes: break
                dupes.add(i)
                for k in dupes:
                    prefix[k] += 1
                    ans[k] = abbrev(words[k], prefix[k])

        return ans  
  
```
</details>  
	
#### 531. Lonely Pixel I
  
<details>
<summary>code</summary>
  
```
def findLonelyPixel(self, picture):
    return sum(col.count('B') == 1 == picture[col.index('B')].count('B') for col in zip(*picture))  
  
```
</details> 
	
#### 536. Construct Binary Tree from String
  
<details>
<summary>code</summary>
  
```
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def str2tree(self, s: str) -> TreeNode:
        return self._str2treeInternal(s, 0)[0]
    
    def _getNumber(self, s: str, index: int) -> (int, int):
        
        is_negative = False
        
        # A negative number
        if s[index] == '-':
            is_negative = True
            index = index + 1
        
        number = 0
        while index < len(s) and s[index].isdigit():
            number = number * 10 + int(s[index])
            index += 1
        
        return number if not is_negative else -number, index
            
    
    def _str2treeInternal(self, s: str, index: int) -> (TreeNode, int):
        
        if index == len(s):
            return None, index
        
        # Start of the tree will always contain a number representing
        # the root of the tree. So we calculate that first.
        value, index = self._getNumber(s, index)
        node = TreeNode(value)
        
        # Next, if there is any data left, we check for the first subtree
        # which according to the problem statement will always be the left child.
        if index < len(s) and s[index] == '(':
            node.left, index = self._str2treeInternal(s, index + 1)
        
        # Indicates a right child
        if node.left and index < len(s) and s[index] == '(':
            node.right, index = self._str2treeInternal(s, index + 1)
        
        return node, index + 1 if index < len(s) and s[index] == ')' else index  
  
```
</details>  
	
#### 545. Boundary of Binary Tree
  
<details>
<summary>code</summary>
  
```
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution(object):
    def boundaryOfBinaryTree(self, root):
        def dfs_leftmost(node):
            if not node or not node.left and not node.right:
                return
            boundary.append(node.val)
            if node.left:
                dfs_leftmost(node.left)
            else:
                dfs_leftmost(node.right)

        def dfs_leaves(node):
            if not node:
                return
            dfs_leaves(node.left)
            if node != root and not node.left and not node.right:
                boundary.append(node.val)
            dfs_leaves(node.right)

        def dfs_rightmost(node):
            if not node or not node.left and not node.right:
                return
            if node.right:
                dfs_rightmost(node.right)
            else:
                dfs_rightmost(node.left)
            boundary.append(node.val)

        if not root:
            return []
        boundary = [root.val]
        dfs_leftmost(root.left)
        dfs_leaves(root)
        dfs_rightmost(root.right)
        return boundary  
  
```
</details>  
	
#### 549. Binary Tree Longest Consecutive Sequence II
  
<details>
<summary>code</summary>
  
```
class Solution:
    def longestConsecutive(self, root: TreeNode) -> int:
                
        def longest_path(root: TreeNode) -> List[int]:
            nonlocal maxval
            
            if not root:
                return [0, 0]
            
            inr = dcr = 1
            if root.left:
                left = longest_path(root.left)
                if (root.val == root.left.val + 1):
                    dcr = left[1] + 1
                elif (root.val == root.left.val - 1):
                    inr = left[0] + 1
            
            if root.right:
                right = longest_path(root.right)
                if (root.val == root.right.val + 1):
                    dcr = max(dcr, right[1] + 1)
                elif (root.val == root.right.val - 1):
                    inr = max(inr, right[0] + 1)
                    
            maxval = max(maxval, dcr + inr - 1)
            return [inr, dcr]
        
        maxval = 0
        longest_path(root)
        return maxval  
  
```
</details>  
	
#### 562. Longest Line of Consecutive One in Matrix
  
<details>
<summary>code</summary>
  
```
def longestLine(self, M):
    maxlen = 0
    currlen = collections.Counter()
    for i, row in enumerate(M):
        for j, a in enumerate(row):
            for key in i, j+.1, i+j+.2, i-j+.3:
                currlen[key] = (currlen[key] + 1) * a
                maxlen = max(maxlen, currlen[key])
    return maxlen  
  
```
</details>  
	
#### 568. Maximum Vacation Days
  
<details>
<summary>code</summary>
  
```
def maxVacationDays(self, flights, days):
    NINF = float('-inf')
    N, K = len(days), len(days[0])
    best = [NINF] * N
    best[0] = 0
    
    for t in xrange(K):
        cur = [NINF] * N
        for i in xrange(N):
            for j, adj in enumerate(flights[i]):
                if adj or i == j:
                    cur[j] = max(cur[j], best[i] + days[j][t])
        best = cur
    return max(best)  
  
```
</details>  
	
#### 582. Kill Process
  
<details>
<summary>code</summary>
  
```
def killProcess(self, pid, ppid, kill):
        d = collections.defaultdict(list)
        for c, p in zip(pid, ppid): d[p].append(c)
        bfs = [kill]
        for i in bfs: bfs.extend(d.get(i, []))
        return bfs  
  
```
</details>  
	
#### 588. Design In-Memory File System
  
<details>
<summary>code</summary>
  
```
from collections import defaultdict
class Node:
    def __init__(self):
        self.child=defaultdict(Node)
        self.content=""
        
class FileSystem(object):

    def __init__(self):
        self.root=Node()
        
    def find(self,path):#find and return node at path.
        curr=self.root
        if len(path)==1:
            return self.root
        for word in path.split("/")[1:]:
            curr=curr.child[word]
        return curr
        
    def ls(self, path):
        curr=self.find(path)
        if curr.content:#file path,return file name
            return [path.split('/')[-1]]
        return sorted(curr.child.keys())
		
    def mkdir(self, path):
        self.find(path)

    def addContentToFile(self, filePath, content):
        curr=self.find(filePath)
        curr.content+=content

    def readContentFromFile(self, filePath):
        curr=self.find(filePath)
        return curr.content  
  
```
</details>  
	
#### 604. Design Compressed String Iterator
  
<details>
<summary>code</summary>
  
```
import re
class StringIterator(object):
    def __init__(self, compressedString):
        self.tokens = []
        for token in re.findall('\D\d+', compressedString):
            self.tokens.append((token[0], int(token[1:])))
        self.tokens = self.tokens[::-1]

    def next(self):
        if not self.tokens: return ' '
        t, n = self.tokens.pop()
        if n > 1: 
            self.tokens.append((t, n - 1))
        return t

    def hasNext(self):
        return bool(self.tokens)  
  
```
</details>  
	
#### 616. Add Bold Tag in String
  
<details>
<summary>code</summary>
  
```
class Solution:
    def addBoldTag(self, s, dict):
        """
        :type s: str
        :type dict: List[str]
        :rtype: str
        """
        status = [False]*len(s)
        final = ""
        for word in dict:
            start = s.find(word)
            last = len(word)
            while start != -1:
                for i in range(start, last+start):
                    status[i] = True
                start = s.find(word,start+1)
        i = 0
        i = 0
        while i < len(s):
            if status[i]:
                final += "<b>"
                while i < len(s) and status[i]:
                    final += s[i]
                    i += 1
                final += "</b>"
            else:
                final += s[i]
                i += 1
        return final  
  
```
</details>  
	
#### 624. Maximum Distance in Arrays
  
<details>
<summary>code</summary>
  
```
 def maxDistance(self, arrays):
        res, curMin, curMax = 0, 10000, -10000
        for a in arrays :
            res = max(res, max(a[-1]-curMin, curMax-a[0]))
            curMin, curMax = min(curMin, a[0]), max(curMax, a[-1])
        return res  
  
```
</details>  
	
#### 631. Design Excel Sum Formula
  
<details>
<summary>code</summary>
  
```
class Excel(object):

    def __init__(self, H, W):
        self.M = [[{'v': 0, 'sum': None} for i in range(H)] for j in range(ord(W) - 64)]

    def set(self, r, c, v):
        self.M[r - 1][ord(c) - 65] = {'v': v, 'sum': None}

    def get(self, r, c):
        cell = self.M[r - 1][ord(c) - 65]
        if not cell['sum']: return cell['v']
        return sum(self.get(*pos) * cell['sum'][pos] for pos in cell['sum'])

    def sum(self, r, c, strs):
        self.M[r - 1][ord(c) - 65]['sum'] = self.parse(strs)
        return self.get(r, c)

    def parse(self, strs):
        c = collections.Counter()
        for s in strs:
            s, e = s.split(':')[0], s.split(':')[1] if ':' in s else s
            for i in range(int(s[1:]), int(e[1:]) + 1):
                for j in range(ord(s[0]) - 64, ord(e[0]) - 64 + 1):
                    c[(i, chr(j + 64))] += 1
        return c  
  
```
</details>  
	
#### 635. Design Log Storage System
  
<details>
<summary>code</summary>
  
```
class LogSystem(object):
    def __init__(self):
        self.logs = []

    def put(self, tid, timestamp):
        self.logs.append((tid, timestamp))
        
    def retrieve(self, s, e, gra):
        index = {'Year': 5, 'Month': 8, 'Day': 11, 
                 'Hour': 14, 'Minute': 17, 'Second': 20}[gra]
        start = s[:index]
        end = e[:index]
        
        return sorted(tid for tid, timestamp in self.logs
                      if start <= timestamp[:index] <= end)  
  
```
</details>  
	
#### 642. Design Search Autocomplete System
  
<details>
<summary>code</summary>
  
```
class TrieNode(object):
    def __init__(self):
        self.children = {}
        self.isEnd = False
        self.data = None
        self.rank = 0
        
class AutocompleteSystem(object):
    def __init__(self, sentences, times):
        self.root = TrieNode()
        self.keyword = ""
        for i, sentence in enumerate(sentences):
            self.addRecord(sentence, times[i])

    def addRecord(self, sentence, hot):
        p = self.root
        for c in sentence:
            if c not in p.children:
                p.children[c] = TrieNode()
            p = p.children[c]
        p.isEnd = True
        p.data = sentence
        p.rank -= hot
    
    def dfs(self, root):
        ret = []
        if root:
            if root.isEnd:
                ret.append((root.rank, root.data))
            for child in root.children:
                ret.extend(self.dfs(root.children[child]))
        return ret
        
    def search(self, sentence):
        p = self.root
        for c in sentence:
            if c not in p.children:
                return []
            p = p.children[c]
        return self.dfs(p)
    
    def input(self, c):
        results = []
        if c != "#":
            self.keyword += c
            results = self.search(self.keyword)
        else:
            self.addRecord(self.keyword, 1)
            self.keyword = ""
        return [item[1] for item in sorted(results)[:3]]  
  
```
</details>  
	
#### 644. Maximum Average Subarray II
  
<details>
<summary>code</summary>
  
```
def findMaxAverage(self, A, K):
    N = len(A)
    P = [0]
    for x in A:
        P.append(P[-1] + x)

    def d(x, y):
        return (P[y+1] - P[x]) / float(y+1-x)

    hull = collections.deque()
    ans = float('-inf')

    for j in xrange(K-1, N):
        while len(hull) >= 2 and d(hull[-2], hull[-1]-1) >= d(hull[-2], j-K):
            hull.pop()
        hull.append(j-K + 1)
        while len(hull) >= 2 and d(hull[0], hull[1]-1) <= d(hull[0], j):
            hull.popleft()
        ans = max(ans, d(hull[0], j))

    return ans  
  
```
</details>  
	
#### 651. 4 Keys Keyboard
  
<details>
<summary>code</summary>
  
```
def maxA(self, N):
    best = [0, 1]
    for x in xrange(2, N+1):
        cur = best[x-1] + 1
        for y in xrange(x-1):
            cur = max(cur, best[y] * (x-y-1))
        best.append(cur)
    return best[N]  
  
```
</details>  
	
#### 656. Coin Path
  
<details>
<summary>code</summary>
  
```
def cheapestJump(self, A, B):
        if not A or A[0] == -1: return []
        dp = [[float('inf')] for _ in A]
        dp[0] = [A[0], 1]
        for j in range(1, len(A)):
            if A[j] == -1: continue
            dp[j] = min([dp[i][0] + A[j]] + dp[i][1:] + [j + 1] for i in range(max(0, j - B), j))
        return dp[-1][1:] if dp[-1][0] < float('inf') else []  
  
```
</details>  
	
#### 694. Number of Distinct Islands
  
<details>
<summary>code</summary>
  
```
Approach: Hash By Path Signature	
class Solution:
    def numDistinctIslands(self, grid: List[List[int]]) -> int:

        # Do a DFS to find all cells in the current island.
        def dfs(row, col, direction):
            if row < 0 or col < 0 or row >= len(grid) or col >= len(grid[0]):
                return
            if (row, col) in seen or not grid[row][col]:
                return
            seen.add((row, col))
            path_signature.append(direction)
            dfs(row + 1, col, "D")
            dfs(row - 1, col, "U")
            dfs(row, col + 1, "R")
            dfs(row, col - 1, "L")
            path_signature.append("0")
        
        # Repeatedly start DFS's as long as there are islands remaining.
        seen = set()
        unique_islands = set()
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                path_signature = []
                dfs(row, col, "0")
                if path_signature:
                    unique_islands.add(tuple(path_signature))
        
        return len(unique_islands)  
  
```
</details>  
	
#### 702. Search in a Sorted Array of Unknown Size
  
<details>
<summary>code</summary>
  
```
class Solution:
    def search(self, reader, target):
        if reader.get(0) == target:
            return 0
        
        # search boundaries
        left, right = 0, 1
        while reader.get(right) < target:
            left = right
            right <<= 1
        
        # binary search
        while left <= right:
            pivot = left + ((right - left) >> 1)
            num = reader.get(pivot)
            
            if num == target:
                return pivot
            if num > target:
                right = pivot - 1
            else:
                left = pivot + 1
        
        # there is no target element
        return -1  
  
```
</details>  
	
#### 708. Insert into a Sorted Circular Linked List
  
<details>
<summary>code</summary>
  
```
class Solution:
    def insert(self, head: 'Node', insertVal: int) -> 'Node':

        if head == None:
            newNode = Node(insertVal, None)
            newNode.next = newNode
            return newNode
 
        prev, curr = head, head.next
        toInsert = False

        while True:
            
            if prev.val <= insertVal <= curr.val:
                # Case #1.
                toInsert = True
            elif prev.val > curr.val:
                # Case #2. where we locate the tail element
                # 'prev' points to the tail, i.e. the largest element!
                if insertVal >= prev.val or insertVal <= curr.val:
                    toInsert = True

            if toInsert:
                prev.next = Node(insertVal, curr)
                # mission accomplished
                return head

            prev, curr = curr, curr.next
            # loop condition
            if prev == head:
                break
        # Case #3.
        # did not insert the node in the loop
        prev.next = Node(insertVal, curr)
        return head  
  
```
</details>  
	
#### 716. Max Stack
  
<details>
<summary>code</summary>
  
```
class MaxStack(list):
    def push(self, x):
        m = max(x, self[-1][1] if self else None)
        self.append((x, m))

    def pop(self):
        return list.pop(self)[0]

    def top(self):
        return self[-1][0]

    def peekMax(self):
        return self[-1][1]

    def popMax(self):
        m = self[-1][1]
        b = []
        while self[-1][0] != m:
            b.append(self.pop())

        self.pop()
        map(self.push, reversed(b))
        return m  
  
```
</details>  
	
#### 723. Candy Crush
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def candyCrush(self, board):
        R, C = len(board), len(board[0])
        todo = False

        for r in xrange(R):
            for c in xrange(C-2):
                if abs(board[r][c]) == abs(board[r][c+1]) == abs(board[r][c+2]) != 0:
                    board[r][c] = board[r][c+1] = board[r][c+2] = -abs(board[r][c])
                    todo = True

        for r in xrange(R-2):
            for c in xrange(C):
                if abs(board[r][c]) == abs(board[r+1][c]) == abs(board[r+2][c]) != 0:
                    board[r][c] = board[r+1][c] = board[r+2][c] = -abs(board[r][c])
                    todo = True

        for c in xrange(C):
            wr = R-1
            for r in xrange(R-1, -1, -1):
                if board[r][c] > 0:
                    board[wr][c] = board[r][c]
                    wr -= 1
            for wr in xrange(wr, -1, -1):
                board[wr][c] = 0

        return self.candyCrush(board) if todo else board  
  
```
</details>  
	
#### 727. Minimum Window Subsequence
  
<details>
<summary>code</summary>
  
```
def minWindow(self, S, T):
	
    # Find - Get ending point of subsequence starting after S[s]
    def find_subseq(s):
        t = 0
        while s < len(S):
            if S[s] == T[t]:
                t += 1
                if t == len(T):
                    break
            s += 1
        
        return s if t == len(T) else None       # Ensure last character of T was found before loop ended
    
    # Improve - Get best starting point of subsequence ending at S[s]
    def improve_subseq(s):
        t = len(T) - 1
        while t >= 0:
            if S[s] == T[t]:
                t -= 1
            s -= 1
        
		return s+1
    
    s, min_len, min_window = 0, float('inf'), ''
    
    while s < len(S):
        end = find_subseq(s)            # Find end-point of subsequence
        if not end:
            break
            
        start = improve_subseq(end)     # Improve start-point of subsequence

		if end-start+1 < min_len:       # Track min length
            min_len = end-start+1
            min_window = S[start:end+1]
        
        s = start+1                     # Start next subsequence search

    return min_window  
  
```
</details>  
	
#### 734. Sentence Similarity
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def areSentencesSimilar(self, words1, words2, pairs):
        from collections import defaultdict
        if len(words1) != len(words2):
            return False
        words = defaultdict(set)
        for word1, word2 in pairs:
            words[word1].add(word2)
            words[word2].add(word1)
        for word1, word2 in zip(words1, words2):
            if word1 != word2 and word2 not in words[word1]:
                return False
        return True  
  
```
</details>  
	
#### 737. Sentence Similarity II
  
<details>
<summary>code</summary>
  
```
Approach #1: Depth-First Search
class Solution(object):
    def areSentencesSimilarTwo(self, words1, words2, pairs):
        if len(words1) != len(words2): return False
        graph = collections.defaultdict(list)
        for w1, w2 in pairs:
            graph[w1].append(w2)
            graph[w2].append(w1)

        for w1, w2 in zip(words1, words2):
            stack, seen = [w1], {w1}
            while stack:
                word = stack.pop()
                if word == w2: break
                for nei in graph[word]:
                    if nei not in seen:
                        seen.add(nei)
                        stack.append(nei)
            else:
                return False
        return True	
	
Approach #2: Union-Find
class DSU:
    def __init__(self, N):
        self.par = range(N)
    def find(self, x):
        if self.par[x] != x:
            self.par[x] = self.find(self.par[x])
        return self.par[x]
    def union(self, x, y):
        self.par[self.find(x)] = self.find(y)

class Solution(object):
    def areSentencesSimilarTwo(self, words1, words2, pairs):
        if len(words1) != len(words2): return False

        index = {}
        count = itertools.count()
        dsu = DSU(2 * len(pairs))
        for pair in pairs:
            for p in pair:
                if p not in index:
                    index[p] = next(count)
            dsu.union(index[pair[0]], index[pair[1]])

        return all(w1 == w2 or
                   w1 in index and w2 in index and
                   dsu.find(index[w1]) == dsu.find(index[w2])
                   for w1, w2 in zip(words1, words2))	
  
```
</details>  
	
#### 742. Closest Leaf in a Binary Tree
  
<details>
<summary>code</summary>
  
```
Approach #1: Convert to Graph
class Solution(object):
    def findClosestLeaf(self, root, k):
        graph = collections.defaultdict(list)
        def dfs(node, par = None):
            if node:
                graph[node].append(par)
                graph[par].append(node)
                dfs(node.left, node)
                dfs(node.right, node)

        dfs(root)
        queue = collections.deque(node for node in graph
                                  if node and node.val == k)
        seen = set(queue)

        while queue:
            node = queue.popleft()
            if node:
                if len(graph[node]) <= 1:
                    return node.val
                for nei in graph[node]:
                    if nei not in seen:
                        seen.add(nei)
                        queue.append(nei)
					
Approach #2: Annotate Closest Leaf
class Solution(object):
    def findClosestLeaf(self, root, k):
        annotation = {}
        def closest_leaf(root):
            if root not in annotation:
                if not root:
                    ans = float('inf'), None
                elif not root.left and not root.right:
                    ans = 0, root
                else:
                    d1, leaf1 = closest_leaf(root.left)
                    d2, leaf2 = closest_leaf(root.right)
                    ans = min(d1, d2) + 1, leaf1 if d1 < d2 else leaf2
                annotation[root] = ans
            return annotation[root]

        #Search for node.val == k
        path = []
        def dfs(node):
            if not node:
                return
            if node.val == k:
                path.append(node)
                return True
            path.append(node)
            ans1 = dfs(node.left)
            if ans1: return True
            ans2 = dfs(node.right)
            if ans2: return True
            path.pop()

        dfs(root)
        dist, leaf = float('inf'), None
        for i, node in enumerate(path):
            d0, leaf0 = closest_leaf(node)
            d0 += len(path) - 1 - i
            if d0 < dist:
                dist = d0
                leaf = leaf0

        return leaf.val					
  
```
</details>  
	
#### 750. Number Of Corner Rectangles
  
<details>
<summary>code</summary>
  
```
Approach #1: Count Corners
class Solution(object):
    def countCornerRectangles(self, grid):
        count = collections.Counter()
        ans = 0
        for row in grid:
            for c1, v1 in enumerate(row):
                if v1:
                    for c2 in xrange(c1+1, len(row)):
                        if row[c2]:
                            ans += count[c1, c2]
                            count[c1, c2] += 1
        return ans
	
Approach #2: Heavy and Light Rows
class Solution(object):
    def countCornerRectangles(self, grid):
        rows = [[c for c, val in enumerate(row) if val]
                for row in grid]
        N = sum(len(row) for row in grid)
        SQRTN = int(N**.5)

        ans = 0
        count = collections.Counter()
        for r, row in enumerate(rows):
            if len(row) >= SQRTN:
                target = set(row)
                for r2, row2 in enumerate(rows):
                    if r2 <= r and len(row2) >= SQRTN:
                        continue
                    found = sum(1 for c2 in row2 if c2 in target)
                    ans += found * (found - 1) / 2
            else:
                for pair in itertools.combinations(row, 2):
                    ans += count[pair]
                    count[pair] += 1

        return ans	
  
```
</details>  
	
#### 759. Employee Free Time
  
<details>
<summary>code</summary>
  
```
  
  def employeeFreeTime(self, schedule):

    ints = sorted([i for s in schedule for i in s], key=lambda x: x.start)
    res, pre = [], ints[0]
    for i in ints[1:]:
        if i.start <= pre.end and i.end > pre.end:
            pre.end = i.end
        elif i.start > pre.end:
            res.append(Interval(pre.end, i.start))
            pre = i
    return res
```
</details>  
	
#### 760. Find Anagram Mappings
  
<details>
<summary>code</summary>
  
```
Approach #1: Hash Table
class Solution(object):
    def anagramMappings(self, A, B):
        D = {x: i for i, x in enumerate(B)}
        return [D[x] for x in A]
	  
```
</details>  
	
#### 772. Basic Calculator III
  
<details>
<summary>code</summary>
  
```
class Solution:
	def calculate(self, s):
		"""
		:type s: str
		:rtype: int
		"""

		# first define a couple helper methods

		# operation helper to perform basic math operations
		def operation(op, second, first):
			if op == "+":
				return first + second
			elif op == "-":
				return first - second
			elif op == "*":
				return first * second
			elif op == "/":  # integer division
				return first // second

		# calculate the relative precedence of the the operators "()" > "*/" > "+="
		# and determine if we want to do a pre-calculation in the stack
		# (when current_op is <= op_from_ops)
		def precedence(current_op, op_from_ops):
			if op_from_ops == "(" or op_from_ops == ")":
				return False
			if (current_op == "*" or current_op == "/") and (op_from_ops == "+" or op_from_ops == "-"):
				return False
			return True

		if not s:
			return 0
		# define two stack: nums to store the numbers and ops to store the operators
		nums, ops = [], []
		i = 0
		while i < len(s):
			c = s[i]
			if c == " ":
				i += 1
				continue
			elif c.isdigit():
				num = int(c)
				while i < len(s) - 1 and s[i + 1].isdigit():
					num = num * 10 + int(s[i + 1])
					i += 1
				nums.append(num)
			elif c == "(":
				ops.append(c)
			elif c == ")":
				# do the math when we encounter a ')' until '('
				while ops[-1] != "(":
					nums.append(operation(ops.pop(), nums.pop(), nums.pop()))
				ops.pop()
			elif c in ["+", "-", "*", "/"]:
				while len(ops) != 0 and precedence(c, ops[-1]):
					nums.append(operation(ops.pop(), nums.pop(), nums.pop()))
				ops.append(c)
			i += 1

		while len(ops) > 0:
			nums.append(operation(ops.pop(), nums.pop(), nums.pop()))

		return nums.pop()  
  
```
</details>  
	
#### 774. Minimize Max Distance to Gas Station
  
<details>
<summary>code</summary>
  
```
Approach #1: Dynamic Programming  
class Solution(object):
    def minmaxGasDist(self, stations, K):
        N = len(stations)
        deltas = [stations[i+1] - stations[i] for i in xrange(N-1)]
        dp = [[0.0] * (K+1) for _ in xrange(N-1)]
        #dp[i][j] = answer for deltas[:i+1] when adding j gas stations
        for i in xrange(K+1):
            dp[0][i] = deltas[0] / float(i + 1)

        for p in xrange(1, N-1):
            for k in xrange(K+1):
                dp[p][k] = min(max(deltas[p] / float(x+1), dp[p-1][k-x])
                               for x in xrange(k+1))

        return dp[-1][K]
	
Approach #2: Heap
class Solution(object):
    def minmaxGasDist(self, stations, K):
        pq = [] #(-part_length, original_length, num_parts)
        for i in xrange(len(stations) - 1):
            x, y = stations[i], stations[i+1]
            pq.append((x-y, y-x, 1))
        heapq.heapify(pq)

        for _ in xrange(K):
            negnext, orig, parts = heapq.heappop(pq)
            parts += 1
            heapq.heappush(pq, (-(orig / float(parts)), orig, parts))

        return -pq[0][0]	

  
```
</details>  
	
#### 1055. Shortest Way to Form String
  
<details>
<summary>code</summary>
  
```
:::python
def shortestWay(self, source: str, target: str) -> int:
    inverted_index = collections.defaultdict(list)
    for i, ch in enumerate(source):
        inverted_index[ch].append(i)

    loop_cnt = 1
    i = -1
    for ch in target:
        if ch not in inverted_index:
            return -1
        offset_list_for_ch = inverted_index[ch]
        # bisect_left(A, x) returns the smallest index j s.t. A[j] >= x. If no such index j exists, it returns len(A).
        j = bisect.bisect_left(offset_list_for_ch, i)
        if j == len(offset_list_for_ch):
            loop_cnt += 1
            i = offset_list_for_ch[0] + 1
        else:
            i = offset_list_for_ch[j] + 1

    return loop_cnt  
  
```
</details> 
	
#### 1056. Confusing Number
  
<details>
<summary>code</summary>
  
```
def confusingNumber(self, N):
        S = str(N)
        rotation = {"0" : "0", "1" : "1", "6" : "9", "8" : "8", "9" : "6"}
        result = []
        
        for c in S[::-1]:           # iterate in reverse
            if c not in rotation:
                return False
            result.append(rotation[c])
                
        return "".join(result) != S  
  
```
</details>  
	
#### 1057. Campus Bikes
  
<details>
<summary>code</summary>
  
```
def assignBikes(self, workers, bikes):
        distances = []     # distances[worker] is tuple of (distance, worker, bike) for each bike 
        for i, (x, y) in enumerate(workers):
            distances.append([])
            for j, (x_b, y_b) in enumerate(bikes):
                distance = abs(x - x_b) + abs(y - y_b)
                distances[-1].append((distance, i, j))
            distances[-1].sort(reverse = True)  # reverse so we can pop the smallest distance
        
        result = [None] * len(workers)
        used_bikes = set()
        queue = [distances[i].pop() for i in range(len(workers))]   # smallest distance for each worker
        heapq.heapify(queue)
        
        while len(used_bikes) < len(workers):
            _, worker, bike = heapq.heappop(queue)
            if bike not in used_bikes:
                result[worker] = bike
                used_bikes.add(bike)
            else:
                heapq.heappush(queue, distances[worker].pop())  # bike used, add next closest bike
        
        return result  
  
```
</details> 
	
#### 1059. All Paths from Source Lead to Destination
  
<details>
<summary>code</summary>
  
```
class Solution:
    
    # We don't use the state WHITE as such anywhere. Instead, the "null" value in the states array below is a substitute for WHITE.
    GRAY = 1
    BLACK = 2

    def leadsToDestination(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        graph = self.buildDigraph(n, edges)
        return self.leadsToDest(graph, source, destination, [None] * n)
        
    def leadsToDest(self, graph, node, dest, states):
        
        # If the state is GRAY, this is a backward edge and hence, it creates a Loop.
        if states[node] != None:
            return states[node] == Solution.BLACK
        
        # If this is a leaf node, it should be equal to the destination.
        if len(graph[node]) == 0:
            return node == dest
        
        # Now, we are processing this node. So we mark it as GRAY.
        states[node] = Solution.GRAY
        
        for next_node in graph[node]:
            
            # If we get a `false` from any recursive call on the neighbors, we short circuit and return from there.
            if not self.leadsToDest(graph, next_node, dest, states):
                return False;
        
        # Recursive processing done for the node. We mark it BLACK.
        states[node] = Solution.BLACK
        return True
        
    def buildDigraph(self, n, edges):
        graph = [[] for _ in range(n)]
        
        for edge in edges:
            graph[edge[0]].append(edge[1])
            
        return graph     
  
```
</details>  
	
#### 1060. Missing Element in Sorted Array
  
<details>
<summary>code</summary>
  
```
Approach 1: One Pass
class Solution:
    def missingElement(self, nums: List[int], k: int) -> int:
        # Return how many numbers are missing until nums[idx]
        missing = lambda idx: nums[idx] - nums[0] - idx
                
        n = len(nums)
        # If kth missing number is larger than 
        # the last element of the array
        if k > missing(n - 1):
            return nums[-1] + k - missing(n - 1) 

        idx = 1
        # find idx such that 
        # missing(idx - 1) < k <= missing(idx)
        while missing(idx) < k:
            idx += 1

        # kth missing number is greater than nums[idx - 1]
        # and less than nums[idx]
        return nums[idx - 1] + k - missing(idx - 1)
			       
Approach 2: Binary Search			       
class Solution:
    def missingElement(self, nums: List[int], k: int) -> int:
        # Return how many numbers are missing until nums[idx]
        missing = lambda idx: nums[idx] - nums[0] - idx
            
        n = len(nums)
        # If kth missing number is larger than 
        # the last element of the array
        if k > missing(n - 1):
            return nums[-1] + k - missing(n - 1) 
        
        left, right = 0, n - 1
        # find left = right index such that 
        # missing(left - 1) < k <= missing(left)
        while left != right:
            pivot = left + (right - left) // 2
            
            if missing(pivot) < k:
                left = pivot + 1
            else:
                right = pivot 
        
        # kth missing number is greater than nums[left - 1]
        # and less than nums[left]
        return nums[left - 1] + k - missing(left - 1) 	
  
```
</details>  
	
#### 1061. Lexicographically Smallest Equivalent String
  
<details>
<summary>code</summary>
  
```
class UF:
    def __init__(self, n):
        self.ps = list(range(n))

    def find(self, x):
        if self.ps[x] != x:
            self.ps[x] = self.find(self.ps[x])
        return self.ps[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return
        if px < py:
            self.ps[py] = px
        else:
            self.ps[px] = py


class Solution:
    def smallestEquivalentString(self, A: str, B: str, S: str) -> str:

        uf = UF(26)
        for a, b in zip(A, B):
            if a != b:
                uf.union(ord(a) - 97, ord(b) - 97)

        return ''.join((chr(uf.find(ord(c) - 97) + 97) for c in list(S)))  
  
```
</details> 
	
#### 1062. Longest Repeating Substring
  
<details>
<summary>code</summary>
  
```
class Solution:
    def search(self, L: int, n: int, S: str) -> str:
        """
        Search a substring of given length
        that occurs at least 2 times.
        @return start position if the substring exits and -1 otherwise.
        """
        # Subtask 2 : TODO
        
    def longestRepeatingSubstring(self, S: str) -> str:
        n = len(S)
        
        # binary search, L = repeating string length
        left, right = 1, n
        while left <= right:
            L = left + (right - left) // 2
            if self.search(L, n, S) != -1:
                left = L + 1
            else:
                right = L - 1
               
        return left - 1
			   
Approach 1: Binary Search + Hashset of Already Seen Strings
class Solution:
    def search(self, L: int, n: int, S: str) -> str:
        """
        Search a substring of given length
        that occurs at least 2 times.
        @return start position if the substring exits and -1 otherwise.
        """
        seen = set()
        for start in range(0, n - L + 1):
            tmp = S[start:start + L]
            if tmp in seen:
                return start
            seen.add(tmp)
        return -1
        
    def longestRepeatingSubstring(self, S: str) -> str:
        n = len(S)
        
        # binary search, L = repeating string length
        left, right = 1, n
        while left <= right:
            L = left + (right - left) // 2
            if self.search(L, n, S) != -1:
                left = L + 1
            else:
                right = L - 1
               
        return left - 1
			   
Approach 2: Binary Search + Hashset of Hashes of Already Seen Strings
class Solution:
    def search(self, L: int, n: int, S: str) -> str:
        """
        Search a substring of given length
        that occurs at least 2 times.
        @return start position if the substring exits and -1 otherwise.
        """
        seen = set()
        for start in range(0, n - L + 1):
            tmp = S[start:start + L]
            h = hash(tmp)
            if h in seen:
                return start
            seen.add(h)
        return -1
        
    def longestRepeatingSubstring(self, S: str) -> str:
        n = len(S)
        
        # binary search, L = repeating string length
        left, right = 1, n
        while left <= right:
            L = left + (right - left) // 2
            if self.search(L, n, S) != -1:
                left = L + 1
            else:
                right = L - 1
               
        return left - 1			   
			   
Approach 3: Binary Search + Rabin-Karp
class Solution:
    def search(self, L: int, a: int, modulus: int, n: int, nums: List[int]) -> str:
        """
        Rabin-Karp with polynomial rolling hash.
        Search a substring of given length
        that occurs at least 2 times.
        @return start position if the substring exits and -1 otherwise.
        """
        # compute the hash of string S[:L]
        h = 0
        for i in range(L):
            h = (h * a + nums[i]) % modulus
              
        # already seen hashes of strings of length L
        seen = {h} 
        # const value to be used often : a**L % modulus
        aL = pow(a, L, modulus) 
        for start in range(1, n - L + 1):
            # compute rolling hash in O(1) time
            h = (h * a - nums[start - 1] * aL + nums[start + L - 1]) % modulus
            if h in seen:
                return start
            seen.add(h)
        return -1
        
    def longestRepeatingSubstring(self, S: str) -> str:
        n = len(S)
        # convert string to array of integers
        # to implement constant time slice
        nums = [ord(S[i]) - ord('a') for i in range(n)]
        # base value for the rolling hash function
        a = 26
        # modulus value for the rolling hash function to avoid overflow
        modulus = 2**24
        
        # binary search, L = repeating string length
        left, right = 1, n
        while left <= right:
            L = left + (right - left) // 2
            if self.search(L, a, modulus, n, nums) != -1:
                left = L + 1
            else:
                right = L - 1
               
        return left - 1			   
  
```
</details>  
	
#### 1064. Fixed Point
  
<details>
<summary>code</summary>
  
```
 def fixedPoint(self, A):
        l, r = 0, len(A) - 1
        while l < r:
            m = (l + r) / 2
            if A[m] - m < 0:
                l = m + 1
            else:
                r = m
        return l if A[l] == l else -1  
  
```
</details>  
	
#### 1065. Index Pairs of a String
  
<details>
<summary>code</summary>
  
```
class TrieNode:
    def __init__(self):
        self.children, self.is_word = {}, False

    @staticmethod
    def construct_trie(words):
        root = TrieNode()
        for word in words:
            node = root
            for c in word:
                node.children.setdefault(c, TrieNode())
                node = node.children[c]
            node.is_word = True
        return root

class Solution:
    def indexPairs(self, text, words):
        res, trie = [], TrieNode.construct_trie(words)
        for l in range(len(text)):
            node = trie
            for r in range(l, len(text)):
                if text[r] not in node.children:
                    break
                node = node.children[text[r]]
                if node.is_word:
                    res.append((l, r))
        return res  
  
```
</details>  
	
#### 1066. Campus Bikes II
  
<details>
<summary>code</summary>
  
```
 def assignBikes(self, workers, bikes):
        def dis(i, j):
            return abs(workers[i][0] - bikes[j][0]) + abs(workers[i][1] - bikes[j][1])
        h = [[0, 0, 0]]
        seen = set()
        while True:
            cost, i, taken = heapq.heappop(h)
            if (i, taken) in seen: continue
            seen.add((i, taken))
            if i == len(workers):
                return cost
            for j in xrange(len(bikes)):
                if taken & (1 << j) == 0:
                    heapq.heappush(h, [cost + dis(i, j), i + 1, taken | (1 << j)])  
  
```
</details>  
	
#### 1086. High Five
  
<details>
<summary>code</summary>
  
```
class Solution:
    def highFive(self, items: List[List[int]]) -> List[List[int]]:
        items.sort(reverse=True)
        
        res = []
        curr = []
        idx = items[0][0]
        
        for i, val in items:
            if i == idx:
                if len(curr) < 5:
                    curr.append(val)
            else:
                res.append([idx, sum(curr) // len(curr)])
                curr = [val]
                idx = i
        
        res.append([idx, sum(curr) // len(curr)])
        
        res = res[::-1]
        
        return res
Solution 2, use priority queue for each id,

class Solution:
    def highFive(self, items: List[List[int]]) -> List[List[int]]:
        d = collections.defaultdict(list)
        
        for idx, val in items:
            heapq.heappush(d[idx], val)
            
            if len(d[idx]) > 5:
                heapq.heappop(d[idx])
        
        res = [[i, sum(d[i]) // len(d[i])] for i in sorted(d)]
        
        return res  
  
```
</details>  
	
#### 1087. Brace Expansion
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def expand(self, S):
        """
        :type S: str
        :rtype: List[str]
        """
        self.res = []
        def helper(s, word):
            if not s:
                self.res.append(word)
            else:
                if s[0] == "{":
                    i = s.find("}")
                    for letter in s[1:i].split(','):
                        helper(s[i+1:], word+letter)
                else:
                    helper(s[1:], word + s[0])
        helper(S, "")
        self.res.sort()
        return self.res  
  
```
</details>  
	
#### 1088. Confusing Number II
  
<details>
<summary>code</summary>
  
```
class Solution(object):
    def confusingNumberII(self, N):
        """
        :type N: int
        :rtype: int
        """
        valid = [0,1,6,8,9]
        mapping = {0: 0,1: 1,6: 9,8: 8, 9: 6}

        self.count = 0

        def backtrack(v, rotation,digit):
            if v: 
                if v != rotation: 
                    self.count += 1  
            for i in valid: 
                if v*10+i > N:
                    break 
                else:
                    backtrack(v*10+i, mapping[i]*digit + rotation, digit*10)
        
        backtrack(1,1, 10)
        backtrack(6,9,10)
        backtrack(8,8,10)
        backtrack(9,6,10)

        return self.count     
  
```
</details>
	
#### 1099. Two Sum Less Than K
  
<details>
<summary>code</summary>
  
```
class Solution:
    def twoSumLessThanK(self, nums: List[int], k: int) -> int:
        nums.sort()
        answer = -1
        left = 0
        right = len(nums) -1
        while left < right:
            sum = nums[left] + nums[right]
            if (sum < k):
                answer = max(answer, sum)
                left += 1
            else:
                right -= 1
        return answer  
  
```
</details> 
	
#### 1100. Find K-Length Substrings With No Repeated Characters
  
<details>
<summary>code</summary>
  
```
def numKLenSubstrNoRepeats(self, S, K):
        res, i = 0, 0
        cur = set()
        for j in xrange(len(S)):
            while S[j] in cur:
                cur.remove(S[i])
                i += 1
            cur.add(S[j])
            res += j - i + 1 >= K
        return res  
  
```
</details>  
	
#### 1101. The Earliest Moment When Everyone Become Friends
  
<details>
<summary>code</summary>
  
```
class Solution:
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        # First, we need to sort the events in chronological order.
        logs.sort(key = lambda x: x[0])

        uf = UnionFind(n)
        # Initially, we treat each individual as a separate group.
        group_cnt = n

        # We merge the groups along the way.
        for timestamp, friend_a, friend_b in logs:
            if uf.union(friend_a, friend_b):
                group_cnt -= 1

            # The moment when all individuals are connected to each other.
            if group_cnt == 1:
                return timestamp

        # There are still more than one groups left,
        #  i.e. not everyone is connected.
        return -1


class UnionFind:

    def __init__(self, size):
        self.group = [group_id for group_id in range(size)]
        self.rank = [0] * size

    def find(self, person):
        if self.group[person] != person:
            self.group[person] = self.find(self.group[person])
        return self.group[person]

    def union(self, a, b):
        """
            return: true if a and b are not connected before
                otherwise, connect a with b and then return false
        """
        group_a = self.find(a)
        group_b = self.find(b)
        is_merged = False
        if group_a == group_b:
            return is_merged

        is_merged = True
        # Merge the lower-rank group into the higher-rank group.
        if self.rank[group_a] > self.rank[group_b]:
            self.group[group_b] = group_a
        elif self.rank[group_a] < self.rank[group_b]:
            self.group[group_a] = group_b
        else:
            self.group[group_a] = group_b
            self.rank[group_b] += 1

        return is_merged  
  
```
</details>  
	
#### 1102. Path With Maximum Minimum Value
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maximumMinimumPath(self, A: List[List[int]]) -> int:
        R, C = len(A), len(A[0])
        parent = [i for i in range(R * C)]
        dire = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        seen = [[0 for _ in range(C)] for _ in range(R)]
        
        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]
        
        def union(x, y):
            rx, ry = find(x), find(y)
            if rx != ry:
                parent[ry] = rx
        
        points = [(x, y) for x in range(R) for y in range(C)]
        points.sort(key = lambda x: A[x[0]][x[1]], reverse = True)
        
        for x, y in points:
            seen[x][y] = 1
            for dx, dy in dire:
                nx, ny = x + dx, y + dy
                if 0 <= nx < R and 0 <= ny < C and seen[nx][ny]:
                    union(x * C + y, nx * C + ny)
            if find(0) == find(R * C - 1):
                return A[x][y]
        return -1  
	
	
class Solution:
    def maximumMinimumPath(self, A: List[List[int]]) -> int:
        dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        R, C = len(A), len(A[0])
        
        def check(val):
            memo = [[0 for _ in range(C)] for _ in range(R)]
            
            def dfs(x,y):
                if x == R - 1 and y == C - 1:
                    return True
                memo[x][y] = 1
                for d in dire:
                    nx = x + d[0]
                    ny = y + d[1]
                    if 0 <= nx < R and 0 <= ny < C and not memo[nx][ny] and A[nx][ny] >= val and dfs(nx,ny):
                        return True
                return False
            
            return dfs(0,0)   
        
        unique = set()
        ceiling = min(A[0][0], A[-1][-1])
        for r in range(R):
            for c in range(C):
                if A[r][c] <= ceiling:
                    unique.add(A[r][c])
                
        arr = sorted(unique)
        l, r = 0, len(arr) - 1
        while l <= r:
            m = l + (r - l) // 2
            # if check(m):
            if check(arr[m]):
                # cause we're trying to find the MAXIMUM of 'minimum'
                l = m + 1
            else:
                r = m - 1
        return arr[r]
	
class Solution:
    def maximumMinimumPath(self, A: List[List[int]]) -> int:
        dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        R, C = len(A), len(A[0])
        
        maxHeap = [(-A[0][0], 0, 0)]
        seen = [[0 for _ in range(C)] for _ in range(R)]
        while maxHeap:
            val, x, y = heapq.heappop(maxHeap)
            # seen[x][y] = 1 # got TLE
            if x == R - 1 and y == C - 1: return -val
            for dx, dy in dire:
                nx, ny = x + dx, y + dy
                if 0 <= nx < R and 0 <= ny < C and not seen[nx][ny]:
                    seen[nx][ny] = 1 # passed
                    heapq.heappush(maxHeap, (max(val, -A[nx][ny]), nx, ny))
        return -1	
  
```
</details>  
	
#### 1120. Maximum Average Subtree
  
<details>
<summary>code</summary>
  
```
 def maximumAverageSubtree(self, root):
        self.res = 0
        def helper(root):
            if not root: return [0, 0.0]
            n1, s1 = helper(root.left)
            n2, s2 = helper(root.right)
            n = n1 + n2 + 1
            s = s1 + s2 + root.val
            self.res = max(self.res, s / n)
            return [n, s]
        helper(root)
        return self.res  
  
```
</details>  
	
#### 1133. Largest Unique Number
  
<details>
<summary>code</summary>
  
```
class Solution:
    def largestUniqueNumber(self, A: List[int]) -> int:        
        return max([-1] + [k for k,v in collections.Counter(A).items() if v == 1])  
  
```
</details>  
	
#### 1134. Armstrong Number
  
<details>
<summary>code</summary>
  
```
import math
class Solution(object):
    def isArmstrong(self, N):
        total = 0 # Keep track of the sum for each digit to the power of k
		
		# Calculate k
		# One method of calculating k is to convert N to a string and then get the length of that.
		# This second method is from this: https://brilliant.org/wiki/finding-digits-of-a-number/
		# It's a pretty useful and interesting formula.
        k = math.floor(math.log10(N)) + 1 
        
		# Create a copy of N so that we can compare the total to N without modifying its original value.
        c = N
        while c: # Terminates when c is 0
            digit = c % 10 # Extract the digit
            total += digit ** k 
            c //= 10
        
        return total == N  
  
```
</details>  
	
#### 1135. Connecting Cities With Minimum Cost
  
<details>
<summary>code</summary>
  
```
class Solution:
    '''Connecting Cities with Minimum Cost == Find Minimum Spanning Tree'''
    def minimumCost(self, N: int, connections: List[List[int]]) -> int:
        '''
        Prim's Algorithm:
        1) Initialize a tree with a single vertex, chosen
        arbitrarily from the graph.
        2) Grow the tree by one edge: of the edges that
        connect the tree to vertices not yet in the tree,
        find the minimum-weight edge, and transfer it to the tree.
        3) Repeat step 2 (until all vertices are in the tree).
        '''
        # city1 <-> city2 may have multiple different cost connections,
        # so use a list of tuples. Nested dict will break algorithm.
        G = collections.defaultdict(list)
        for city1, city2, cost in connections:
            G[city1].append((cost, city2))
            G[city2].append((cost, city1))
        
        queue = [(0, N)]  # [1] Arbitrary starting point N costs 0.
        visited = set()
        total = 0
        while queue and len(visited) < N: # [3] Exit if all cities are visited.
            # cost is always least cost connection in queue.
            cost, city = heapq.heappop(queue)
            if city not in visited:
                visited.add(city)
                total += cost # [2] Grow tree by one edge.
                for edge_cost, next_city in G[city]:
                    heapq.heappush(queue, (edge_cost, next_city))
        return total if len(visited) == N else -1
    
    def minimumCost(self, N: int, connections: List[List[int]]) -> int:
        '''
        Kruskal's Algorithm:
        1) Create a forest F (a set of trees), where each vertex in 
        the graph is a separate tree.
        2) Create a set S containing all the edges in the graph.
        3) While S is nonempty and F is not yet spanning (fully connected):
            3A) Remove an edge with minimum weight from S
            3B) If the removed edge connects two different trees then 
            add it to the forest F, combining two trees into a single tree.
        '''
        def find(city):
            # Recursively re-set city's parent to its parent's parent.
            # Build the bush: ideally each tree/set is of height 1.
            if parent[city] != city:
                parent[city] = find(parent[city])
            return parent[city]
        
        def union(c1, c2):
            root1, root2 = find(c1), find(c2)
            if root1 == root2:
                return False
            parent[root2] = root1  # Always join roots!
            return True
        
        # [1] Keep track of disjoint sets. Initially each city is its own set.
        parent = {city: city for city in range(1, N+1)}
        # [2] Sort connections so we are always picking minimum cost edge.
        connections.sort(key=lambda x: x[2])
        total = 0
        for city1, city2, cost in connections:  # [3A]  
            if union(city1, city2):  # [3B]
                total += cost
        # Check that all cities are connected.
        root = find(N)
        return total if all(root == find(city) for city in range(1, N+1)) else -1  
  
```
</details>  
	
#### 1136. Parallel Courses
  
<details>
<summary>code</summary>
  
```
class Solution:
    def minimumSemesters(self, N: int, relations: List[List[int]]) -> int:
        graph = {i: [] for i in range(1, N + 1)}
        for start_node, end_node in relations:
            graph[start_node].append(end_node)

        visited = {}

        def dfs(node: int) -> int:
            # return the longest path (inclusive)
            if node in visited:
                return visited[node]
            else:
                # mark as visiting
                visited[node] = -1

            max_length = 1
            for end_node in graph[node]:
                length = dfs(end_node)
                # we meet a cycle!
                if length == -1:
                    return -1
                else:
                    max_length = max(length+1, max_length)
            # mark as visited
            visited[node] = max_length
            return max_length

        max_length = -1
        for node in graph.keys():
            length = dfs(node)
            # we meet a cycle!
            if length == -1:
                return -1
            else:
                max_length = max(length, max_length)
        return max_length  
  
```
</details>  
	
#### 1150. Check If a Number Is Majority Element in a Sorted Array
  
<details>
<summary>code</summary>
  
```
def isMajorityElement(self, nums, target):
    N = len(nums)
    if nums[N // 2] != target:
        return False
    lo = bisect.bisect_left(nums, target)
    hi = bisect.bisect_right(nums, target)
    return hi - lo > N // 2


def isMajorityElement(self, nums, target):

        def search(a, x):
            lo, hi = 0, len(a)
            while lo < hi:
                mid = (lo + hi) // 2
                if a[mid] < x:
                    lo = mid + 1
                else:
                    hi = mid
            return lo
            
        N = len(nums)
        if nums[N // 2] != target:
            return False
        lo = search(nums, target)
        hi = search(nums, target + 1)
        return hi - lo > N // 2


def isMajorityElement(self, nums, target):
    k = len(nums) // 2
    if nums[k] != target:
        return False
    lo = bisect.bisect_left(nums, target, 0, k)
    hi = lo + k
    return hi < len(nums) and nums[hi] == target  
  
```
</details>  
	
#### 1151. Minimum Swaps to Group All 1's Together
  
<details>
<summary>code</summary>
  
```
Approach 1: Sliding Window with Two Pointers
class Solution:
    def minSwaps(self, data: List[int]) -> int:
        ones = sum(data)
        cnt_one = max_one = 0
        left = right = 0
        while right < len(data):
            # updating the number of 1's by adding the new element
            cnt_one += data[right]
            right += 1
            # maintain the length of the window to ones
            if right - left > ones:
                # updating the number of 1's by removing the oldest element
                cnt_one -= data[left]
                left += 1
            # record the maximum number of 1's in the window
            max_one = max(max_one, cnt_one)
        return ones - max_one	
		
		
Approach 2: Sliding Window with Deque (Double-ended Queue)	
class Solution:
    def minSwaps(self, data: List[int]) -> int:
        ones = sum(data)
        cnt_one = max_one = 0

        # maintain a deque with the size = ones
        deque = collections.deque()
        for i in range(len(data)):

            # we would always add the new element into the deque
            deque.append(data[i])
            cnt_one += data[i]

            # when there are more than ones elements in the deque,
            # remove the leftmost one
            if len(deque) > ones:
                cnt_one -= deque.popleft()
            max_one = max(max_one, cnt_one)
        return ones - max_one  
  
```
</details>  
	
#### 1153. String Transforms Into Another String
  
<details>
<summary>code</summary>
  
```
class Solution:
    def canConvert(self, str1: str, str2: str) -> bool:
        if str1 == str2:
            return True
        
        conversion_mappings = dict()
        unique_characters_in_str2 = set()
        
        # Make sure that no character in str1 is mapped to multiple characters in str2.
        for letter1, letter2 in zip(str1, str2):
            if letter1 not in conversion_mappings:
                conversion_mappings[letter1] = letter2
                unique_characters_in_str2.add(letter2)
            elif conversion_mappings[letter1] != letter2:
                # letter1 maps to 2 different characters, so str1 cannot transform into str2.
                return False
        
        
        if len(unique_characters_in_str2) < 26:
            # No character in str1 maps to 2 or more different characters in str2 and there
            # is at least one temporary character that can be used to break any loops.
            return True
        
        # The conversion mapping forms one or more cycles and there are no temporary 
        # characters that we can use to break the loops, so str1 cannot transform into str2.
        return False  
  
```
</details>  
		
#### 1165. Single-Row Keyboard
  
<details>
<summary>code</summary>
  
```
class Solution:
    def calculateTime(self, keyboard: str, word: str) -> int:
        cur_index = 0
        time = 0
        for w in word:
            next_index = keyboard.index(w)
            time += abs(next_index - cur_index)
            cur_index = next_index
        return time  
  
```
</details>  
		
#### 1166. Design File System
  
<details>
<summary>code</summary>
  
```

# The TrieNode data structure.
class TrieNode(object):
    def __init__(self, name):
        self.map = defaultdict(TrieNode)
        self.name = name
        self.value = -1

class FileSystem:

    def __init__(self):
        
        # Root node contains the empty string.
        self.root = TrieNode("")

    def createPath(self, path: str, value: int) -> bool:
        
        # Obtain all the components
        components = path.split("/")
        
        # Start "curr" from the root node.
        cur = self.root
        
        # Iterate over all the components.
        for i in range(1, len(components)):
            name = components[i]
            
            # For each component, we check if it exists in the current node's dictionary.
            if name not in cur.map:
                
                # If it doesn't and it is the last node, add it to the Trie.
                if i == len(components) - 1:
                    cur.map[name] = TrieNode(name)
                else:
                    return False
            cur = cur.map[name]
        
        # Value not equal to -1 means the path already exists in the trie. 
        if cur.value!=-1:
            return False
        
        cur.value = value
        return True

    def get(self, path: str) -> int:
        
        # Obtain all the components
        cur = self.root
        
        # Start "curr" from the root node.
        components = path.split("/")
        
        # Iterate over all the components.
        for i in range(1, len(components)):
            
            # For each component, we check if it exists in the current node's dictionary.
            name = components[i]
            if name not in cur.map:
                return -1
            cur = cur.map[name]
        return cur.value  
  
```
</details>  
		
#### 1167. Minimum Cost to Connect Sticks
  
<details>
<summary>code</summary>
  
```
def connectSticks(self, A):
        heapq.heapify(A)
        res = 0
        while len(A) > 1:
            x, y = heapq.heappop(A), heapq.heappop(A)
            res += x + y
            heapq.heappush(A, x + y)
        return res  
  
```
</details>  
	
#### 1168. Optimize Water Distribution in a Village
  
<details>
<summary>code</summary>
  
```
Approach 1: Prim's Algorithm with Heap
class Solution:
    def minCostToSupplyWater(self, n: int, wells: List[int], pipes: List[List[int]]) -> int:

        # bidirectional graph represented in adjacency list
        graph = defaultdict(list)

        # add a virtual vertex indexed with 0.
        #   then add an edge to each of the house weighted by the cost
        for index, cost in enumerate(wells):
            graph[0].append((cost, index + 1))

        # add the bidirectional edges to the graph
        for house_1, house_2, cost in pipes:
            graph[house_1].append((cost, house_2))
            graph[house_2].append((cost, house_1))

        # A set to maintain all the vertex that has been added to
        #   the final MST (Minimum Spanning Tree),
        #   starting from the vertex 0.
        mst_set = set([0])

        # heap to maitain the order of edges to be visited,
        #   starting from the edges originated from the vertex 0.
        # Note: we can start arbitrarily from any node.
        heapq.heapify(graph[0])
        edges_heap = graph[0]

        total_cost = 0
        while len(mst_set) < n + 1:
            cost, next_house = heapq.heappop(edges_heap)
            if next_house not in mst_set:
                # adding the new vertex into the set
                mst_set.add(next_house)
                total_cost += cost
                # expanding the candidates of edge to choose from
                #   in the next round
                for new_cost, neighbor_house in graph[next_house]:
                    if neighbor_house not in mst_set:
                        heapq.heappush(edges_heap, (new_cost, neighbor_house))

        return total_cost	
			      
Approach 2: Kruskal's Algorithm with Union-Find
class UnionFind:
    """
        Implementation of UnionFind without load-balancing.
    """
    def __init__(self, size) -> None:
        """
        container to hold the group id for each member
        Note: the index of member starts from 1,
            thus we add one more element to the container.
        """
        self.group = [i for i in range(size + 1)]
        # the rank of each node for later rebalancing
        self.rank = [0] * (size + 1)

    def find(self, person: int) -> int:
        """
            return the group id that the person belongs to
        """
        if self.group[person] != person:
            self.group[person] = self.find(self.group[person])
        return self.group[person]

    def union(self, person_1: int, person_2: int) -> bool:
        """
            Join the groups together.
            return:
                false when the two persons belong to the same group already,
                otherwise true
        """
        group_1 = self.find(person_1)
        group_2 = self.find(person_2)
        if group_1 == group_2:
            return False

        # attach the group of lower rank to the group with higher rank
        if self.rank[group_1] > self.rank[group_2]:
            self.group[group_2] = group_1
        elif self.rank[group_1] < self.rank[group_2]:
            self.group[group_1] = group_2
        else:
            self.group[group_1] = group_2
            self.rank[group_2] += 1

        return True


class Solution:
    def minCostToSupplyWater(self, n: int, wells: List[int], pipes: List[List[int]]) -> int:
        ordered_edges = []
        # add the virtual vertex (index with 0) along with the new edges.
        for index, weight in enumerate(wells):
            ordered_edges.append((weight, 0, index+1))

        # add the existing edges
        for house_1, house_2, weight in pipes:
            ordered_edges.append((weight, house_1, house_2))

        # sort the entire edges by their weights
        ordered_edges.sort(key=lambda x: x[0])

        # iterate through the ordered edges
        uf = UnionFind(n)
        total_cost = 0
        for cost, house_1, house_2 in ordered_edges:
            # determine if we should add the new edge to the final MST
            if uf.union(house_1, house_2):
                total_cost += cost

        return total_cost		
  
```
</details>  
	
#### 1180. Count Substrings with Only One Distinct Letter
  
<details>
<summary>code</summary>
  
```
class Solution:
    def countLetters(self, S: str) -> int:
        total = 1
        substrings = [0] * len(S)
        substrings[0] = 1
        for i in range(1, len(S)):
            if S[i - 1] == S[i]:
                substrings[i] = substrings[i-1] + 1
            else:
                substrings[i] = 1
            total += substrings[i]
        return total
  
	class Solution:
    def countLetters(self, S: str) -> int:
        total = 1
        count = 1
        for i in range(1, len(S)):
            if S[i] == S[i-1]:
                count += 1
            else:
                count = 1
            total += count
        return total	
```
</details>  
		
#### 1182. Shortest Distance to Target Color
  
<details>
<summary>code</summary>
  
```
class Solution:
    def shortestDistanceColor(self, colors: List[int], queries: List[List[int]]) -> List[int]:
        hashmap = collections.defaultdict(list)
        for i,c in enumerate(colors):
            hashmap[c].append(i)

        query_results = []
        for i, (target, color) in enumerate(queries):
            if color not in hashmap:
                query_results.append(-1)
                continue

            index_list = hashmap[color]
            # use bisect from Python standard library
            # more details: https://docs.python.org/3/library/bisect.html
            insert = bisect.bisect_left(index_list, target)

            # compare the index on the left and right of insert
            # make sure it will not fall out of the index_list
            left_nearest = abs(index_list[max(insert - 1, 0)] - target)
            right_nearest = abs(index_list[min(insert, len(index_list) - 1)] - target)
            query_results.append(min(left_nearest, right_nearest))

        return query_results  
		
Approach 2: Pre-computed
class Solution:
    def shortestDistanceColor(self, colors: List[int], queries: List[List[int]]) -> List[int]:
        # initializations
        n = len(colors)
        rightmost = [0, 0, 0]
        leftmost = [n - 1, n - 1, n - 1]

        distance = [[-1] * n for _ in range(3)]

        # looking forward
        for i in range(n):
            color = colors[i] - 1
            for j in range(rightmost[color], i + 1):
                distance[color][j] = i - j
            rightmost[color] = i + 1

        # looking backward
        for i in range(n - 1, -1, -1):
            color = colors[i] - 1
            for j in range(leftmost[color], i - 1, -1):
                # if the we did not find a target color on its right
                # or we find out that a target color on its left is
                # closer to the one on its right
                if distance[color][j] == -1 or distance[color][j] > j - i:
                    distance[color][j] = j - i
            leftmost[color] = i - 1

        return [distance[color - 1][index] for index,color in queries]		
  
```
</details> 
		
#### 1183. Maximum Number of Ones
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maximumNumberOfOnes(self, width: int, height: int, sideLength: int, maxOnes: int) -> int:
        res = []
        for i in range(sideLength):
            for j in range(sideLength):
                res += [((width - i - 1) // sideLength + 1) * ((height - j - 1) // sideLength + 1)]
        res = sorted(res,reverse = True)
        return sum(res[:maxOnes])
		
Or 1-liner just for fun..

class Solution:
    def maximumNumberOfOnes(self, width: int, height: int, sideLength: int, maxOnes: int) -> int:
        return sum(sorted([((width-i-1)//sideLength+1)*((height-j-1)//sideLength+1) for i in range(sideLength) for j in range(sideLength)],reverse = True)[:maxOnes])
		
  
```
</details>  
		
#### 1196. How Many Apples Can You Put into the Basket
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maxNumberOfApples(self, arr: List[int]) -> int:
        arr.sort()
        apples = units = 0

        for _, weight in enumerate(arr):
            units += weight
            if units > 5000:
                break

            apples += 1
        return apples  
  
Approach 2: Min-Heap
class Solution:
    def maxNumberOfApples(self, arr: List[int]) -> int:
        # note that build a heap only requires O(N)
        # more details: https://stackoverflow.com/questions/9755721/how-can-building-a-heap-be-on-time-complexity
        heapq.heapify(arr)
        apples = units = 0

        # note that arr[0] always represents the smallest
        # element in the min-heap
        while arr and units + arr[0] <= 5000:
            units += heapq.heappop(arr)
            apples += 1
        return apples		
		
Approach 3: Bucket Sort
class Solution:
    def maxNumberOfApples(self, arr: List[int]) -> int:
        # initialize the bucket to store all elements
        size = max(arr) + 1
        bucket = [0] * size
        for weight in arr:
            bucket[weight] += 1

        apples = units = 0
        for i in range(size):
            # if we have apples of i units of weight
            if bucket[i] != 0:
                # we need to make sure that:
                # 1. we do not take more apples than those provided
                # 2. we do not exceed 5000 units of weight
                take = min(bucket[i], (5000 - units) // i)
                if take == 0:
                    break

                apples += take
                units += take * i
        return apples		
		
```
</details>  
		
#### 1197. Minimum Knight Moves
  
<details>
<summary>code</summary>
  
```
class Solution:
    def minKnightMoves(self, x: int, y: int) -> int:
        # the offsets in the eight directions
        offsets = [(1, 2), (2, 1), (2, -1), (1, -2),
                   (-1, -2), (-2, -1), (-2, 1), (-1, 2)]

        def bfs(x, y):
            visited = set()
            queue = deque([(0, 0)])
            steps = 0

            while queue:
                curr_level_cnt = len(queue)
                # iterate through the current level
                for i in range(curr_level_cnt):
                    curr_x, curr_y = queue.popleft()
                    if (curr_x, curr_y) == (x, y):
                        return steps

                    for offset_x, offset_y in offsets:
                        next_x, next_y = curr_x + offset_x, curr_y + offset_y
                        if (next_x, next_y) not in visited:
                            visited.add((next_x, next_y))
                            queue.append((next_x, next_y))

                # move on to the next level
                steps += 1

        return bfs(x, y)  
  
```
</details>  
		
#### 1198. Find Smallest Common Element in All Rows
  
<details>
<summary>code</summary>
  
```
def smallestCommonElement(self, M):
        c = collections.Counter()
        for row in M:
            for a in row:
                c[a] += 1
                if c[a] == len(M):
                    return a
        return -1  
  
```
</details>  
		
#### 1199. Minimum Time to Build Blocks
  
<details>
<summary>code</summary>
  
```
class Solution:
    def minBuildTime(self, blocks: List[int], split: int) -> int:
        heapq.heapify(blocks)
        while len(blocks) > 1:
            block_1 = heapq.heappop(blocks)
            block_2 = heapq.heappop(blocks)
            new_block = max(block_1, block_2) + split
            heapq.heappush(blocks, new_block)
        return blocks[0]  
  
```
</details>  
		
#### 1213. Intersection of Three Sorted Arrays
  
<details>
<summary>code</summary>
  
```
class Solution:
    def arraysIntersection(self, arr1: List[int], arr2: List[int], arr3: List[int]) -> List[int]:
        ans = []
        # prepare three pointers to iterate through three arrays
        # p1, p2, and p3 point to the beginning of arr1, arr2, and arr3 accordingly
        p1 = p2 = p3 = 0
        while p1 < len(arr1) and p2 < len(arr2) and p3 < len(arr3):
            if arr1[p1] == arr2[p2] == arr3[p3]:
                ans.append(arr1[p1])
                p1 += 1
                p2 += 1
                p3 += 1
            else:
                if arr1[p1] < arr2[p2]:
                    p1 += 1
                elif arr2[p2] < arr3[p3]:
                    p2 += 1
                else:
                    p3 += 1
        return ans  
  
```
</details>  
		
#### 1214. Two Sum BSTs
  
<details>
<summary>code</summary>
  
```
class Solution:
    def twoSumBSTs(self, root1: TreeNode, root2: TreeNode, target: int) -> bool:
        stack, s = [], set()
        # traverse the first tree 
        # and store node complements (target - val) in hashset
        while stack or root1:
            while root1:
                stack.append(root1)
                root1 = root1.left
            root1 = stack.pop()
            s.add(target - root1.val)
            root1 = root1.right
        
        # traverse the second tree 
        # and check if one of the values exists in hashset
        while stack or root2:
            while root2:
                stack.append(root2)
                root2 = root2.left
            root2 = stack.pop()
            if root2.val in s:
                return True
            root2 = root2.right

        return False  
  
```
</details>  
		
#### 1216. Valid Palindrome III
  
<details>
<summary>code</summary>
  
```
class Solution:
    def isValidPalindrome(self, s: str, k: int) -> bool:
        n = len(s)
        dp = [[0] * (n + 1) for _ in range(n + 1)] 
        for i in range(n + 1): 
            for j in range(n + 1): 
                if not i or not j: 
                    dp[i][j] = i or j 
                elif s[i - 1] == s[n - j]: 
                    dp[i][j] = dp[i - 1][j - 1] 
                else: 
                    dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1])
        return dp[n][n] <= k * 2  
  
```
</details> 
		
#### 1228. Missing Number In Arithmetic Progression
  
<details>
<summary>code</summary>
  
```
Soluton 1: Arithmetic Sequence Sum
		
		
 def missingNumber(self, A):
        return (min(A) + max(A)) * (len(A) + 1) / 2 - sum(A)		
		
Solution 2: Binary Search	
		
def missingNumber(self, A):
        n = len(A)
        d = (A[-1] - A[0]) / n
        left, right = 0, n
        while left < right:
            mid = (left + right) / 2
            if A[mid] == A[0] + d * mid:
                left = mid + 1
            else:
                right = mid
        return A[0] + d * left  
  
```
</details>  
		
#### 1229. Meeting Scheduler
  
<details>
<summary>code</summary>
  
```
class Solution:
    def minAvailableDuration(self, slots1: List[List[int]], slots2: List[List[int]], duration: int) -> List[int]:

        slots1.sort()
        slots2.sort()
        pointer1 = pointer2 = 0

        while pointer1 < len(slots1) and pointer2 < len(slots2):
            # find the boundaries of the intersection, or the common slot
            intersect_right = min(slots1[pointer1][1], slots2[pointer2][1])
            intersect_left = max(slots1[pointer1][0],slots2[pointer2][0])
            if intersect_right - intersect_left >= duration:
                return [intersect_left, intersect_left + duration]
            # always move the one that ends earlier
            if slots1[pointer1][1]< slots2[pointer2][1]:
                pointer1 += 1
            else:
                pointer2 += 1
        return [] 

Method 2: Heap:		
		
class Solution:
    def minAvailableDuration(self, slots1: List[List[int]], slots2: List[List[int]], duration: int) -> List[int]:
        # build up a heap containing time slots last longer than duration
        timeslots = list(filter(lambda x: x[1] - x[0] >= duration, slots1 + slots2))
        heapq.heapify(timeslots)

        while len(timeslots) > 1:
            start1, end1 = heapq.heappop(timeslots)
            start2, end2 = timeslots[0]
            if end1 >= start2 + duration:
                return [start2, start2 + duration]
        return []		
  
```
</details>
		
#### 1231. Divide Chocolate
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maximizeSweetness(self, sweetness: List[int], k: int) -> int:
        # Initialize the left and right boundaries.
        # left = 1 and right = (total sweetness) / (number of people).
        number_of_people = k + 1
        left = min(sweetness)
        right = sum(sweetness) // number_of_people
        
        while left < right:
            # Get the middle index between left and right boundary indexes.
            # cur_sweetness stands for the total sweetness for the current person.
            # people_with_chocolate stands for the number of people that have 
            # a piece of chocolate of sweetness greater than or equal to mid.  
            mid = (left + right + 1) // 2
            cur_sweetness = 0
            people_with_chocolate = 0
            
            # Start assigning chunks to the current person.
            for s in sweetness:
                cur_sweetness += s
                
                # If the total sweetness is no less than mid, this means we can break off
                # the current piece and move on to assigning chunks to the next person.
                if cur_sweetness >= mid:
                    people_with_chocolate += 1
                    cur_sweetness = 0

            if people_with_chocolate >= k + 1:
                left = mid
            else:
                right = mid - 1
                
        return right  
  
```
</details>  
		
#### 1236. Web Crawler
  
<details>
<summary>code</summary>
  
```
class Solution:
    def crawl(self, startUrl: str, htmlParser: 'HtmlParser') -> List[str]:
        visited = {startUrl}
        domain = startUrl.split("http://")[1].split("/")[0]
        ans = [startUrl]
        queue = collections.deque([startUrl])
        while queue:
            for _ in range(len(queue)):
                url = queue.popleft()
                check = htmlParser.getUrls(url)
                for new_url in check:
                    if new_url in visited:
                        continue
                    if new_url.split("http://")[1].split("/")[0] != domain:
                        continue
                    ans.append(new_url)
                    visited.add(new_url)
                    queue.append(new_url)        
        return ans  
  
```
</details>  
		
#### 1244. Design A Leaderboard
  
<details>
<summary>code</summary>
  
```
class Leaderboard:

    def __init__(self):
        self.scores = {}

    def addScore(self, playerId: int, score: int) -> None:
        if playerId not in self.scores:
            self.scores[playerId] = 0
        self.scores[playerId] += score

    def top(self, K: int) -> int:
    
        # This is a min-heap by default in Python.
        heap = []
        for x in self.scores.values():
            heapq.heappush(heap, x)
            if len(heap) > K:
                heapq.heappop(heap)
        res = 0
        while heap:
            res += heapq.heappop(heap)
        return res

    def reset(self, playerId: int) -> None:
        self.scores[playerId] = 0  
		
Approach : Using a TreeMap / SortedMap		

from sortedcontainers import SortedDict

class Leaderboard:

    def __init__(self):
        self.scores = {}
        self.sortedScores = SortedDict()

    def addScore(self, playerId: int, score: int) -> None:

        # The scores dictionary simply contains the mapping from the
        # playerId to their score. The sortedScores contain a BST with 
        # key as the score and value as the number of players that have
        # that score.     
        if playerId not in self.scores:
            self.scores[playerId] = score
            self.sortedScores[-score] = self.sortedScores.get(-score, 0) + 1
        else:
            preScore = self.scores[playerId]
            val = self.sortedScores.get(-preScore)
            if val == 1:
                del self.sortedScores[-preScore]
            else:
                self.sortedScores[-preScore] = val - 1    
            
            newScore = preScore + score;
            self.scores[playerId] = newScore
            self.sortedScores[-newScore] = self.sortedScores.get(-newScore, 0) + 1
        
    def top(self, K: int) -> int:
        count, total = 0, 0;

        for key, value in self.sortedScores.items():
            times = self.sortedScores.get(key)
            for _ in range(times): 
                total += -key;
                count += 1;
                
                # Found top-K scores, break.
                if count == K:
                    break;
                
            # Found top-K scores, break.
            if count == K:
                break;
        
        return total;

    def reset(self, playerId: int) -> None:
        preScore = self.scores[playerId]
        if self.sortedScores[-preScore] == 1:
            del self.sortedScores[-preScore]
        else:
            self.sortedScores[-preScore] -= 1
        del self.scores[playerId];		
  
```
</details>  
		
#### 1245. Tree Diameter
  
<details>
<summary>code</summary>
  
```
class Solution:
    def treeDiameter(self, edges: List[List[int]]) -> int:

        # build the adjacency list representation of the graph
        graph = [set() for i in range(len(edges)+1)]
        for edge in edges:
            u, v = edge
            graph[u].add(v)
            graph[v].add(u)

        def bfs(start):
            """
             return the farthest node from the 'start' node
               and the distance between them.
            """
            visited = [False] * len(graph)

            visited[start] = True
            queue = deque([start])
            distance = -1
            last_node = None
            while queue:
                next_queue = deque()
                while queue:
                    next_node = queue.popleft()
                    for neighbor in graph[next_node]:
                        if not visited[neighbor]:
                            visited[neighbor] = True
                            next_queue.append(neighbor)
                            last_node = neighbor
                distance += 1
                queue = next_queue

            return last_node, distance

        # 1). find one of the farthest nodes
        farthest_node, distance_1 = bfs(0)
        # 2). find the other farthest node
        #  and the distance between two farthest nodes
        another_farthest_node, distance_2 = bfs(farthest_node)

        return distance_2  
		
Approach 2: Centroids of Graph via BFS
		
class Solution:
    def treeDiameter(self, edges: List[List[int]]) -> int:

        # build the adjacency list representation of the graph.
        graph = [set() for i in range(len(edges)+1)]
        for edge in edges:
            u, v = edge
            graph[u].add(v)
            graph[v].add(u)

        # find the outer most nodes, _i.e._ leaf nodes
        leaves = []
        for vertex, links in enumerate(graph):
            if len(links) == 1:
                leaves.append(vertex)

        # "peel" the graph layer by layer,
        #   until we have the centroids left.
        layers = 0
        vertex_left = len(edges) + 1
        while vertex_left > 2:
            vertex_left -= len(leaves)
            next_leaves = []
            for leaf in leaves:
                # the only neighbor left on the leaf node.
                neighbor = graph[leaf].pop()
                graph[neighbor].remove(leaf)
                if len(graph[neighbor]) == 1:
                    next_leaves.append(neighbor)
            layers += 1
            leaves = next_leaves

        return layers * 2 + (0 if vertex_left == 1 else 1)
		
Approach 3: DFS (Depth-First Search)
		
class Solution:
    def treeDiameter(self, edges: List[List[int]]) -> int:

        # build the adjacency list representation of the graph.
        graph = [set() for i in range(len(edges)+1)]
        for edge in edges:
            u, v = edge
            graph[u].add(v)
            graph[v].add(u)

        diameter = 0

        def dfs(curr, visited):
            """
                return the max distance
                  starting from the 'curr' node to its leaf nodes
            """
            nonlocal diameter

            # the top 2 distance starting from this node
            top_1_distance, top_2_distance = 0, 0

            distance = 0
            visited[curr] = True

            for neighbor in graph[curr]:
                if not visited[neighbor]:
                    distance = 1 + dfs(neighbor, visited)

                if distance > top_1_distance:
                    top_1_distance, top_2_distance = distance, top_1_distance
                elif distance > top_2_distance:
                    top_2_distance = distance

            # with the top 2 distance, we can update the current diameter
            diameter = max(diameter, top_1_distance + top_2_distance)

            return top_1_distance

        visited = [False for i in range(len(graph))]
        dfs(0, visited)

        return diameter		
  
```
</details>  
		
#### 1259. Handshakes That Don't Cross
  
<details>
<summary>code</summary>
  
```
@functools.lru_cache(None)
    def numberOfWays(self, n):
        return sum(self.numberOfWays(i) * self.numberOfWays(n - 2 - i) for i in range(0, n, 2)) if n else 1

Solution 2: Catalan Numbers
 def numberOfWays(self, n):
        res = 1
        for i in xrange(1, n / 2 + 1):
            res *= n - i + 1
            res /= i
        return res / (n / 2 + 1) % (10**9 + 7)	
  
```
</details>  
	
#### 1265. Print Immutable Linked List in Reverse
  
<details>
<summary>code</summary>
  
```
✔️ Solution 1: Using Recursive

class Solution:
    def printLinkedListInReverse(self, head: 'ImmutableListNode') -> None:
        if head == None: return
        self.printLinkedListInReverse(head.getNext())
        head.printValue()
Complexity:

Time: O(N)
Space: O(N)
✔️ Solution 2: Using Stack

class Solution:
    def printLinkedListInReverse(self, head: 'ImmutableListNode') -> None:
        st = []
        while head != None:
            st.append(head)
            head = head.getNext()
        while st:
            st.pop().printValue()
Complexity:

Time: O(N)
Space: O(N)
✔️ Follow-up 1:Constant space complexity?

class Solution:
    def printLinkedListInReverse(self, head: 'ImmutableListNode') -> None:
        def count(head):
            cnt = 0
            while head:
                cnt += 1
                head = head.getNext()
            return cnt
        
        def printAt(head, index):
            for i in range(index):
                head = head.getNext()
            head.printValue()
        
        n = count(head)
        for i in range(n-1, -1, -1):
            printAt(head, i)
Complexity:

Time: O(N^2)
Space: O(1)
✔️ Follow-up 2: Linear time complexity and less than linear space complexity?

class Solution:
    def printLinkedListInReverse(self, head: 'ImmutableListNode') -> None:
        def count(head):
            cnt = 0
            while head:
                cnt += 1
                head = head.getNext()
            return cnt
        
        def getHeadNodes(head, step):
            cnt = 0
            ans = []
            while head:
                if cnt % step == 0:
                    ans.append(head)
                head = head.getNext()
                cnt += 1
            return ans
        
        def printReverse(head, step):
            cnt = 0
            st = []
            while head != None and cnt < step:
                st.append(head)
                head = head.getNext()
                cnt += 1
            while st:
                st.pop().printValue()
        
        step = int(sqrt(count(head))) + 1
        headNodes = getHeadNodes(head, step)
        while headNodes:
            printReverse(headNodes.pop(), step)
Complexity:

Time: O(N)
Space: O(Sqrt(N))  
  
```
</details> 
	
#### 1272. Remove Interval
  
<details>
<summary>code</summary>
  
```
class Solution:
    def removeInterval(self, intervals: List[List[int]], toBeRemoved: List[int]) -> List[List[int]]:

        remove_start, remove_end = toBeRemoved
        output = []

        for start, end in intervals:
            # If there are no overlaps, add the interval to the list as is.
            if start > remove_end or end < remove_start:
                output.append([start, end])
            else:
                # Is there a left interval we need to keep?
                if start < remove_start:
                    output.append([start, remove_start])
                # Is there a right interval we need to keep?
                if end > remove_end:
                    output.append([remove_end, end])

        return output  
  
```
</details>  
	
#### 1274. Number of Ships in a Rectangle
  
<details>
<summary>code</summary>
  
```
class Solution:
    def countShips(self, sea: 'Sea', topRight: 'Point', bottomLeft: 'Point') -> int:
        # If the current rectangle does not contain any ships, return 0.         
        if (bottomLeft.x > topRight.x) or (bottomLeft.y > topRight.y):
            return 0
        if not sea.hasShips(topRight, bottomLeft):
            return 0

        # If the rectangle represents a single point, a ship is located.
        if (topRight.x == bottomLeft.x) and (topRight.y == bottomLeft.y):
            return 1

        # Recursively check each of the 4 sub-rectangles for ships.
        mid_x = (topRight.x + bottomLeft.x) // 2
        mid_y = (topRight.y + bottomLeft.y) // 2
        return self.countShips(sea, Point(mid_x, mid_y), bottomLeft) + \
               self.countShips(sea, topRight, Point(mid_x + 1, mid_y + 1)) + \
               self.countShips(sea, Point(mid_x, topRight.y), Point(bottomLeft.x, mid_y + 1)) + \
               self.countShips(sea, Point(topRight.x, mid_y), Point(mid_x + 1, bottomLeft.y))
  
  
```
</details>  
	
#### 1426. Counting Elements
  
<details>
<summary>code</summary>
  
```
def countElements(self, arr: List[int]) -> int:
    count = 0
    for x in arr:
        if x + 1 in arr:
            count += 1
    return count

# Note that we could also do this as a one-liner generator comprehension. 
# return sum(1 for x in arr if x + 1 in arr)  
  
Approach 2: Search with HashSet
def countElements(self, arr: List[int]) -> int:
    hash_set = set(arr)
    count = 0
    for x in arr:
        if x + 1 in hash_set:
            count += 1
    return count	
	
Approach 3: Search with Sorted Array	
def countElements(self, arr: List[int]) -> int:
    arr.sort()
    count = 0
    run_length = 1
    for i in range(len(arr)):
        if arr[i - 1] != arr[i]:
            if arr[i - 1] + 1 == arr[i]:
                count += run_length
            run_length = 0
        run_length += 1
    return count
	
```
</details>  
	
#### 1427. Perform String Shifts
  
<details>
<summary>code</summary>
  
```
class Solution:
    def stringShift(self, string: str, shift: List[List[int]]) -> str:
        for direction, amount in shift:
            amount %= len(string)
            if direction == 0:
                # Move necessary amount of characters from start to end
                string = string[amount:] + string[:amount]
            else:
                # Move necessary amount of characters from end to start
                string = string[-amount:] + string[:-amount]
        return string  
	
Approach 2: Compute Net Shift
	
class Solution:
    def stringShift(self, string: str, shift: List[List[int]]) -> str:

        # Add up the left shifts and right shifts.
        overall_shifts = [0, 0]
        for direction, amount in shift:
            overall_shifts[direction] += amount
        left_shifts, right_shifts = overall_shifts
        
        # Determine which shift (if any) to perform.
        if left_shifts > right_shifts:
            left_shifts = (left_shifts - right_shifts) % len(string)
            string = string[left_shifts:] + string[:left_shifts]
        else:
            right_shifts = (right_shifts - left_shifts) % len(string)
            string = string[-right_shifts:] + string[:-right_shifts]

        return string	
	
class Solution:
    def stringShift(self, s: str, shift: List[List[int]]) -> str:

        # Count the number of left shifts. A right shift is a negative left shift.
        left_shifts = 0
        for direction, amount in shift:
            if direction == 1:
                amount = -amount
            left_shifts += amount
            
        # Convert back to a positive, do left shifts, and return.
        left_shifts %= len(s)
        s = s[left_shifts:] + s[:left_shifts]
        return s	
  
```
</details>  
	
#### 1428. Leftmost Column with at Least a One
  
<details>
<summary>code</summary>
  
```
class Solution:
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        rows, cols = binaryMatrix.dimensions()
        smallest_index = cols
        # Go through each of the rows.
        for row in range(rows):
            # Linear seach for the first 1 in this row.
            for col in range(cols):
                if binaryMatrix.get(row, col) == 1:
                    smallest_index = min(smallest_index, col)
                    break
        # If we found an index, we should return it. Otherwise, return -1.
        return -1 if smallest_index == cols else smallest_index  
	
Approach 2: Binary Search Each Row
class Solution:
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        rows, cols = binaryMatrix.dimensions()
        smallest_index = cols
        for row in range(rows):
            # Binary Search for the first 1 in the row.
            lo = 0
            hi = cols - 1
            while lo < hi:
                mid = (lo + hi) // 2
                if binaryMatrix.get(row, mid) == 0:
                    lo = mid + 1
                else:
                    hi = mid
            # If the last element in the search space is a 1, then this row
            # contained a 1.
            if binaryMatrix.get(row, lo) == 1:
                smallest_index = min(smallest_index, lo)
        # If smallest_index is still set to cols, then there were no 1's in 
        # the grid. 
        return -1 if smallest_index == cols else smallest_index	
		    
Approach 3: Start at Top Right, Move Only Left and Down		    
		    
class Solution:
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        
        rows, cols = binaryMatrix.dimensions()
        
        # Set pointers to the top-right corner.
        current_row = 0
        current_col = cols - 1
        
        # Repeat the search until it goes off the grid.
        while current_row < rows and current_col >= 0:
            if binaryMatrix.get(current_row, current_col) == 0:
                current_row += 1
            else:
                current_col -= 1
        
        # If we never left the last column, it must have been all 0's.
        return current_col + 1 if current_col != cols - 1 else -1		    
  
```
</details>  
	
#### 1429. First Unique Number
  
<details>
<summary>code</summary>
  
```
from collections import deque

class FirstUnique:

    def __init__(self, nums: List[int]):
        self._queue = deque(nums)
        self._is_unique = {}
        for num in nums:
            # Notice that we're calling the "add" method of FirstUnique; not of the queue. 
            self.add(num)
        
    def showFirstUnique(self) -> int:
        # We need to start by "cleaning" the queue of any non-uniques at the start.
        # Note that we know that if a value is in the queue, then it is also in
        # is_unique, as the implementation of add() guarantees this.
        while self._queue and not self._is_unique[self._queue[0]]:
            self._queue.popleft()
        # Check if there is still a value left in the queue. There might be no uniques.
        if self._queue:
            return self._queue[0] # We don't want to actually *remove* the value.
        return -1
        
    def add(self, value: int) -> None:
        # Case 1: We need to add the number to the queue and mark it as unique. 
        if value not in self._is_unique:
            self._is_unique[value] = True
            self._queue.append(value)
        # Case 2 and 3: We need to mark the number as no longer unique.
        else:
            self._is_unique[value] = False  
	
Approach 3: LinkedHashSet for Queue, and HashMap of Unique-Statuses
	
# In Python, we have to make do with the OrderedDict class. We can use it as a Set by setting
# the values to None.

from collections import OrderedDict

class FirstUnique:

    def __init__(self, nums: List[int]):
        self._queue = OrderedDict()
        self._is_unique = {}
        for num in nums:
            # Notice that we're calling the "add" method of FirstUnique; not of the queue. 
            self.add(num)
        
    def showFirstUnique(self) -> int:
        # Check if there is still a value left in the queue. There might be no uniques.
        if self._queue:
            # We don't want to actually *remove* the value.
            # Seeing as OrderedDict has no "get first" method, the way that we can get
            # the first value is to create an iterator, and then get the "next" value
            # from that. Note that this is O(1).
            return next(iter(self._queue))
        return -1
        
    def add(self, value: int) -> None:
        # Case 1: We need to add the number to the queue and mark it as unique. 
        if value not in self._is_unique:
            self._is_unique[value] = True
            self._queue[value] = None
        # Case 2: We need to mark the value as no longer unique and then 
        # remove it from the queue.
        elif self._is_unique[value]:
            self._is_unique[value] = False
            self._queue.pop(value)
        # Case 3: We don't need to do anything; the number was removed from the queue
        # the second time it occurred.	
  
```
</details>  
	
#### 1474. Delete N Nodes After M Nodes of a Linked List
  
<details>
<summary>code</summary>
  
```
class Solution:
    def deleteNodes(self, head: ListNode, m: int, n: int) -> ListNode:
        dummy = ListNode(None)
        dummy.next = head
        i = 0
        while head:
            if i < m-1:
                i += 1
            else:
                j = 0
                while j < n and head.next:
                    head.next = head.next.next
                    j += 1
                i = 0
            head = head.next
        return dummy.next  
  
```
</details>  
	
#### 1485. Clone Binary Tree With Random Pointer
  
<details>
<summary>code</summary>
  
```
class Solution:
    def copyRandomBinaryTree(self, root: 'Node') -> 'NodeCopy':
        if not root:   # Protection against a null root input  
            return None

        # Step 1. Create a copy of each node
        copy = {}
        stack = [root]

        while stack:
            node = stack.pop()
            copy[node] = NodeCopy(node.val)
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)

        # Step 2. Connect the copied nodes together
        stack = [root]

        while stack:
            node = stack.pop()
            if node.left:
                copy[node].left = copy[node.left]
                stack.append(node.left)
            if node.right:
                copy[node].right = copy[node.right]
                stack.append(node.right)
            if node.random:
                copy[node].random = copy[node.random]

        return copy[root]  
	
Approach 2: Recursive Tree Traversal + Hashmap	
	
class Solution:
    def copyRandomBinaryTree(self, root: 'Node') -> 'NodeCopy':
        if not root:
            return None

        def dfs_copy(node):
            if not node:
                return

            copy[node] = NodeCopy(node.val)

            dfs_copy(node.left)
            dfs_copy(node.right)

            return node

        def dfs_connect(node):
            if not node:
                return

            if node.left:
                copy[node].left = copy[node.left]
            if node.right:
                copy[node].right = copy[node.right]
            if node.random:
                copy[node].random = copy[node.random]

            dfs_connect(node.left)
            dfs_connect(node.right)

            return node     

        copy = {}
        dfs_copy(root)
        dfs_connect(root)
        return copy[root]	
  
```
</details>  
	
#### 1490. Clone N-ary Tree
  
<details>
<summary>code</summary>
  
```
def cloneTree(self, root: 'Node') -> 'Node':
	return Node(root.val, [self.cloneTree(child) for child in root.children]) if root else None  
  
```
</details>  
	
#### 1506. Find Root of N-Ary Tree
  
<details>
<summary>code</summary>
  
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""
class Solution:
    def findRoot(self, tree: List['Node']) -> 'Node':
        # set that contains all the child nodes.
        seen = set()

        # add all the child nodes into the set
        for node in tree:
            for child in node.children:
                # we could either add the value or the node itself.
                seen.add(child.val)

        # find the node that is not in the child node set.
        for node in tree:
            if node.val not in seen:
                return node  
  
2.
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""

class Solution:
    def findRoot(self, tree: List['Node']) -> 'Node':
        value_sum = 0

        for node in tree:
            # the value is added as a parent node
            value_sum += node.val
            for child in node.children:
                # the value is deducted as a child node.
                value_sum -= child.val

        # the value of the root node is `value_sum`
        for node in tree:
            if node.val == value_sum:
                return node	
```
</details>  
	
#### 1522. Diameter of N-Ary Tree
  
<details>
<summary>code</summary>
  
```
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""
class Solution:
    def diameter(self, root: 'Node') -> int:
        diameter = 0

        def height(node):
            """ return the height of the node """
            nonlocal diameter

            if len(node.children) == 0:
                return 0

            # select the top two heights
            max_height_1, max_height_2 = 0, 0
            for child in node.children:
                parent_height = height(child) + 1
                if parent_height > max_height_1:
                    max_height_1, max_height_2 = parent_height, max_height_1
                elif parent_height > max_height_2:
                    max_height_2 = parent_height

            # calculate the distance between the two farthest leaves nodes.
            distance = max_height_1 + max_height_2
            diameter = max(diameter, distance)

            return max_height_1

        height(root)
        return diameter  
	
Approach 2: Distance with Depth

"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
"""
class Solution:
    def diameter(self, root: 'Node') -> int:
        """
        :type root: 'Node'
        :rtype: int
        """
        diameter = 0

        def maxDepth(node, curr_depth):
            """ return the maximum depth of leaves nodes
                 descending from the current node
            """
            nonlocal diameter

            if len(node.children) == 0:
                return curr_depth
            
            # select the top 2 depths from its children
            max_depth_1, max_depth_2 = curr_depth, 0
            for child in node.children:
                depth = maxDepth(child, curr_depth+1)
                if depth > max_depth_1:
                    max_depth_1, max_depth_2 = depth, max_depth_1
                elif depth > max_depth_2:
                    max_depth_2 = depth

            # calculate the distance between the two farthest leaves nodes
            distance = max_depth_1 + max_depth_2 - 2 * curr_depth
            diameter = max(diameter, distance)

            return max_depth_1

        maxDepth(root, 0)
        return diameter	
  
```
</details>  
	
#### 1533. Find the Index of the Large Integer
  
<details>
<summary>code</summary>
  
```
class Solution:
    def getIndex(self, rd: 'ArrayReader') -> int:
        l, r = 0, rd.length() - 1
        while l < r:
            h = (r - l + 1) // 2  # half, h * 2 <= r - l + 1
            if rd.compareSub(l, l + h - 1, l + h, l + h * 2 - 1) != 1:
                l = l + h
            else:
                r = l + h - 1
        return l  
  
```
</details>  
	
#### 1538. Guess the Majority in a Hidden Array
  
<details>
<summary>code</summary>
  
```
def guessMajority(self, reader: 'ArrayReader') -> int:
        n,groupA,groupB,aIdx,bIdx=reader.length(),1,0,None,None
        first,second=reader.query(0,1,2,3),reader.query(0,1,2,4)
        for i in range(4,n):
            if reader.query(0,1,2,i)==first:
                groupA,aIdx=groupA+1,i
            else:
                groupB,bIdx=groupB+1,i
        for i in range(3):
            nxt=[v for v in [0,1,2,3,4] if v!=i]
            if reader.query(*nxt)==second:
                groupA,aIdx=groupA+1,i
            else:
                groupB,bIdx=groupB+1,i
        return aIdx if groupA>groupB else bIdx if groupB>groupA else -1  
  
```
</details>  
	
#### 1548. The Most Similar Path in a Graph
  
<details>
<summary>code</summary>
  
```
class Solution:
    def mostSimilar(self, n: int, roads: List[List[int]], names: List[str], tp: List[str]) -> List[int]:
        # construct graph
        graph = [[] for _ in range(n)]
        for u, v in roads:
            graph[u].append(v)
            graph[v].append(u)
        
        # init variables
        m = len(tp)
        dp = [[m] * n for _ in range(m)]
        prev = [[0] * n for _ in range(m)]
        
        # populate dp
        for v in range(n):
            dp[0][v] = (names[v] != tp[0])
        for i in range(1, m):
            for v in range(n):
                for u in graph[v]:
                    if dp[i-1][u] < dp[i][v]:
                        dp[i][v] = dp[i-1][u]
                        prev[i][v] = u
                dp[i][v] += (names[v] != tp[i])
                
        # re-construct path
        path, min_dist = [0], m
        for v in range(n):
            if dp[-1][v] < min_dist:
                min_dist = dp[-1][v]
                path[0] = v
        for i in range(m - 1, 0, -1):
            u = prev[i][path[-1]]
            path.append(u)
            
        return path[::-1]  
  
```
</details>  
	
#### 1564. Put Boxes Into the Warehouse I
  
<details>
<summary>code</summary>
  
```
class Solution:
    def maxBoxesInWarehouse(self, boxes: List[int], warehouse: List[int]) -> int:
        # Preprocess the height of the warehouse rooms to get usable heights
        for i in range(1, len(warehouse)):
            warehouse[i] = min(warehouse[i - 1], warehouse[i])

        # Iterate through boxes from the smallest to the largest
        boxes.sort()

        count = 0

        for room in reversed(warehouse):
            # Count the boxes that can fit in the current warehouse room
            if count < len(boxes) and boxes[count] <= room:
                count += 1

        return count  
	
Approach 2: Add Largest Possible Boxes from Left to Right
class Solution:
    def maxBoxesInWarehouse(self, boxes: List[int], warehouse: List[int]) -> int:

        i = 0
        count = 0
        boxes.sort(reverse = True)

        for room in warehouse:
            # Iterate through boxes from largest to smallest
            # Discard boxes that doesn't fit in the current warehouse
            while i < len(boxes) and boxes[i] > room:
                i += 1
            if i == len(boxes):
                return count
            count += 1
            i += 1

        return count	
  
```
</details>  

