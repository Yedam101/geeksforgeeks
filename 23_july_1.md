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

### 프로그래머스 최댓값과 최솟값

```
def solution(s):
    s = list(map(int, s.split()))
    mm = []
    mm.append(str(min(s)))
    mm.append(str(max(s)))
    return ' '.join(m for m in mm)
```
#### 프로그래머스 다른 답
```
def solution(s):
    s = list(map(int,s.split()))
    return str(min(s)) + " " + str(max(s))
```

### 프로그래머스 최솟값 만들기

```
def solution(A,B):
    a = sorted(A)
    b = sorted(B, reverse=True)
    ab = list(zip(a,b))
    mulab = [i*j for i, j in ab]
    return sum(mulab)
```

### 프로그래머스 JadenCase 문자열 만들기

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

### 프로그래머스 전화번호 목록

```
def solution(p):
    p = sorted(p)
    for i in range(len(p)-1):
        if p[i] == p[i+1][:len(p[i])]:
            return False
    return True
```

### 프로그래머스 의상

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

### 프로그래머스 다리를 지나는 트럭

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

### 프로그래머스 주식가격
#### 효율성 통과 못 함
```
def solution(prices):
    stack = []
    psec = []
    result = []
    for s, v in enumerate(prices):
        psec.append([v,s+1])
            
    
    while psec:
        if not stack:
            stack.append(psec.pop(0))
        elif psec[0][0] >= stack[-1][0]:
            stack.append(psec.pop(0))
        elif psec[0][0] < stack[-1][0]:
            i, j = stack.pop()
            k = psec[0][1] - j
            result.append([i,j,k])
    re = []
    for i in stack:
        j, k = i
        p = len(prices) - k
        re.append([j,k,p])
    
    result = sorted(re + result, key = lambda x:x[1])
    
    return [k for i,j,k in result]
```

#### 통과함

```
def solution(prices):
    answer = [0] * len(prices)
    stack = []

    for i, price in enumerate(prices):
        while stack and prices[stack[-1]] > price:
            j = stack.pop()
            answer[j] = i - j
        stack.append(i)

    while stack:
        j = stack.pop()
        answer[j] = len(prices) - 1 - j

    return answer
```

#### 다른 사람 풀이

```
def solution(prices):
    answer = [0] * len(prices)
    for i in range(len(prices)):
        for j in range(i+1, len(prices)):
            if prices[i] <= prices[j]:
                answer[i] += 1
            else:
                answer[i] += 1
                break
    return answer
```

### 125. Valid Palindrome

```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        S = ''
        for i in s:
            if i.isalnum():
                S += i.lower()

        left, right = 0, len(S) -1

        while left < right:
            if S[left] == S[right]:
                left += 1
                right -= 1
            else:
                return False
        return True 
```

### 937. Reorder Data in Log Files

```
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        al = []
        di = []

        for i in range(len(logs)):
            if logs[i][-1].isalpha():
                al.append(logs[i])
            else:
                di.append(logs[i])
        al = [i.split() for i in al]
        al = sorted(al, key = lambda x : (x[1:], x[0]))
        al = [" ".join(i) for i in al]
        return al + di
```

### 819. Most Common Word
#### 정규식 사용
```
from collections import Counter
import re
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        par = re.sub(r'[^a-zA-Z0-9]', ' ', paragraph).lower()
        par = par.split(" ")
        par = Counter(par).most_common()
        for i in par:
            if i[0] not in banned and i[0] != "":
                return i[0]
```
#### 정규식 사용 안함
```
from collections import Counter
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        p = ""
        for i in paragraph:
            if i.isalnum():
                p += i.lower()
            else:
                p += " "
        par = p.split(" ")
        par = Counter(par).most_common()
        for i in par:
            if i[0] not in banned and i[0] != "":
                return i[0]
```

### 49. Group Anagrams

```
from collections import defaultdict
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        dict = defaultdict(list)

        for i in strs:
            if ''.join(sorted(i)) in dict:
                dict[''.join(sorted(i))].append(i)
            else:
                dict[''.join(sorted(i))] = [i]

        v = list(dict.values())
        return v
```