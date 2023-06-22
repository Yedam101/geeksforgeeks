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
### 프로그래머스 방금그곡 
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
### 프로그래머스 방금그곡 모듈화 코드 & 간결한 코드
#### 모듈화
```
def convert_sharp(notes):
    notes = list(notes)
    for i in range(len(notes) - 1):
        if notes[i + 1] == '#':
            notes[i] = notes[i].lower()
            notes[i + 1] = ''
    return ''.join(notes)

def convert_time(time_str):
    m, s = map(int, time_str.split(':'))
    return m * 60 + s

def get_played_notes(play_time, sheet_music):
    sheet_len = len(sheet_music)
    played_len = play_time // sheet_len
    played_rem = play_time % sheet_len
    return sheet_music * played_len + sheet_music[:played_rem]

def solution(m, musicinfos):
    m = convert_sharp(m)
    result = []
    
    for i, musicinfo in enumerate(musicinfos):
        start, end, title, sheet_music = musicinfo.split(',')
        start, end = map(convert_time, [start, end])
        play_time = end - start
        sheet_music = convert_sharp(sheet_music)
        played_notes = get_played_notes(play_time, sheet_music)
        
        if m in played_notes:
            result.append((play_time, i, title))
    
    if not result:
        return "(None)"
    
    result.sort(key=lambda x: (-x[0], x[1]))
    return result[0][2]
```
#### 간결화
```
def convert(time):
    m, s = map(int, time.split(':'))
    return m * 60 + s

def solution(m, musicinfos):
    m = m.replace('A#', 'a').replace('C#', 'c').replace('D#', 'd').replace('F#', 'f').replace('G#', 'g')
    result = []
    for i, musicinfo in enumerate(musicinfos):
        start, end, title, sheet = musicinfo.split(',')
        start, end = map(convert, [start, end])
        sheet = sheet.replace('A#', 'a').replace('C#', 'c').replace('D#', 'd').replace('F#', 'f').replace('G#', 'g')
        play_time = end - start
        played_notes = (sheet * (play_time // len(sheet))) + sheet[:play_time % len(sheet)]
        if m in played_notes:
            result.append((play_time, i, title))
    if not result:
        return "(None)"
    result.sort(key=lambda x: (-x[0], x[1]))
    return result[0][2]
```
### 프로그래머스 오픈채팅방
#### 내 답
```
def solution(record):
    record = [[k] + v.split() for k, v in enumerate(record)]
    dict = {}
    for item in record:
        key = item[2]
        value = item[:2] + item[3:]  
        if key in dict:
            dict[key].append(value)
        else:
            dict[key] = [value]
    result = []
    for k in dict.keys():
        info = dict[k]
        n = -1
        if info[n][1] == 'Leave':
            n -= 1
        for j in info:
            if len(j) == 3:
                j[-1] = info[n][2]
            else:
                j.append(info[n][2])
            result.append(j)
    result = sorted(result)
    answer = []
    for i in result:
        if i[1] == 'Enter':
            temp = f"{i[2]}님이 들어왔습니다."
            answer.append(temp)
        elif i[1] == 'Leave':
            temp = f"{i[2]}님이 나갔습니다."
            answer.append(temp)

    return answer
```
#### 더 좋은 답
```
def solution(record):
    userDict = {}
    answer = []

    for line in record:
        line = line.split(" ")

        if line[0] == "Enter":
            userDict[line[1]] = line[2]
        elif line[0] == "Change":
            userDict[line[1]] = line[2]

    for line in record:
        line = line.split(" ")
        targetString = userDict[line[1]]
        if line[0] == "Enter":
            targetString += "님이 들어왔습니다."
        elif line[0] == "Leave":
            targetString += "님이 나갔습니다."
        else:
            continue
        answer.append(targetString)

    return answer
```

