### 프로그래머스 2 x n 타일링

```
def solution(n):
    nums = [0] * (n+1)
    nums[1] = 1
    nums[2] = 2
    for i in range(3, n+1):
        nums[i] = (nums[i-1] + nums[i-2]) % 1000000007
        
    return nums[n]
```

### 프로그래머스 타겟 넘버

```
# global 

answer = 0

def solution(num, target):
    
    def dfs(idx, result):
        if idx == len(num):
            if result == target:
                global answer
                answer += 1
                return answer
        else:
            dfs(idx+1, result+num[idx])
            dfs(idx+1, result-num[idx])
        
    dfs(0,0)
    return answer
    
# nonlocal

def solution(num, target):
    answer = 0
    def dfs(idx, result):
        if idx == len(num):
            if result == target:
                nonlocal answer
                answer += 1
                return answer
        else:
            dfs(idx+1, result+num[idx])
            dfs(idx+1, result-num[idx])
        
    dfs(0,0)
    return answer
```

### 프로그래머스 땅따먹기

```
def solution(land):
    for i in range(len(land)-1):
        for j in range(4):
            m = land[i][0:j] + land[i][j+1:]
            land[i+1][j] += max(m)
    
    return max(land[-1])
```

### 프로그래머스 멀리 뛰기

```
def solution(n):
    if n >= 3:
        dp = [0] * (n+1)
        dp[1] = 1
        dp[2] = 2
        for i in range(3, n+1):
            dp[i] = (dp[i-1] + dp[i-2]) % 1234567

    else:
        dp = [0,1,2]
    return dp[n]
```

### 프로그래머스 숫자의 표현

```
def solution(n):
    count = 1
    nums = [i for i in range(1,n+1)]
    if n == 1:
        return count 

    left = 0
    right = 1
    while left < right and right < n-1:
        if sum(nums[left:right+1]) < n:
            right += 1
        elif sum(nums[left:right+1]) > n:
            left += 1
        else:
            count += 1
            left += 1
            right += 1

    return count
```

### 최댓값과 최솟값

```
def solution(s):
    s = list(map(int, s.split()))
    mm = []
    mm.append(str(min(s)))
    mm.append(str(max(s)))
    return ' '.join(m for m in mm)
```
#### 다른 답
```
def solution(s):
    s = list(map(int,s.split()))
    return str(min(s)) + " " + str(max(s))
```

### 최솟값 만들기

```
def solution(A,B):
    a = sorted(A)
    b = sorted(B, reverse=True)
    ab = list(zip(a,b))
    mulab = [i*j for i, j in ab]
    return sum(mulab)
```

### JadenCase 문자열 만들기

```
def solution(s):
    point = 0
    while point < len(s):
        if point == 0 or not s[point-1].isalnum():
            s = s[:point] + s[point].upper() + s[point+1:]
        else:
            s = s[:point] + s[point].lower() + s[point+1:]

        point += 1
    return 
```

### 전화번호 목록

```
def solution(p):
    p = sorted(p)
    for i in range(len(p)-1):
        if p[i] == p[i+1][:len(p[i])]:
            return False
    return True
```

### 의상

```
from collections import defaultdict
def solution(clothes):
    dict = defaultdict(list)
    
    for i in range(len(clothes)):
        if dict[clothes[i][1]] == []:
            dict[clothes[i][1]] = [clothes[i][0]]

        else:
            dict[clothes[i][1]].append(clothes[i][0])
    val = dict.values()
    count = [len(i)+1 for i in val]
    result = 1
    for i in count:
        result *= i
    return result -1
```

### 다리를 지나는 트럭

```
def solution(bridge_length, weight, truck_weights):
    sec = 1
    q = []
    q.append([truck_weights.pop(0),sec])


    while q:
        sec += 1
        if q[0][1] + bridge_length == sec:
            q.pop(0)
        if truck_weights and sum([i[0] for i in q]) + truck_weights[0] <= weight and len(q) < bridge_length:
            q.append([truck_weights.pop(0), sec])
    return sec
```