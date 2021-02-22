# string 문제



## 1. 문자가 어디에 들어갈 수 있는가?

> 빈 자리에 K길이의 단어가 들어갈 수 있는 곳이 몇 군데인지 확인하기
>
> 채울있는 곳은 1, 없는 곳은 0인 배열이 주어진다.

🤷‍♀️ 어떻게 풀어야하나?

1.  N X N 행렬이므로 먼저 N번 loop

2. 행 검사

3. 열 검사

   ```python
   for i in range(N):
       cnt = 0
       
       # 행 검사
       for j in range(N):
           if puzzle[i][j] == 1:
               cnt+=1
           if puzzle[i][j] == 0 or j == N-1:
               # 벽을 만났을 때 그동안 쌓아온 cnt 값이 K이면 들어갈 수 있다.
               if cnt == K:
                   ans + =1
               cnt = 0
       # 열 검사 
       for j in range(N):
           if puzzle[j][i] == 1:
               cnt+=1
           if puzzle[i][j] == 0 or j == N-1: # 우측과 하단에 0으로 패팅하면 j == N-1의 조건 필요없음
               # 벽을 만났을 때 그동안 쌓아온 cnt 값이 K이면 들어갈 수 있다.
               if cnt == K:
                   ans + =1
               cnt = 0
   
   ```



## 2. 글자 세로로 읽기

> 각 글자의 i번째의 요소들을 순서대로 출력하여라.

🤷‍♀️ 어떻게 풀어야하나?

1. 글자들의 최대 길이를 구한다.

2. 최대 길이에 미치지 못한 글자들은 예외처리

   ```python
   word = [0] * 5
   max_len = 0
   
   # 글자들의 최대 길이 구하기
   for i in range(5):
       word[i] = list(input())
       if len(word[i]) > max_len:
           max_len = len(word[i])
           
   for i in range(max_len):
       for j in range(5):
           # 인덱스 에러 방지위해
           if len(word[j]) > i:
               print(word[j][i], end='')
           # 또는 예외처리로 해결 가능(추천하지 않음)
       	try : print(word[j][i], end='')
           except: pass
       
   ```

   

## 3. 쇠막대기 자르기

> () : laser,  ( : 쇠막대기의 시작 ,  ) : 쇠막대기의 끝

🤷‍♀️ 어떻게 풀어야하나?

1. laser를 만날때마다 이전 laser들의 수 + 1 로 부분 토막남

2. 열린 괄호 `(` 카운팅

3. 닫힌 괄호 `)` 만났을 때, 전 idx를 확인 => `(` 이라면(laser라면) cnt -= 1, ans += 1

4. 닫힌 괄호 `)` 만났을 때, 전 idx를 확인 => `)` 이라면(laser가 아니라면) cnt -= 1, ans += cnt

   ```python
   iron_bar = input()
   cnt = 0
   ans = 0
   
   for i in range(len(iron_bar)):
       # 열린 괄호라면 막대 추가
       if iron_bar[i] == '(':
           cnt += 1
       else:
           # 닫힌 괄호하면 막대감소
           # 레이저라면 당연히 카운트 감소
           cnt -= 1
           
           # 레이저 라면
           if iron_bar[i-1] == '(':
               #레이저로 인해 잘린 막대들이 생겼으므로
               ans += cnt
           else:
               # 막대의 끝이라면
               ans += 1
   ```

   ```python
   # 리스트 활용
   s = []
   ans = 0
   
   for i in range(len(iron_bar)):
       if iron_bar[i] == '(':
           s.append('(')
       else:
           s.pop()
           
           if iron_bar[i-1] == '(':
               ans + = len(s)
           else:
               ans += 1
   ```

