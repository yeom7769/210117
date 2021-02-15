# 배열 2(Array2)

- 배열 : 2차 배열
- 📌부분집합 생성
- 바이너리 서치(binary search)
- 셀렉션 알고리즘(selection)
- 선택 정렬



## 1. 배열

1. 2차원 배열

> 1차원 list를 묶어놓은 list
>
> 🙋‍♀️ 같은 크기 아니여도 2차원 배열 가능

- 가로 : 행 / 세로 : 열

  ```python
  arr=[[0,1,2],[3,4,5]]
  # 2X3 (행X열)
  print(arr[0][1])
  # 1
  # 요소 접근시, index로 접근 가능
  ```

  ```python
  # 2차원 배열 생성
  N, M = map(int, input().split())
  
  arr=[]
  for i in range(N):
      arr.append(list(map(int, input().split())))
  ==================================================================
  arr=[0]*N
  for i in range(N):
      arr[i]=list(map(int, input().split()))
  ==================================================================
  arr=[list(map(int,input().split())) for _ in range(N)]
  # _는 값의 활용이 없기 때문에 i대신 _씀
  ```
  
  ```python
  # 2차원 배열 초기화
  arr=[[0 for _in range(M)] for _ in range(N)]
  arr2=[[0]*M for _ in range(N)]
  ```
  
  

2. 2차원 배열의 접근

- 배열 순회 : n xm 배열의 n*m개의 모든 원소를 빠짐없이 조사하는 방법

- **`행 우선 순회`**

  ```python
  # i 행의 좌표
  # j 열의 좌표
  for i in range(len(Array)):
      for j in range(len(Array[i])): # 역행 : for j in range(M-1,-1,-1)
          print(Array[i][j])
  ```
  
- **`열 우선 순회`**

  ```python
  for j in range(len(Array[o])):
      for i in range(len(Array)):
          Array[i][j] #필요한 연산 수행
  # 역열        
  # for i in range(len(Array)-1,-1,-1)
  ```

- 지그재그 순회(참고)

  ```python
  for i in range(len(Array)):
      for j in range(len(Array[i])):
          Array[i][j+(m-1-2*j)*(i%2)]
  # if 분기 사용법도 가능
  ```

- **`🔥델타를 이용한 2차 배열 탐색`**

  - 현재 위치를 알아야함

    |      | 행   | 열   |
    | ---- | :--- | ---- |
    | 상   | -1   | 0    |
    | 하   | +1   | 0    |
    | 좌   | 0    | -1   |
    | 우   | 0    | +1   |

    ```python
    dr=[-1,1,0,0] #상하좌우
    dc=[0,0,-1,1]
    drc=[[-1,0],[1,0],[0,-1],[0,1]]
    #현재 위치
    r=1
    c=1
    
    for i in range(4):
        nr=r+dr[i]
        nc=c+dc[i]
        if nr<0 or nr>=N or nc<0 or nc>=N: continue # 벽에 붙어있는 경우
        print(arr[nr][nc])
    ```

    

## 2. 부분집합

> 추후에는 재귀로 구현!

1. 부분집합의 수 

   : 집합의 원소가 n개일 때, 공집합을 포함한 부분집합의 수 = 2**n

   

2. 부분집합 생성

   ```python
   bit=[0,0,0,0]
   for i in range(2): # 0,1
       bit[0]=i
       for j in range(2):
           bit[1]=j
           for k in range(2):
               bit[2]=k
               for l in range(2):
                   bit[3]=l
   ```

   - <u>출력은 2진수(bit)로 나타남!</u>

     

3. 비트 연산자

   - `&` : and 연산
   - `|` : or 연산
   - `<<` : 비트 열을 왼쪽으로 이동(새로운 자리는 0으로 채워짐)
     - <u>!!2배가 됨!!</u>
     - A << N : A라는 숫자를 N번 왼쪽으로 이동 = A*(2**N)
   - `>>` :  비트 열을 오른쪽으로 이동
     
     - <u>!!0.5배가 됨!!</u>
     
       

4. **`🔥간결한 부분집합 생성`**

   ```python
   arr=[4,5,6,7]
   
   n=len(arr)
   for i in range(1<<n):
       for j in range(n):
           if i & (1<<j):
               print(arr[j], end=",")
       print()
   print()
   ```




## 3. 검색(Search)

> 저장되어 있는 자료 중에서 원하는 항목 찾는 방법

- 순차 검색(sequential search)
- 이진 검색(binary search)
- 인덱싱(indexing)



1. 순차검색(Sequential Search)

   > 가장 간단 + 직관적 but 검색 대상 수 많은 경우, 수행시간 급격히 증가 -> 비효율적
   >
   > 순차구조로 구현된 자료구조에서 원하는 항목을 찾을 때 유용

   ```python
   arr=[4,9,11,23,19,7]
   
   key=2
   
   for i in range(len(arr)):
       if key==arr[i]:
           print(i, "에 위치하고 있음")
           break
   else:
       print("못 찾음")
   ==============================================
   i=0
   n=len(arr)
   
   while i<n and arr[i]!=key:
       i+=1
   
   print(i, "에 위치하고 있음")
   ```



2. 이진 검색(Binary Search)

   > 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법 => <u>정렬된 상태여야 함</u>

   1️⃣ 자료의 중앙에 있는 원소를 고름

   2️⃣ 중앙 원소의 값과 찾고자 하는 목표값을 비교

   3️⃣ 작다면 왼쪽 반에 대해서 새로 검색을 수행, 크다면 오른쪽 반에 대해서 새로 검색

   ```python
   def binarySearch(a, key):
       start = 0
       end = len(a)-1
       
       while start <= end:
           middle = (start + end)//2
           if a[middle] == key: #검색 성공
               return true
           elif a[middle] > key:
               end = middle-1
           else: start = middle+1
       return false # 검색 실패
   ```



# 4. 선택 정렬

> 주어진 자료들 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환하는 방식

- 일반적인 설렉션 알고리즘

> k번째로 작은 원소를 찾는 알고리즘

```python
def selectionSort(a):
    for i in range(len(a)-1):
        min_idx=i
        for j in range(i+1,len(a)):
            if a[min_idx] > a[j]:
                min_idx=j
        a[i], a[min_idx]=a[min_idx],a[i]
```







