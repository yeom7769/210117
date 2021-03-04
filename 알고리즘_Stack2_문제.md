# Stack2 문제풀이



## 1. forth

> 후위 표기법을 사용한 forth라는 컴퓨터 언어에서 연산 결과를 출력하는 프로그램을 만드시오. 형식이 잘못되어 연산이 불가능한 경우 'error' 출력

🙄 푸는 방법

1. stack 사용

2. 피연산자인 경우 예외처리 방법 : isdigit() 사용 🔥 문자일 때 사용해야 함

3. 연산자인 경우 예외처리 방법 : 피연산자가 2개 미만일 때 error

   ```python
   def calc(exp): # exp : 입력값
       stack = []
    # 문자열 스캔
       for i in range(len(exp)):
       # 연산자일 때 (피연산자 2개이상인지 체크)
       	if exp[i] == '+' or exp[i] == '-' or exp[i] == '*' or exp[i] == '/':
               if len(stack) >= 2:
                   op2 = int(stack.pop())
                   op1 = int(stack.pop())
                   if exp[i] == '+': stack.append(op1+op2)
                   elif exp[i] == '-': stack.append(op1-op2)
                   elif exp[i] == '*': stack.append(op1*op2)
                   else: stack.append(op1//op2)
               else: return 'error'
       # 피연산자일 때(push)
   		elif exp[i] != '.': stack.append(exp[i])
       # .일 때
       	else:
               if len(stack) == 1: return stack.pop()
               else: return 'error'
   ```
   
   

## 2. 미로

> 미로에서 출발지에서 목적지에 도착하는 경로가 존재하는지 확인하는 프로그램을 작성. 도착할 수 있으면 1, 없으면 0 출력

🙄 푸는 방법

1. 재귀 사용 : dfs

2. 델타 사용

3. 2인 출발지에서 0을 따라 3인 도착지에 도착해야함

4. 2 찾기

5. dfs : 방문체크 v + 인접정점 w(인덱스 체크 -> 방문체크 -> 0,1 구분) + dfs(w)

   ```python
   def dfs(x, y):
       global flag
       if arr[x][y] == 3: 
           flag = 1
           return # 3이후의 0은 갈 필요가 없어짐
       
       dx = [-1, 1, 0, 0] # 상하좌우
   	dy = [0, 0, -1, 1]
       #방문처리
       arr[x][y] = 9
       #시작점에 인접한 정점(w) 중에 방문하지 않은 정점이 있으면 dfs
       for i in range(4):
           nx = x+dx[i]
           ny = x+dy[i]
       	# 0은 통로, 1은 벽, 3은 도착, 9는 방문
           if nx < 0 or nx >= N: continue # 인덱스체크
           if ny < 0 or ny >= N: continue
           if arr[nx][ny] == 9 : continue # 방문체크
           if arr[nx][ny] == 1 : continue # 벽 체크
           dfs(nx, ny)
           
   
   def find_start(arr):
       for x in range(N):
           for y in range(N):
               if arr[x][y] == 2: return x, y
   
   flag = 0 #dfs
   
   sx, sy = find_start(arr)
   dfs(sx, sy)
   ```

   

## 3. 배열 최소 합

> 행렬에서 한줄에 하나씩 숫자를 골라 합의 최소를 출력하는 프로그램을 만드시오. 세로로 같은 줄에서 두 개 이상의 숫자를 고를 수 없다.

🙄 푸는 방법

1. 순열 사용

2. 가지치기 : 현제 총합이 최소합보다 크면 return 

   ```python
   def perm(n, k, cursum): # k : depth
       global ans
       #가지치기
       if cursum > ans: return
       
       if n == k: 
           if ans > cursum : ans = cursum
       else:
           for i in range(N):
               if visited[i]: continue
               order[k] == arr[k][i]
               visited[i] = 1
               perm(n, k+1, cursum+order[k])
               visited[i] = 0
   ```

   

### 4. 토너먼트 카드게임

> 두 그룹이 각각 1명이 되면 양 쪽의 카드를 비교해 승자를 가리고, 다시 더 큰 그룹의 승자를 뽑는 카드게임을 진행한다.

🙄 푸는 방법

1. 재귀 사용

2. l==r이면 return ㅣ

3. l != r이면 반으로 계속 줄이기

   ```python
   def f(l, r): #l : 외쪽인덱스 r : 오른쪽인덱스
       #기본파트
       if l == r: return l
       #유도파트
       #아래서 받은 값
       r1 = f(l, (l+r)//2)
       l1 = f((l+r)//2+1, r)
       #계산해서 리턴
       if card[r1] == card[r2]:
           return r1
       else: 
           #가위바위보 처리
   ```

   

### 5. 계산기

> 문자열로 이루어진 계산식이 주어질 때, 이 계산식을 후위 표기식으로 바꾸어 계산하는 프로그램을 작성하시오.

🙄 푸는 방법

1. 피연산자 -> stack에 저장

2. 연산자 

   1. `(`: push
   2. `)` : `(`까지 pop => stack에 저장
   3. 사직연산자 
      - 토큰 > stack[-1] : push
      - 토큰 <= stack[-1] : 토큰보다 낮은 연산자까지 pop => stack에 저장

   ```python
   def priority(c): # 우선순위 판별
       if c == '(' : return 0
       elif c == '+' or c =='-': return 1
       elif c == '*' or c == '/':return 2
       else: 3
   
   def infix_to_postfix(infix):
       rst_str = []
       #중위식 스캔
       for i in range(len(infix)):
       	# 피연산자 -> 문자열 저장
           if infix[i].isdigit():
               rst_str.append(infix[i])
           # 연산자
           else:
           	# ( : push
               if infix[i] == '(':
                   stack.append(infix[i])
               # ) : (나올 때까지 pop
               elif infix[i] == ')':
                   while stack[-1] != '(':
                       rst_str.append(stack.pop())
                   stack.pop() # 마지막으로 만나는 '('는 그냥 버림
               # 사칙연산
               else:
                   if len(stack) != 0:
                       while priority(infix[i]) <= priority(stack[-1]):
                           rst_str.append(stack.pop())
                           if len(stack) == 0: break
                   stack.append(infix[i])
                   
   	while len(stack) != 0:
           rst_str.append(stack.pop())
       return ''.join(rst_str)
   
   ```

   