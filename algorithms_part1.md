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

