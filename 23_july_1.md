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