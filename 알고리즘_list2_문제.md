# list2 실습

> 색칠하기, 부분집합의 합 , 이진탐색 , 특별한 정렬



## 1. 색칠하기

> 빨강과 파랑의 색이 칠해진 사각형에서 겹쳐진 부분을 구하여라(배열)
>
> ex) 2 2 3 4 1 => 좌측 상단 꼭짓점 (2,2) 우측 하단 꼭짓점 (3,4) 색(1: 빨강 2:파랑)

🤷‍♀️ 어떻게 풀어야하나?

1.  0으로 전체 배열 초기화
2.  빨강색을 +1로 채우기
3.  후에 파란색에 `+2`을 하여 채우기 🔥 무조건 +1하지 않아도 더하는 숫자를 다르게하면 더 쉬움!
4.  전체에서 3으로 채워진 칸수 카운팅 

😥 같은 색인 영역은 겹치지 않는다는 제약조건!! ==> 문제 자세히 읽기!!

```python
arr = [[0]*10 for _ in range(10)] # (1)
cnt=0
N=int(input()) # 색칠 횟수

for _ in range(N): # (2), (3)
    r1, c1, r2, c2, color = map(int, input().split())
    for i in range(r1, r2+1):
        for j in range(c1, c2+1):   
            arr[i][j] += color
            
for i in range(10): # (4)
    for j in range(10):
        if arr[i][j] ==3: cnt+=1
```



## 2. 부분집합의 합

> 부분집합 중 N개의 원소를 갖고 있으며 원소의 합이 K인 부분집합의 개수를 출력하시오.

🤷‍♀️ 어떻게 풀어야하나?

1.  부분집합 생성
2.  부분집합과 n(12)과 비교하기
3.  sum과 cnt
4. 맞는 확인

```python
# 완전 검색
arr = list(range(1, 13))
n=12
N, K # N : 원소 수 , K : 원소들의 합
ans = 0

for i in range(1, 1<<n): # 공집합 제외 부분집합
    sum_val=0
    cnt=0
    for j in range(n):
        if i & (1<<j):
            sum+=arr[j]
            cnt+=1
    if sum_val==K and cnt==N:
        ans+=1
```



## 3. 이진탐색

🤷‍♀️ 어떻게 풀어야하나?

1. 이진탐색 함수를 만든다.
2. A, B 를 비교하는 if 문 만든다.

```python
while start <= end:
    middle = (start+end)//2
    cnt+=1 # cnt의 위치를 잘 설정하면 중복으로 사용 안해도 됨
    if key == middle: # 성공
        return cnt
    elif key < middle: # 찾는 값이 좌측에 있을 때
        end = middle
    else:
        statrt = middle # 찾는 값이 우측에 있을 때
```



## 4. 정렬

> 큰수 작은수 큰수 작은수....

🤷‍♀️ 어떻게 풀어야하나?

1. 선택 정렬 사용

2. if 구문 i%2로 구별 ==> idx가 짝수일 때 최대, 홀수일때 최소

   ```python
   def selection(a):
       for i in range(N-1):
           idx=i # 최대값 또는 최소값의 인덱스
           #최대값
           if not i%2:
               for j in range(i+1, N):
                   if a[idx] < arr[j]:
                       idx = j
                   a[i], a[idx] = a[idx], a[i]
           #최소값
           else:
               for j in range(i+1, N):
                   if a[idx] > arr[j]:
                       idx = j
                   a[i], a[idx] = a[idx], a[i]
   ```

   

## 5. GNS

> ZRO, ONE, TWO, THR, FOR, FIV, SIX, SVN, EGT, NIN 으로 구성된 배열을 오른차순으로 정렬하여라.

🤷‍♀️ 어떻게 풀어야하나?

1. 카운팅사용
2. 0~9의 배열을 만든다.
3. 입력 배열을 순회하면서 카운팅한다.
4. 카운팅에 맞게 배열 정렬

```python
tmp=input() # '#1 7041'이런건 필요없으므로 버려!
arr=list(map(int, input().split()))
cnt=[0]*10

code =[[[0]*128 for _ in range(128)] for _ in range(128)] # 128의 3차원 배열
# 비교 구문 없어도 됨!
code[ord('Z')][ord('R')][ord('0')] = 0 
code[ord('O')][ord('N')][ord('E')] = 1
code[ord('T')][ord('W')][ord('O')] = 2
code[ord('T')][ord('H')][ord('R')] = 3
code[ord('F')][ord('O')][ord('R')] = 4
code[ord('F')][ord('I')][ord('V')] = 5
code[ord('S')][ord('I')][ord('X')] = 6
code[ord('S')][ord('V')][ord('N')] = 7
code[ord('E')][ord('G')][ord('T')] = 8
code[ord('N')][ord('I')][ord('N')] = 9

# 숫자를 하나하나 불러올 필요 x
digit = ["ZRO", "ONE","TWO", "THR", "FOR", "FIV", "SIX", "SVN", "EGT", "NIN"]

for i in range(len(arr)):
    cnt[code[ord(arr[i][0])][ord(arr[i][1])][ord(arr[i][2])]]+=1

for i in range(10):
    for j in range(count[i]):
        print(digit[i], end=' ')
print()
```



## 7. 사다리타기1

> 당첨의 출발지가 어디인지 찾으시오.

🤷‍♀️ 어떻게 풀어야하나?

1. delta 검색 이용
2. 아래에서 출발하여 방향은 ''좌우상''밖에 없음
3. 좌우 먼저(0을 만날때까지: 방문체크), 나머지는 상으로 이동

```python
# 100X100 행렬

SIZE =100 # 파이썬에서 대문자는 상수를 의미
arr=[list(map(int, input().split())) for _ in range(SIZE)]

dx=[0,0,-1] # 좌우상
dy=[-1,1,0]

# 마지막 행에서 2인 좌표 찾기
def find_start(arr):
    for i in range(SIZE):
        if arr[SIZE-1][i] == 2:
            x = SIZE-1
            Y = i
            return x, y

# 갈수있는 길인지 아닌지 확인하는 함수
def check(arr, nx, ny):
    if x<0 or x>=SIZE: return False # 인덱스 벗어나는 걸 먼저!!
    if y<0 or x>=SIZE: return False
    
    if arr[x][y]==0: return False
    if arr[x][y]==9: return False # 방문했던 곳은 9로 채울 것이기 때문
        
def Ladder(arr):
    x, y = find_start(arr) # 현재 좌표
    nx, ny = 0, 0 # 다음 좌표
    
    while True: #언제끝날지 모르므로 무한루프
        if x == 0: return y
        for i in range(3): # 좌우상 방향 보기
            nx=x+dx[i]
            ny=y+dy[i]
            # 갈수 있는 길인지 check
            if check(arr, nx, ny):
                x, y = nx, ny
                arr[x][y]=9 # 방문했을 경우 9로 입력
                break
```

