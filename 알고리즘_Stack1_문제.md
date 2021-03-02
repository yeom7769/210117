# Stack1 문제풀이

- 그래프 순회(탐색)
  - 깊이 우선 탐색(dfs) : stack(후입선출)
  - 너비 우선 탐색(bfs) : queue(선입선출)



## 1. 그래프 경로

> V개의 노드를 E개의 간선으로 연결한 방향성 그래프에서 특정 두 개의 노드에 경로가 존재하는 확인해보자.

🤷‍♀️ 푸는 방법

1. DFS 방식 사용
2. return은 초기값으로 돌아감을 뜻함 => flag, visited 활용, return

```python
def dfs(v):
    global flag
    if V == e:
        flag = 1
        return 
    
    visited[v] = 1
    
    for w in range(1, V+1):
        if visited[w] == 0 and adj[v][w] == 1:
            dfs(w)

def dfs2(v):
    if V == e:
        return 1
    
    visited[v] = 1
    
    for w in range(1, V+1):
        if visited[w] == 0 and adj[v][w] == 1:
            if dfs(w) == 1:
                return 1
    return 0
            
flag = 0
V, E # 노드, 간선
adj #인접행렬
visited = [0]*(V+1) #방문체크
s, e # 시작, 도착
```



## 2. 반복된 문자 지우기

> 문자열 s에서 반복된 문자를 지운다.

🤷‍♀️  푸는 방법

1. stack 에 push
2. top과 비교
3. 같으면 지우기(pop할때 stack 안이 비어있는지 확인부터하기)

```python
stack = []
str # 문자열 s

for i in range(len(str)):
    #스택 비어있으면 push
    if not stack:
        stack.push(str[i])
    else:
        if stack[-1]==str[i]:
            stack.pop()
   
```



## 3. 괄호검사

> {} ()가 제대로 짝을 이뤘는지 검사

🤷‍♀️ 푸는 방법

1. 왼쪽 `(`,`{` : push
2. 오른쪽 `)`,`}` : pop
3. isEmpth, 같은 쌍인지 체크

```python
def solve(str):
    global flag
    stack = []
    
    for i in range(len(str)):
        # 왼쪽괄호면 push
        if str[i] == '{' or str[i] == '(':
            stack.append(str[i])
        # 오른쪽괄호면 pop
        elif str[i] == '}' or str[i] == ')':
            # isEmpth
            if len(stack) == 0:
                flag = 0
                return 
            else:
                temp = stack.pop()
                #같은상인지 확인
                if str[i] == ')':
                    if temp != '(':
                        flag = 0 
                        return
                else:
                    if temp != '{':
                        flag = 0
                        return
                    
     if len(stack) != 0:
        flag = 0
        return
                

flag = 1
solve(str)
```



## 4. 색종이붙이기

🤷‍♀️ 푸는 방법

1. 재귀 사용
2. 귀납 활용

```python
def f(k):
    if k <= 1: return 1
    else: return f(k-1)+2*f(k-2)
```

