
### 프로그래머스 미로 탈출
#### 내 답

```
from collections import deque

def solution(maps):
    maps = [list(i) for i in maps]
    m, n = len(maps), len(maps[0])
    dx = [0,0,1,-1]
    dy = [1,-1,0,0]
    
    def bfs(x, y, to):
        q = deque([(x, y, 0)])  
        check = [[False]*n for _ in range(m)]
        check[x][y] = True
        
        while q:
            x, y, count = q.popleft()
            if maps[x][y] == to:
                return x, y, count
            
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                
                if 0 > nx or m <= nx or 0 > ny or n <= ny:
                    continue
                if maps[nx][ny] != 'X' and not check[nx][ny]:
                    check[nx][ny] = True
                    q.append((nx, ny, count+1))
        return x, y, -1

    
    for i in range(m):
        for j in range(n):
            if maps[i][j] == 'S':
                x, y, StoL = bfs(i, j, 'L')
                if StoL == -1:
                    return -1
                _, _, LtoE = bfs(x, y, 'E')
                if LtoE == -1:
                    return -1
                return StoL + LtoE
```
#### 목표지점 먼저 찾기
```
from collections import deque

def solution(maps):
    maps = [list(i) for i in maps]
    m, n = len(maps), len(maps[0])
    dx = [0,0,1,-1]
    dy = [1,-1,0,0]
    
    def bfs(x, y, end):
        q = deque([(x, y, 0)])  
        check = [[False]*n for _ in range(m)]
        check[x][y] = True

        while q:
            x, y, count = q.popleft()
            if (x, y) == end:
                return count

            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                if 0 > nx or m <= nx or 0 > ny or n <= ny:
                    continue
                if maps[nx][ny] != 'X' and not check[nx][ny]:
                    check[nx][ny] = True
                    q.append((nx, ny, count+1))
        return -1

    # 시작점(S), 레버(L), 출구(E) 위치 찾기
    for i in range(m):
        for j in range(n):
            if maps[i][j] == 'S':
                S = (i, j)
            elif maps[i][j] == 'L':
                L = (i, j)
            elif maps[i][j] == 'E':
                E = (i, j)

    StoL = bfs(*S, L)
    if StoL == -1:
        return -1
    LtoE = bfs(*L, E)
    if LtoE == -1:
        return -1
    return StoL + LtoE
```