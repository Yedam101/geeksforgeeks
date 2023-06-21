### 프로그래머스 캐시

```
def solution(cacheSize, cities):
    cities = [city.lower() for city in cities]
    q = ['0']*cacheSize
    count = 0
    while cities:
        temp = cities.pop(0)
        if temp in q:
            count += 1
            q.append(temp)
            q.pop(q.index(temp))
        else:
            count += 5
            q.append(temp)
            q.pop(0)
    return count
```

### 프로그래머스 방금그곡 틀린 코드 아 재생시간 고려 안함;;

```
def solution(m, musicinfos): 
    musicinfos = [i.split(",") for i in musicinfos]
    # m의 '#' -> 소문자로
    left, right, m = 0, 1, list(m)
    while right < len(m):
        if m[right] == '#':
            m[left:right+1] = m[left].lower()
        else:
            left, right = left+1, right+1
    m = ''.join(m)

    
    for i in range(len(musicinfos)):
        # 시간 하나로
        temp = musicinfos[i].pop(0)
        temp = int(temp[:2])*60+int(temp[3:5])
        musicinfos[i][0] = (int(musicinfos[i][0][:2])*60+int(musicinfos[i][0][3:5])) - temp
        
        # 악보 '#' -> 소문자로
        left, right, mscf = 0, 1, list(musicinfos[i][2])
        while right < len(mscf):
            if mscf[right] == '#':
                mscf[left:right+1] = mscf[left].lower()
            else:
                left, right = left+1, right+1
        musicinfos[i][2] = ''.join(mscf)
    
    result = []
    for i in range(len(musicinfos)):
        mscf = musicinfos[i][2]
        short = min(len(m), len(mscf))
        long = max(len(m), len(mscf))
        for j in range(long):
            if m[:short] in mscf[:short]:
                result.append(musicinfos[i])
                break
            else:
                mscf = mscf[1:] + mscf[0]
        
    if not result:
        return "(None)"
    result = sorted(result, key=lambda x:-x[0])               
    return result[0][1]

```

### 프로그래머스 방금그곡 수정해서 정답
```
def solution(m, musicinfos): 
    musicinfos = [i.split(",") for i in musicinfos]
    left, right, m = 0, 1, list(m)
    while right < len(m):
        if m[right] == '#':
            m[left:right+1] = m[left].lower()
        else:
            left, right = left+1, right+1
    m = ''.join(m)

    
    for i in range(len(musicinfos)):
        temp = musicinfos[i].pop(0)
        temp = int(temp[:2])*60+int(temp[3:5])
        musicinfos[i][0] = (int(musicinfos[i][0][:2])*60+int(musicinfos[i][0][3:5])) - temp
        
        left, right, mscf = 0, 1, list(musicinfos[i][2])
        while right < len(mscf):
            if mscf[right] == '#':
                mscf[left:right+1] = mscf[left].lower()
            else:
                left, right = left+1, right+1
        mscf = ''.join(mscf)
        
        if musicinfos[i][0] > len(mscf):
            mscf = mscf*(musicinfos[i][0] // len(mscf)) + mscf[:(musicinfos[i][0] % len(mscf))]
        else:
            mscf = mscf[:musicinfos[i][0]]
        musicinfos[i][2] = mscf
        musicinfos[i].append(i)
    
    result = []
    
    for i in range(len(musicinfos)):
        if m in musicinfos[i][2]:
            result.append(musicinfos[i])
    if not result:
        return "(None)"
    result = sorted(result, key=lambda x:(-x[0],x[-1]))
    return result[0][1]

```