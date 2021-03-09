# 알고리즘 문제풀이



## 1. 행렬찾기

> 행렬 안에 있는 0이 아닌 숫자들의 모음으로 되어있는 사각형을 찾는 프로그램

🙄 푸는 방법

1. 행렬을 순회하면서 0이 아닌 숫자를 찾는다
2. 찾은 그 자리에서 행과 열을 카운팅하여 행렬의 길이를 구한다
3. 다 구했으면 찾은 사각형의 행렬을 0으로CLEAR한다
4. 원래 자리에서 다시 0이 아닌 숫자를 찾는다.

```python
# 사각형의 크기를 찾는 함수
def search_size(r, c):
    r_cnt, c_cnt = 0, 0
    
    #행의 길이 찾는 순회
    for i in range(r, N):
        if arr[i][c] != 0:
            r_cnt += 1
        else: break
            
    #행의 길이 찾는 순회
    for i in range(c, N):
        if arr[i][c] != 0:
            c_cnt += 1
        else: break
        
	ans.append([r_cnt, c_cnt, r_cnt*c_cnt])
    init(r, c, r_cnt, c_cnt)


# 화학물질을 빈용기로 변환
def init(r, c, r_cnt, c_cnt):
    for i in range(r, r+r_cnt):
        for j in range(c, c+c_cnt):
            arr[i][j] = 0
            
# 정렬
def counting_sort(idx):
    cnt = [0]*1000
    sort_ans = [o]*len(ans) # ans는 [행 크기, 열 크기, 행렬의 크기] 로 구성됨
    
    #카운팅 하는 과정(카운팅 정렬)
    for i in range(len(ans)):
        cnt[ans[i][idx]] += 1
        
    #누적
    for i in range(1, len(cnt)):
        cnt[i] += cnt[i-1]
        
    # 정렬하여 넣는 과정
    for i in range(len(ans)-1, -1, -1):
        sort_ans[cnt[ans[i][idx]-1]] = ans[i]
        cnt[ans[i][idx]]-=1
	
    return sort_ans

# 메인
ans = []

# 행우선순회 방식으로 순회하면서 사각형의 좌표를 구함
for i in range(N):
    for j in range(N):
        if arr[i][j] != 0:
            search_size(i,j)
            
ans = counting_sort(0) #행을 기준으로 정렬
ans = counting_sort(2) #열의 크기로 다시한번 정렬
```





## 2. 러시아 깃발

> 러시아 국기 같은 깃발을 만들기 위해서 새로 칠해야 하는 칸의 개수의 최솟값

🙄 푸는 방법

1. 흰 - 파 - 빨로 순서가 정해져있음
2. 중복 순열 이용

```python
# N:행 M:열
# flag : 깃발정보

def perm(idx, sub_sum):
    global ans
    
    #유망성 검사(가지치기)
    if sub_sum > N:
        return
    if idx == 3:
        if sub_sum == N:
            cnt =0
            st =sel[0]
            st2=st+sel[1]
            
            for i in range[:st]:
            	for j in i:
                    if j != 'W': cnt+=1
                        
            for i in range[st:st2]:
                for j in i:
                    if j != 'B': cnt+=1
                        
            for i in range[st2:]:
                for j in i:
                    if j != 'R': cnt+=1
            ans = cnt            
		
    
    for i in range(1, N-1): #각각 한줄씩 확보를 해야하므로
        sel[idx] = i
        perm(idx+1, sub_sum+i)


# 메인
sel = [0]*3
ans = 987654321

# 앞에는 idx, 뒤에는 중간합
perm(0, 0)
```

```python
w = [0]*N
b = [0]*N
r = [0]*N

for i in range(N):
    for j in range(M):
        if flag[i][j] != 'W':
            w[i] += 1
        if flag[i][j] != 'B':
            b[i] += 1
        if flag[i][j] != 'R':
            r[i] += 1

#누적시키기            
for i in range(1, N):
    w[i] += w[i-1]
    b[i] += b[i-1]
    r[i] += r[i-1]
    
    
ans = 987654321

for i in range(N-2):
    for j in range(i+1, N-1):
        w_cnt = w[i]
        b_cnt = b[j] - b[i]
        r_cnt = r[N-1]-r[j]
        
        if ans > w_cnt+b_cnt+r_cnt:
            ans = w_cnt+b_cnt+r_cnt
```



## 3. 조합

```python
# 4C3
n=len(a)

for i in range(0, n-2):
    for j in range(i+1, n-1):
        for k in range(j+1, n):
            print(a[i], a[j], a[k])
```

