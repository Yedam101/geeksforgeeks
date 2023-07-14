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

### 프로그래머스 광물 캐기
#### 내 답 - 길고 복잡
```
import math
def solution(picks, minerals):
            
    tnum = sum(picks)
    f = math.ceil(len(minerals) / 5)
    td, ti, ts = picks
    
    if len(minerals) > tnum*5:
        minerals = minerals[:tnum*5]
    else:
        if len(minerals) != f*5:
            for i in range(f*5-len(minerals)):
                minerals.append('zero')
                
    dict = {'diamond':25, 'iron':5, 'stone':1, 'zero':0}
    weight = []
    
    for i in range(len(minerals)//5):
        tmp = minerals[5*i:5*(i+1)]
        count = 0
        for j in tmp:
            count += dict[j]
        weight.append([count, i])
    weight = sorted(weight, key = lambda x:-x[0])

    for i in range(len(weight)):
        if td != 0:
            weight[i].append('D')
            td -= 1
        elif ti != 0:
            weight[i].append('I')
            ti -= 1
        else:
            weight[i].append('S')
            ts -= 1
    
    weight = sorted(weight, key = lambda x:x[1])
    result = 0

    for i in range(len(weight)):
        tmp = minerals[5*i:5*(i+1)]
        if weight[i][2] == 'D':
            tmp_dict = {"diamond":1, 'iron':1, 'stone':1, 'zero':0}
        elif weight[i][2] == 'I':
            tmp_dict = {"diamond":5, 'iron':1, 'stone':1, 'zero':0}
        else:
            tmp_dict = {"diamond":25, 'iron':5, 'stone':1, 'zero':0}
        for j in tmp:
            result += tmp_dict[j]
    return result
```
#### 남의 답
```
def solution(picks, minerals):
    def solve(picks, minerals, fatigue):
        if sum(picks) == 0 or len(minerals) == 0:
            return fatigue
        result = [float('inf')]
        for i, fatigues in enumerate(({"diamond": 1, "iron": 1, "stone": 1},
                                      {"diamond": 5, "iron": 1, "stone": 1},
                                      {"diamond": 25, "iron": 5, "stone": 1},)):
            if picks[i] > 0:
                temp_picks = picks.copy()
                temp_picks[i] -= 1
                result.append(
                    solve(temp_picks, minerals[5:], fatigue + sum(fatigues[mineral] for mineral in minerals[:5])))
        return min(result)

    return solve(picks, minerals, 0)
```

### 프로그래머스 리코쳇 로봇
#### 내 답

```
from collections import deque
def solution(board):
    board = [list(i) for i in board]
    m, n = len(board), len(board[0])
    dx = [0,0,1,-1]
    dy = [1,-1,0,0]
    
    start, end = None, None
    for i in range(m):
        for j in range(n):
            if board[i][j] == 'R':
                start = (i, j)
            elif board[i][j] == 'G':
                end = (i, j)
    
    def bfs(start):
        q = deque([start])
        visited = [[False]*n for _ in range(m)]
        visited[start[0]][start[1]] = True
        count = [[0]*n for _ in range(m)]
        
        while q:
            x, y = q.popleft()
            for i in range(4):
                nx, ny = x, y
                while True:
                    nx += dx[i]
                    ny += dy[i]
                    if nx < 0 or nx >= m or ny < 0 or ny >= n or board[nx][ny] == 'D':
                        nx -= dx[i]
                        ny -= dy[i]
                        break
                if visited[nx][ny] == False:
                    visited[nx][ny] = True
                    count[nx][ny] = count[x][y] + 1
                    q.append((nx, ny))
                    
        return count[end[0]][end[1]] if visited[end[0]][end[1]] else -1

    return bfs(start)
```
#### 좀 더 간결한 남의 답
```
def solution(board):
    que = []
    for x, row in enumerate(board):
        for y, each in enumerate(row):
            if board[x][y] == 'R':
                que.append((x, y, 0))
    visited = set()
    while que:
        x, y, length = que.pop(0)
        if (x, y) in visited:
            continue
        if board[x][y] == 'G':
            return length
        visited.add((x, y))
        for diff_x, diff_y in ((0, 1), (0, -1), (1, 0), (-1, 0)):
            now_x, now_y = x, y
            while True:
                next_x, next_y = now_x + diff_x, now_y + diff_y
                if 0 <= next_x < len(board) and 0 <= next_y < len(board[0]) and board[next_x][next_y] != 'D':
                    now_x, now_y = next_x, next_y
                    continue
                que.append((now_x, now_y, length + 1))
                break
    return -1
```