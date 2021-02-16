# 알고리즘 문제풀이(배열)



## 1. 5X5 행렬에서 인접한 요소들의 차이들의 절댓값 합을 구하여라

> delta 검색 사용

```python
'''
1 1 1 1 1
1 0 0 0 1
1 0 0 0 1
1 0 0 0 1
1 1 1 1 1
'''

# 절댓값 계산 함수
def cal_abs(num1, num2):
    if num1 - num2 >= 0: return num1-num2
    else: return num2-num1

# 입력 받기
N=5
arr = [[0 for _ in range(5)] for _ in range(5)] # 0으로 초기화 (안해도 됨)
for i in range(5):
    arr[i] = list(map(int, input().split()))

dr = [-1, 1, 0, 0] # 상하좌우
dc = [0, 0, -1, 1]

sum_value = 0
for r in range(N):
    for c in range(N):
        for i in range(4):
            nc = c + dc[i]
            nr = r + dr[i]
            if 0 <= nc < N and 0 <= nr < N: # 파이썬만 되는 조건 => a<b<c 와 같이 이중 부등호는 파이썬만 가능
            # if nc < 0 or nc >= N: continue
            # if nr < 0 or nr >= N: continue
                sum_value += cal_abs(arr[r][c], arr[nr][nc])

print(sum_value)
# 24
```



## 2. 달팽이 숫자

> 달팽이는 1부터 N*N까지의 숫자가 시계방향으로 이루어져 있다. 다음과 같이 정수 N을 입력 받아 N크기의 달팽이를 출력하시오.

🤷‍♀️ 어떻게 풀어야하나?

- N*N 행렬을 0으로 채움
- 시계방향 => 우하좌상
  - dr=[0,1,0,-1]
  - dc=[1,0,-1,0]
- for => N*N 돌리기
- dir(현재위치) = 0
- 방향 바꾸는 조건
  - 인덱스조건(막혔을 때)
  - arr[dr] [dc] != 0
- 방향 바꾸기 dir=(dir+1)%4

```python
# 달팽이 숫자
T = int(input())
for test_case in range(1, T + 1):
    N=int(input())
    arr=[[0 for _ in range(N)] for _ in range(N)]
    # 시계방향 우하좌상
    dr=[0, 1, 0, -1]
    dc=[1, 0, -1, 0]
    # 초기 위치 r,c와 채워넣을 숫자 num
    r, c, num=0, 0, 0
    # k=dir로 dr, dc의 idx
    k=0
    
    # 행렬의 모든 칸을 다 돌아야하므로 N*N번 loop
    for i in range(N*N):
        # 숫자 입력
        num+=1
        arr[r][c]=num
        # 위치 옮기기
        r+=dr[k]
        c+=dc[k]
        # 만약 idx를 벗어나거나 0이 아닌 숫자로 채워진 곳이라면
        if r<0 or r>=N or c<0 or c>=N or arr[r][c] !=0:
            # 그 전 위치로 돌아가서
            r-=dr[k]
            c-=dc[k]
            # dr, dc 위치를 바꿔줌
            k+=1
            k%=4
            #방향을 바꾼 위치로 옮기기
            r+=dr[k]
            c+=dc[k]
```



## 3. sum

> 100X100의 2차원 배열이 주어질 때, 각 행의 합, 각 열의 합, 각 대각선의 합 중 최댓값을 구하는 프로그램을 작성하여라.

🤷‍♀️ 풀이 방법

- 각 행, 열의 합 : 행 우선, 열 우선
- 대각선 : 델타 검색 사용

```python
# 배열 받기
arr= [list(map(int, input().split())) for _ in range(100)]
max_value=0
N=100

# 행 우선
for i in range(N):
    sum_value=0
    for j in range(N):
        sum_value+=arr[i][j]
    if sum_value > max_value:
        max_value=sum_Value
        
# 열 우선
for i in range(N):
    sum_value=0
    for j in range(N):
        sum_value+=arr[j][i] # 정사각형이면 j, i만 바꾸면 가능
    if sum_value > max_value:
        max_value=sum_Value
        
# 대각선 \
sum_value=0
for i in range(N):
    sum_value+=arr[i][i]
if sum_value > max_value:
    max_value=sum_Value

# 대각선 /
sum_Value=0
for i in range(N):
    sum_value+=arr[i][N-1-I]
if sum_value > max_value:
    max_value=sum_Value
```

