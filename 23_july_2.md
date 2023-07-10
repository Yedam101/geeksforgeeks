### 17. Letter Combinations of a Phone Number

```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        dic = {"2": "abc", "3": "def", "4": "ghi", "5": "jkl",
        "6" : "mno", "7" : "pqrs", "8" : "tuv", "9" : "wxyz"}

        result = []

        def dfs(index, path):
            if len(path) == len(digits):
                result.append(path)
                return
            for i in range(index, len(digits)):
                for j in dic[digits[index]]:
                    dfs(i+1, path+j)
        
        dfs(0, "")
        if digits == "":
            return []
        return result
```

### 46. Permutations

```
from itertools import permutations
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        nums = [list(i) for i in permutations(nums,len(nums))]
        return nums
```

### 39. Combination Sum

```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []

        def dfs(csum, index, path):
            if csum < 0:
                return
            if csum == 0:
                result.append(path)
                return
            for i in range(index, len(candidates)):
                dfs(csum-candidates[i], i, path+[candidates[i]])
            
        dfs(target, 0, [])
        return result
```

### 78. Subsets

```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []

        def dfs(index, path):
            result.append(path)
            for i in range(index, len(nums)):
                dfs(i+1, path+[nums[i]])
        dfs(0, [])
        return result
```

### 프로그래머스 요격 시스템
#### 시간초과
```
from collections import deque

def solution(targets):
    targets = sorted(targets, key=lambda x : x[0])
    targets = deque(targets)
    stack = []

    while targets:
        if not stack:
            stack.append(targets.popleft())
        if stack[-1][1] > targets[0][0]:
            if stack[-1][1] > targets[0][1]:
                stack.pop()
                stack.append(targets.popleft())
            else:
                targets.popleft()
        elif stack[-1][1] <= targets[0][0]:
            stack.append(targets.popleft())
    return len(stack)
```
#### 시간 초과 해결
```
def solution(targets):
    answer = 0
    targets.sort(key=lambda x: x[1])
    temp = 0
    for i in targets:
        if i[0]<temp:
            continue
        else:
            answer+=1
            temp = i[1]

    return answer
```