# queue 문제풀이



## 1. 피자굽기

> 화덕에 가장 마지막까지 남아있는 피자 번호를 알아내는 프로그램. 치즈는 한번 돌떄마다 //2을 해준다.

🙄 푸는 방법

1. 큐 사용

2. 출력을 피자의 번호로 해야하므로 치즈양과 인덱스 번호를 한번에 묶어야함

   ```python
   # N : 화덕 크기 M : 피자의 개수 pizza : 치즈양
   firepot = [] # 화덕 큐 생성
   
   for i in range(N):
       firepot.append((i+1, pizza[i]))
       
   next_pizza = N
   last_pizza = -1
   
   while firepot:
       num, cheese = firepot.pop(0)
       
       cheese //= 2
       last_pizza = num
       
       #치즈가 남아있다면
       if cheese:
           firepot.append((num, cheese))
       else:
           if next_pizza < M:
               firepot.append((next_pizza+1, pizza[next_pizza]))
               
               next_pizza += 1
   ```

   

## 2. 노드의 거리

> 출발 노드에서 최소 몇 개의 간선을 지나면 도착 노드에 갈 수 있는지 알아내는 프로그램을 만드시오.

🙄 푸는 방법

1. BFS 사용

2. 출발지 방문체크

3. 큐 삽입

4. while != queue

5. 큐 pop -> adj 확인

6. 갈수 있는 길+방문x => 큐삽입

   ```python
   # V : 노드 개수 E : 간선 수 S : 출발노드 G : 도착노드
   
   # 인접행렬 초기화
   adj = [[0]*(V+1) for _ in range(V+1)]
   
   # 입력을 통해 인접행렬 할당
   for i in range(E):
       a, b = map(int, input().split())
       # 무방향이므로 양쪽연결
       adj[a][b] = adj[b][a] = 1
       
   def BFS(S):
       # 큐를 생성과 동시에 선언
       Q = [[S, 0]]
       visited = [False]*(V+1)
       visited[S] = True
       
       while Q:
           v, dist = Q.pop(0)
       
       	if v == G: return dist
           
           for i in range(1, V+1):
               if adj[V][i] == 1 and visited[i] ==False:
                   Q.append([i, dist+1])
                   visited[i] = True
   ```



## 3. 미로의 거리

> NxN 크기의 미로에서 최소 몇 개의 칸을 지나면 출발지에서 도착지에 다다를 수 있는지 알아내는 프로그램을 작성하시오.

🙄 푸는 방법

1. BFS 사용

2. 델타 탐색 이용

3. 출발지 2를 찾는 search 함수 구현

   ```python
   dr = [-1, 1, 0, 0]
   dc = [0, 0, -1, 1]
   
   def search():
       for i in range(N):
           for j in range(N):
               if maze[i][j] == '2': return i, j
   
   def BFS(r, c):
       #선형큐를 이용해서 작성을 해보자.
       q = [o]*10000
       front = -1
       rear = 0
       q[rear] = (r,c)
       
       dist = [[-1]*(N) for _ in rnage(N)]
       dist[r][c] = 0
       
       while front != rear:
           front += 1
           curr_r, curr_c = q[front]
           if maze[curr_r][curr_c] == '3': return dist[curr_r][curr_c]-1
           for i in range(4):
               nr = curr_r + dr[i]
               nc = curr_c + dc[i]
               
               if nr<0 or nr>=N or nc<0 or nc>=N: continue
               if maze[nr][nc] != '1' and dist[nr][nc] == -1: 
                   dist[nr][nc] = dist[curr_r][curr_c] + 1
                   rear += 1
                   q[rear] = (nr, nc)
       return 0
   ```

