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

### 40. Combination Sum II

```
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        candidates.sort()
        def dfs(cur, idx, path):
            if cur > target: return
            if cur == target:
                result.append(path)
                return
            for i in range(idx, len(candidates)):
                if i > idx and candidates[i] == candidates[i-1]:
                    continue
                dfs(cur+candidates[i], i+1, path+[candidates[i]])
        dfs(0, 0, [])
        return result
```

### 743. Network Delay Time

```
from collections import defaultdict
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # u now, v destination, w dist

        dict = defaultdict(list)
        for u,v,w in times:
            dict[u].append([w,v])

        q = []
        dist = [100000001]*(n+1)
        dist[k] = 0
        heapq.heappush(q,(0,k))

        while q:
            d, n = heapq.heappop(q)
            if dist[n] < d:  # 이 if 문 안 넣어도 문제없이 돌아가지만 넣으면 속도 단축
                continue
            for i in dict[n]:
                cost = d + i[0]
                if cost < dist[i[1]]:
                    dist[i[1]] = cost
                    heapq.heappush(q, (cost, i[1]))

        dist = dist[1:]
        if max(dist) == 100000001:
            return -1
        else:
            return max(dist)
```

### 787. Cheapest Flights Within K Stops

```
from collections import defaultdict
import heapq

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        dict = defaultdict(list)
        for u, v, w in flights:
            dict[u].append((v, w))

        q = [(0, src, K + 1)] 
        dist = {}  

        while q:
            d, node, k = heapq.heappop(q)
            if k < 0:
                continue
            if node == dst:
                return d
            if node not in dist or k > dist[node]:
                dist[node] = k
                for v, w in dict[node]:
                    cost = d + w
                    heapq.heappush(q, (cost, v, k - 1))
        return -1 if dst not in dist else dist[dst]
```

### 104. Maximum Depth of Binary Tree
#### BFS 풀이
```
from collections import deque
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0 
        q = deque([root])
        depth = 0

        while q:
            depth += 1
            for _ in range(len(q)):
                cur = q.popleft()
                if cur.left:
                    q.append(cur.left)
                if cur.right:
                    q.append(cur.right)
        return depth
```
### 재귀 풀이
```
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def recur(root):
            if not root:
                return 0
            r = helper(root.right)
            l = helper(root.left)
            return 1 + max(r, l)
        return recur(root)
```

### 687. Longest Univalue Path

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    mxlen = 0
    def longestUnivaluePath(self, root: Optional[TreeNode]) -> int:

        def recur(root):
            if root is None:
                return 0

            l = recur(root.left)
            r = recur(root.right)

            if root.left and root.val == root.left.val:
                l += 1
            else:
                l = 0
            if root.right and root.val == root.right.val:
                r += 1
            else:
                r = 0

            self.mxlen = max(self.mxlen, l+r)
            return max(l, r)
        
        recur(root)
        return self.mxlen
```