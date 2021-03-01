# string 과제



## 1. 파스칼의 삼각형

> 첫 번째 줄은 항상 1
>
> 두 번째 줄부터 각 숫자들은 자신의 왼쪽과 오른쪽 위의 숫자의 합

🤷‍♀️ 어떻게 풀어야하나?

1. 2차원 배열 초기화

2. for문을 사용

   ```python
   for i in range(10):
       for j in range(i+1):
           if j == 0 or i == j:
           	memo[i][j] = 1
           else:
               memo[i][j] = memo[i-1][j-1] + memo[i-1][j]
          
   ```
