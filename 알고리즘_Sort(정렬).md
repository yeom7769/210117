# Sort(정렬) 알고리즘

- 입력 받는 방법

```python
# 줄 단위마다 input 들어옴
# T = test_case 횟수
T=int(input())
N=int(input())
arr=list(map(int, input().split()))
```



## 1. min-max

> N개의 양의 정수에서 가장 큰 수와 가장 작은 수의 차이

```python
def Bubble_sort(arr):
    for i in range(len(arr)-1, 0, -1): # 남은 하나의 수는 정렬할 필요 x
        for j in range(i): # # 정렬된 뒷 숫자는 정렬할 필요 x
            if arr[j]>arr[j+1]:
                arr[j], arr[j+1]=arr[j+1],arr[j]
```

- Bubble_sort를 이용하여 리스트를 정렬시키고 min(arr[0])과 max(arr[-1]) 얻음
- 버블정렬 이용시 빅오는 n**2이므로 비효율적일 수 있음. => 이를 고려해보면 for 문안에서 if로 max, min을 추출하면 더 효율적임



## 2. 전기버스

> 종착까지 갈 수 있는 최소 충전 수
>
> K = 한번 충전까지 갈 수 있는 거리
>
> N = 총 거리
>
> M = 충전횟수

- k만큼 가고 충전소가 없다면 한칸한칸 돌아온다. 만약 나 자신으로 돌아온다면 0을 반환

```python
K, N, M = map(int, input().split())
charge=list(map(int, input().split()))

bus_stop = [o]*(N+1)

# 충전소 표시
for i in charge:
    bus_stop[i]=1

bus, ans =0, 0 # 버스 위치, 충전 횟수

while True:
    bus+=K
    if bus>=N: break # 종점 도착 or 지나간 경우
    
    for j in range(bus, bus-k, -1):
        if bus_stop[i]==1:
            ans+=1
            bus=i
            break
    # 충전기 못 찾았을 때
    else:
        ans=0
        break       
```

```python
# 충전소를 0,1로 리스트화 시키지 않고 숫자 그대로 사용하는 방법
charge=[0]+charge+[N]
last=0

for i in range(1,M+2):
    if charge[i]-charge[i-1]>K:
        ans=0
        break
    if charge[i]>last+K:
        last=charge[i-1]
        ans+=1
```



## 3. 구간합

> N개 정수 들어있는 배열에서 M개씩 묶어서 더하고 합이 가장 큰 경우와 가장 작은 경우의 차이를 출력

- 합을 구한 뒤, max_value와 min_value를 바로 판단

- 중복된 연산을 제외하면 더 빨라짐

  ```python
  tmp = 0
  
  # 첫 구간 구하기
  for i in range(M):
      tmp+=nums[i]
      
  for i in range(M,N):
      tmp=tmp+nums[i]-nums[i-M]
      
  ```

  

## 4. 카드

> 가장 많은 카드에 적힌 숫자와 카드가 몇장인지 출력

- input으로 숫자들(0338573와 같은)은 int으로 받으면 앞의 0이 사라짐. 이점 주의해야함. 