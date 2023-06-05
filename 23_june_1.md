### 704. Binary Search

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)-1
        while left < right:
            mid = (left+right)//2
            if nums[mid] >= target:
                right = mid
            else:
                left = mid + 1
        if nums[left] != target:
            return -1
        return left
```

### 74. Search a 2D Matrix

```
class Solution:
    def searchMatrix(self, m: List[List[int]], target: int) -> bool:
        m = sum(m,[])
        if target in m:
            return True
        else:
            return False
```

### 875. Koko Eating Bananas

```
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        start, end = 1, max(piles)
        while start < end:
            mid = (start + end) // 2
            check = 0
            for i in piles:
                check += ((i-1)//mid)+1
            if check > h:
                start = mid+1
            else:
                end = mid
        return start
```