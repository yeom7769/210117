# Stack1

- 스택
- 재귀호출
- Memoization
- DP
- DFS
- 실습 1,2,3



## 1. 스택

> 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료 구조

- 스택에 저장된 자료는 <u>선형 구조</u>를 갖는다.
  - 선형 구조 : 자료 간의 관계가 1:1의 관계
- 스택에 자료를 삽입하거나 스택에서 자료를 꺼낼 수 있다.
- 🔥**`후입선출`**(LIFO, Last In First Out)



#### 1.1  스택의 구현

- 자료구조 : 자료를 선형으로 저장할 저장소
  - 스택에서 마지막 삽입된 원소의 위치 : top
- 연산
  - 삽입 : push
    - append()
  - 삭제 : pop() 
    - pop()의 인덱스를 적지 않으면 맨 마지막을 꺼냄
  - 스택이 공백인지 확인 : isEmpty
  - top에 있는 원소를 반환하는 연산 peek(확인만 가능. 데이터를 꺼내지 않고 값만 반환)



1. 스택의 push 알고리즘

   > 배열은 크기가 정해져있어서 배열 안이 비어있는지, 채워져있는지 확인부터 해야함
   >
   > 리스트는 크기가 유동적이라서 상관 없음

2.  스택의 pop 알고리즘

   > 공백 검사 필수

   ```python
   SIZE = 100
   stack = [0]*SIZE
   top = -1
   k = 0
   
   def push(item):
       global top
   
       if top > SIZE - 1:
           return
       else:
           top += 1
           stack[top] = item
   
   def pop():
       global top
       if top == -1:
           print("Empty!")
       else:
           result = stack[top]
           top -= 1
           return result
   ```

   

- 스택 구현 고려 사항
  - 1차원 배열을 사용하여 구현할 경우, 스택의 크기를 변경하기가 어렵
  - 저장소를 동적으로 할당하여 스택을 구현하면 위의 문제를 해결 가능



#### 1.2 재귀 호출

> 자기 자신을 호출하여 순환 수행되는 것

- 피보나치 수를 구하는 재귀함수

  ```python
  def fibo(n):
      if n<2:
          return n
      else:
          return fibo(n-1)+fibo(n-2)
  ```

- 재귀 호출 시, 시간이 매우 오래 걸림

  🤷‍♀️ 엄청난 중복 호출 때문!

  

#### 1.3 Memoization

> <u>이전에 계산한 값을 메모리에 저장</u>해서 다시 계산하지 않도록 하여 전체적인 실행속도를 빠르게 하는 기술

-  동적 계획법(DP)의 핵심이 되는 기술

```python
# memo를 위한 배열을 할당하고, 모두 0으로 초기화
# memo[0]을 0으로 memo[1]은 1로 초기화

def fibo1(n):
    global memo
    if n >= 2 and len(memo) <= n:
        memo.append(fibo1(n-1)+fibo1(n-2))
    return memo[n]

memo = [0, 1]

#================================================
memo2 = [-1] *21
memo2[0] =0
memo2[1] =1

def fibo2(n):
    if memo2[n] == -1:
        memo2[n] = fibo2(n-1) + fibo2(n-2)
    return memo2[n]
```



#### 1.4 DP(Dynamic Programming)

> 최적화 문제를 해결하는 알고리즘
>
> 입력크기가 작은 것부터 구해서 입력크기를 키운다.



#### 1.5 ✨DFS(깊이우선탐색)

> 비선형구조인 그래프 구조는 모든 자료를 빠짐없이 검색하는 것이 중요

1. 깊이 우선 탐색(Depth First Search, DFS)

2. 너비 우선 탐색(Breadth First Search, BFS)

   ```python
   def fibo2(n):
   	f= [0,1]
   
   	for i in range(2, n+1):
   		f.append(f[i-1]+f[i-2])
   
   	return f[n]
   ```

   

- **`DFS 알고리즘`**
  1. 시작 정점 v를 결정하여 방문
  2. 정점 v에 인접한 정점 중에서 
     - 방문하지 않았던 정점이 있다면 정점 v를 push(오름차순으로)
     - 방문했다면 pop해서 되돌아 감
  3.  스택이 공백 될 때까지 2반복
  
  ```python
  def dfs_recursive(G, V):
      visited[v] == True
      
      for w in adj(G, V): # 인접 행렬의 모든 정점 LOOP
          if visited[w] != true:
              dfs_recursive(G, W)
  ```
  
  ```python
  def dfs(v): # v : 시작정점
      visited = [0]*(V+1) # V : 정점 개수
      # visited 체크 : 하고픈 일 해라(출력)
      visited[v] = 1
      
      # 시작정점 v의 인접한 모든 정점(w)에 대해서 loop
      # 인접정점 w가 방문하지 않았으면 다시 dfs(w)로 재귀 호출
      for w in range(1, V+1):
      	if adj[v][w] == 1 and visited[w] == 0:
              dfs(w)
              
  ```
  
  

🔥 최대 간선 수 : v(v-1)/2



#### 1.6 연습문제3

> 인접 리스트
>
> 연결되어 있는 두개의 정점 사이의 간선을 순서대로 나열 해 놓은 것을 이용하여 깊이 우선 탐색 경로를 출력하시오. 시작 정점을 1로 시작

```python
'''
7 8 # 정점 개수, 간선 개수
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''

v, E # 정점 개수, 간선 개수
tmp = list(map(int, input().split()))
adj = [[0] * (V+1) for _ in range(V+1)] # 인덱스에 체크하기 위해. 정점은 1번부터 7번까지
#입력
for i in range(E):
    S, E = tmp[2*i], tmp[2*i+1]
    adj[s][e] = 1
    adf[e][s] = 1
```

