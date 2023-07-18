
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

### 프로그래머스 호텔 대실
#### 내 답. 우선순위 큐 사용
```
import heapq

def solution(book_time):
    book = []
    for s, e in book_time:
        s = int(s[:2])*60 + int(s[3:])
        e = int(e[:2])*60 + int(e[3:]) + 10
        book.append([s, e])
    book.sort()

    rooms = []
    for s, e in book:
        if rooms and rooms[0] <= s:
            heapq.heappop(rooms)
        heapq.heappush(rooms, e)

    return len(rooms)
```
#### 다른 답 누적합
```
def solution(book_time):
    time_table = [0 for _ in range(60 * 24)]
    for start, end in book_time:
        start_minutes = 60 * int(start[:2]) + int(start[3:])
        end_minutes = 60 * int(end[:2]) + int(end[3:]) + 10

        if end_minutes > 60 * 24 - 1:
            end_minutes = 60 * 24 - 1

        for i in range(start_minutes, end_minutes):
            time_table[i] += 1
    return max(time_table)
```