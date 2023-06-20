### 226. Invert Binary Tree

```
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
                return
        self.invertTree(root.left) 
        self.invertTree(root.right)  
        root.left, root.right = root.right, root.left
        return root
```

### 104. Maximum Depth of Binary Tree

```
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # if not root:
        #     return 0
        # return 1 + max(self.maxDepth(root.right), self.maxDepth(root.left))

        def helper(root):
            if not root:
                return 0
            r = helper(root.right)
            l = helper(root.left)
            return 1 + max(r, l)
        return helper(root)
```

### 543. Diameter of Binary Tree

```
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.maxi = 0
        def helper(root):
            if not root:
                return 0
            
            lh = helper(root.left)
            rh = helper(root.right)
        
            self.maxi = max(self.maxi, lh + rh)

            return 1 + max(lh, rh)
        
        helper(root)
        return self.maxi
```

### 100. Same Tree

```
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:

        def rec(p,q):
            if not p and not q:
                return True
            if (not p and q) or (not q and p):
                return False
            if p.val != q.val:
                return False
            return rec(p.left, q.left) and rec(p.right, q.right)
        
        return rec(p,q) 
```

### 프로그래머스 - 개인정보 수집 유효기간

```
def solution(today, terms, privacies):
    answer = []
    today = int(today[:4])*336+(int(today[5:7])-1)*28+int(today[8:])
    
    priv = [i.split() for i in privacies]
    priv = [[int(i[0][:4])*336+(int(i[0][5:7])-1)*28+int(i[0][8:]), i[1]] for i in priv]
    
    termd = {}
    for i in terms:
        termd[i[0]] = int(i[2:])*28

    for i in range(len(priv)):
        date = priv[i][0]+termd[priv[i][1]]
        if date <= today:
            answer.append(i+1)
    return answer
```

### 200. Number of Islands

```
# class Solution:
#     def numIslands(self, grid: List[List[str]]) -> int:
#         m, n = len(grid), len(grid[0])
#         check = [[False]*n for i in range(m)]
        
#         # 섬의 개수
#         count = 0
#         dx = [0, 0, 1, -1]
#         dy = [1, -1, 0, 0]

#         def dfs(x, y):
#             for i in range(4):
#                 nx = x + dx[i]
#                 ny = y + dy[i]
#                 if m <= nx or 0 > nx or n <= ny or 0 > ny:
#                     return
#                 elif grid[nx][ny] == '1' and check[nx][ny] == False:
#                     check[nx][ny] = True
#                     dfs(nx, ny)
#             return  

#         for i in range(m):
#             for j in range(n):
#                 if grid[i][j] == '1' and check[i][j] == False:
#                     check[i][j] = True
#                     count += 1
#                     dfs(i, j)
#         return count

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m = len(grid)
        if m == 0:  # Handle the case when grid is empty
            return 0
        n = len(grid[0])
        
        check = [[False]*n for _ in range(m)]
        count = 0
        dx = [0, 0, 1, -1]
        dy = [1, -1, 0, 0]

        def dfs(x, y):
            if grid[x][y] == '0' or check[x][y]:
                return
            check[x][y] = True
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                if 0 <= nx < m and 0 <= ny < n:
                    dfs(nx, ny)
            
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1' and not check[i][j]:
                    dfs(i, j)
                    count += 1

        return count
```

### 695. Max Area of Island

```
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m = len(grid)
        if m == 0:
            return 0
        n = len(grid[0])
        check = [[False]*n for _ in range(m)]
        
        zmax = 0
        dx = [0,0,1,-1]
        dy = [1,-1,0,0]
        def dfs(x, y):
            # 종료조건 현재 0 이거나 체크가 참일때
            if grid[x][y] == 0 or check[x][y]:
                return 0
            check[x][y] = True
            count = 1
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                if 0 <= nx < m and 0 <= ny < n:
                    count += dfs(nx, ny)
            return count

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1 and not check[i][j]:
                    xmax = dfs(i, j)
                    zmax = max(zmax, xmax)
        return zmax
```

### 130. Surrounded Regions

```
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if not board:
            return []
        m, n = len(board), len(board[0])
        check = [[False]*n for _ in range(m)]

        dx = [0,0,1,-1]
        dy = [1,-1,0,0]
        def dfs(x, y):
            if board[x][y] != "O" or check[x][y]:
                return
            check[x][y] = True

            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                if 0 <= nx < m and 0 <= ny < n:
                    dfs(nx, ny)

        for i in range(m):
            for j in range(n):
                if i == 0 or i == m-1 or j == 0 or j == n-1:
                    dfs(i, j)

        for i in range(m):
            for j in range(n):
                if not check[i][j] and board[i][j] == 'O':
                    board[i][j] = "X"
```

### 프로그래머스 배달

```
import heapq

# def solution(N, road, K):
#     # pre
#     q = []
#     start = 1
#     dist = [10001]*(N+1)
#     graph = [[] for _ in range(N+1)]
#     for i in range(N):
#         u, v, k = road[i]
#         graph[u].append((k,v))
#         graph[v].append((k,u))
    
#     # digk
#     heapq.heappush(q,(0,start))
#     while q:
#         d, node = heapq.heappop(q)
#         if dist[node] < d:
#             continue
#         for i in graph[node]:
#             cost = d + i[0]
#             if cost < dist[i[1]]:
#                 dist[i[1]] = cost
#                 heapq.heappush(q,(cost,i[1]))
    
#     count = 0
#     for i in dist:
#         if i <= K:
#             count += 1
    
#     return count


import heapq

def dijk(dist, adj):
    q = []
    heapq.heappush(q, [0,1])
    while q:
        cost, node = heapq.heappop(q)
        for c, n in adj[node]:
            if cost+c < dist[n]:
                dist[n] = cost+c
                heapq.heappush(q, [cost+c, n])

def solution(N, road, K):
    dist = [float('inf')]*(N+1)
    dist[1] = 0
    adj = [[] for _ in range(N+1)]
    for i in road:
        u, v, k = i
        adj[u].append((k,v))
        adj[v].append((k,u))
        
    dijk(dist,adj)
    count = 0
    for i in dist:
        if i <= K:
            count += 1
    return count

```