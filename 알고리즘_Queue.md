# 큐

1. 큐
2. 큐의 우선순위
3. bfs
4. 큐의 활용 : 버퍼



### 1. 큐

- 큐의 특징 : 삽입과 삭제의 위치가 제한적인 자료구조

  - 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조
  - 선입선출구조(FIFO, First In First Out)

  

- 큐의 기본 연상

  - 삽입 : enQueue(item)
  - 삭제 : enQueue()
  - 앞쪽에서 원소 삭제 없이 반환 : Qpeek()

  

- 큐의 구현

  1. 선형큐

  - 1차월 배열을 이용한 큐
    - 큐의 크기 = 배열의 크기
    - front : 마지막에 꺼내진 원소의 인덱스
    - rear : 마지막 원소의 인덱스
  - 상태표현
    - 초기상태 : front = rear = -1
    - 공백상태 : front = rear
    - 포화 상태 : rear = n-1(n : 배열의 크기)

  

  - 삽입 : enQueue(item)

    ```python
    def enQueue(item):
        global rear
        if isFull(): print('Queue_Full')
        else:
            rear += 1
            Q[rear] = item
    ```

  - 삭제 : deQueue()

     ```python
    def deQueue():
        global front
        if isEmpty(): print('Queue_Empty')
        else:
            front += 1
            return Q[front]
     ```

  - 공백상태 및 포화상태 검사 : isEmpty(), isFull()

    ```python
    def isEmpty():
        return front == rear
    
    def isFull():
        return rear == len(Q)-1
    ```

  - 검색 : Qpeek()

    - 가장 앞에 있는 원소를 검색하여 반환하는 연산

    ```python
    def Qpeek():
        if isEmpty() : print("Queue_Empty")
        else: return Q[front+1]    
    ```



​	🙄 선형큐의 문제점 : 배열의 앞부분에 활용할 수 있는 공간이 있음에도 포화상태로 잘못 인식하는 경우가 생김.

​	🔥 원형 큐의 논리적 구조 탄생

1. 1 마이쮸 연습문제

   ```python
   N =20 #마이쮸 개수
   
   queue = [(1,0)] #초기화
   # (a, b) a: 사람번호 b : 직전까지 받았던 마이쮸 개수
   
   new_people =1
   last_people = 0
   
   while N>0:
       num, cnt = queue.pop(0) #받으러온 사람, 저번까지 받은 개수
       
       last_people = num # 마지막으로 받으러 온 사람
       cnt +=1 #저번보다는 하나더 챙겨가자
       
       N -= cnt #num이라는 친구가 cnt개수만큼 가져감
       new_people += 1
       
       queue.append((num, cnt)) #맨뒤로가서 다시 줄을 섬
       queue.append((new_people, 0)) # 새로운 사람도 줄섬
   ```

   

2. 원형 큐

   - 초기 공백 상태 : front = rear = 0
   - 포화상태 : 삽입할 rear의 다음 위치 = 현재 front
   - idx의 순환 : front와 rear의 위치가 배열의 마지막 인덱스인 n-1를 가리킨 후, 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동
   - front 변수 : front가 있는 자리는 사용하지 않고 항상 빈자리로 둠
     - empty, full을 분별하기 위해
   - 삽입 위치 : (rear+1)%len(Q)
   - 삭제 위치 : (front+1)%len(Q)

   

3. 연결 큐(linked list)

   - 노드의 연결 순서. 링크로 연결되어 있음

   

### 2. 큐의 우선순위

- 특징
  - 우선순위를 가진 항목들을 저장하는 큐
  - FIFO순서가 아닌 우선순위가 높은 순서대로 먼저 나간다.



- 배열을 이용한 우선순위 큐
  - 원소를 삽입하는 과정에서 우선순위를 비교하여 적절한 위치에 삽입



### 3. 큐의 활용 : 버퍼

> 버퍼 : 데이터를 다른 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역



- 버퍼의 자료 구조 

  - 입출력 및 네트워크와 관련된 기능에서 이용

  

### 4. BFS(너비우선탐색)

> 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식

```python
def BFS(G, v):
    visited = [0]*n
    queue = []
    queue.append(v)
    
    while queue:
        t = queue.pop(0)
        if not visited[t]:
            visited[t]=True
            for i in G[t]:
                if not visited[i]: queue.append(i)
                    
#==========================================================
def BFS(v): # G : 그래프, v : 탐색 시작점
    visited = [0]*(n+1) # n : 정점의 개수
    q = [] # 큐 생성
    
    q.append(v) # 시작정점 v를 enQueue
    visited[v] = 1 # 방문한 것으로 표시
    
    while len(q) != 0: # 큐가 비어있지 않은 경우
        t = q.pop(0) # deQueue
        for w in range(1, v+1): # 정점 t와 인접한 정점 w에 대해
            if visited[w]==0 and adj[t][w]==1: # 방문하지 않은 곳이라면
                q.append(w)
                visited[w] = visited[t] + 1 # 방문한 것으로 표시(거리로 표시)
                
#============================================================
adj = [[0]*(v+1) for _ in range(v+1)]

#인접 행렬 입력
for i in range(E):
    s, e = temp[2*i], temp[2*i+1]
    # 무향그래프
    adj[s][e] = adj[e][s] = 1
    
BFS(1)
```

