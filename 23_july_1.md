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