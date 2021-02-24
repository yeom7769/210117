# Stack2

1. 계산기
2. 백트래킹
3. 분할정복
4. 실습1,2



## 1. 계산기

> 문자열로 된 계산식이 주어질 때, 스택을 이용하여 계산식의 값을 계산할 수 있음.

STEP1. 중위 표기법(연산자를 피연산자의 가운데 표기)의 수식을 후위 표기법으로 변경 ==> 괄호 없애기 위해

STEP2. 후위 표기법(연산자를 피연산자 뒤에 표기)의 수식을 스택을 이용하여 계산



- STEP1 알고리즘

  - 피연산자는 그냥 쓰고, 연산자는 스택 넣기. 만약 <u>우선순위가 낮은 것</u>(같다면 pop)이 들어온다면 스택에 담겨져 있는 것을 pop

  ```markdown
  예) 우선 중위 표기법에서 후위 표법으로의 변환
  (6+5*(2-8)/2)
  
  1. 토큰 하나 가져오기
  1-1. 피연산자면 바로 출력
  2. 스택 연산 push()
     : 토큰이 연산자면 스택 top과 비교
     - 토큰의 우선순위가 높으면 push
  3. top 변경
  ```

  - 스택 밖에서의 우선순위
    1. `(`
    2. `*/`
    3. `+-`
  - `)` 는 `(` 만날때까지 pop

- STEP2 알고리즘
  - 피연산자를 만나면 스택에 push
  - 연산자를 만나면 필요한 만큼(2개) 피연사자를 스택에서 pop하여 연산하고, 연산 결과를 다시 스택에 push
  - 수식이 끝나면, 마지막으로 스택을 pop하여 출력



## 2. 백트래킹

> 해를 찾는 도중에 '막히면', 되돌아가서 다시 해를 찾아 가는 기법

- 깊이우선탐색과의 차이 : 해결책으로 이어질 것 같지 않으면 그 경로를 따라가지 않음으로써 시도의 횟수를 줄임.(prunning 가지치기)
- 백트래킹은 불필요한 경로를 조기에 차단



- 백트래킹을 이용한 알고리즘은 다음과 같다.

  ```python
  # powerset을 구하는 백트래킹 알고리즘
  
  def construct_candiates(a, k, input, c):
      c[0] = True
      c[1] = False
      return 2
  
  def backtrack(a, k, input):
      global maxcandidates
      c=[0]*maxcandidates
      
      if k  == input:
          process_solution(a, k) # 답이면 원하는 작업을 한다
      else:
          k+=1
          ncandidates = construct_candiates(a, k, input, c)
          for i in range(ncandidates):
              a[k] = c[i]
              backtrack(a, k, input)
              
  maxcandidates = 100
  nmax =100
  a = [0] *nmax
  backtrack(a, 0, 3)
  ```

  ```python
  N = 3 #자료 수
  arr = [1,2,3] # 우리가 활용할 데이터
  sel = [0] * N # 내가 해당 원소를 뽑았는지 체크
  
  def powerset(idx): # 부분집합 함수 #idx : 현재 depth
      if idx == N:
          print(*sel)
          return
      
      # idx자리를 뽑고 간다
      sel[idx] = 1
      powerset(idx+1)
      # idx자리를 뽑지 않는다
      sel[idx] =0
      powerset(idx+1)
  
  powerset(0)
  ```

  ```python
#순열 재귀
  N = 3 #자료 수
  arr = [1,2,3] # 우리가 활용할 데이터
  sel = [0] * N # 내가 해당 원소를 뽑았는지 체크
  check = [0]*N # check가 없으면 !중복순열!
  
  def perm(idx):
      if idx == N: # idx를 r로 바꾸면 nPr!!
          print(sel)
      else:
          for i in range(N):
              if check[i] == 0:
                  sel[idx] = arr[i]
                  check[i] = 1
                  perm(idx+1)
                  check[i] = 0
                  
  perm(0)
  ```
  
  ```python
  #순열 비트
  arr = [1,2,3] # 우리가 활용할 데이터
  sel = [0] * N # 내가 해당 원소를 뽑았는지 체크
  N = 3 #자료 수
  
  def perm(idx, check): # check : 10진수 정수
      if idx==N:
          print(sel)
          return
      
      for j in range(N):
          if check & (1<<j): continue
              
          sel[idx] =arr[j]
          perm(idx+1, check | (1<<j))
          
  perm(0,0)
  ```
  
  ```python
  #순열 스왑
  arr=[1,2,3]
  N =3
  
  def perm(idx):
      if idx==N:
          print(arr)
      else:
          for i in range(idx, N):
              arr[idx], arr[i] = arr[i],arr[idx]
              perm(idx+1)
              arr[idx], arr[i]= arr[i],arr[idx]
  ```



## 3. 분할 정복 알고리즘

1. 거듭제곱

   ```python
   # 거듭제곱 C**n
   # n은 짝수 : C**(n/2)*C**(n/2)
   # n은 홀수 : C**(n/2)*C**(n/2)*C
   
   # 반복문을 이용한 선형시간 O(n)
   
   def iterative_power(x, n):
       result =1
       for i in range(1, n+1):
           result *= x
       return result
   
   def recursive_power(x, n):
       if n==1: return x
       if n%2 == 0:
           y = recursive_power(x, n//2)
           return y*y
       else:
           y =recursive_power(x, (n-1)//2)
           return y*y*x
   ```

   

## 4. 퀵 정렬

> 주어진 배열을 두 개로 분할하고 각각을 정렬한다.