# string1 문제풀이



## 1. 문자열 비교

> pattern이 text 안에 있는지 확인

🤷‍♀️ 어떻게 풀어야하나?

1. brute force 방법 사용

2. 텍스트의 길이와 pattern 길이 구하기

3. 초기 인덱스 설정 (0으로)

4. while문 구현

   ```python
   str1, str2 # pattern, text
   N, M # len(pattern), len(text)
   
   def bruteforce_for(str1, str2):
       for i in range(M-N+1):
           for j in range(N):
               if str1[j] != str2[i+j]:
                   break
           else:
               retrun 1
       return 0
   
   def bruteforce_while(str1, str2):
       i, j=0 # i:덱스트 인덱스, j:패턴 인덱스
       
       # 패턴의 길이가 끝까지 확인했다면 멈춤
       # 텍스트를 다 확인했다면 멈춤
       while j < N and i < M:
           if str1[j] != str2[i]:
               i = i-j
               j = -1
           i+=1
           j+=1
       if j == N:
           return 1
       else:
           return 0
   ```

   

## 2. 회문

> 어느 방향에서 읽어도 같은 문자열을 회문이라고 한다.

🤷‍♀️ 어떻게 풀어야하나?

1. 3중 for문을 이용

2. 회문의 길이가 M인 것을 고려하여 N-M+1번 순회하는 것을 명심!

   ```python
   N, M # N: 2차원 리스트의 크기, M : 우리가 찾고 싶은 회문의 길이
   words # 2차원 배열
   
   def my_reverse(line):
       r_line =[]
       # 뒤에서부터 읽어오면서 뒤집은 리스트를 만들자
       for i in range(len(line)-1, -1, -1):
           r_line.append(line[i])
       return r_line
   
   def my_find():
       # 전체 크기가 N
       for i in range(N):
           # 가로 검사(행)
           for j in range(N-M+1):
               # 부분 문자열을 위한 빈리스트
               tmp = []
               for k in range(M):
                   tmp.append(words[i][j+k]) 
               # for문은 tmp = words[i][j:j+M]과 같음
               
               if tmp == my_reverse(tmp):
                   return tmp
           # 세로 검사(행)
           for j in range(N-M+1):
               tmp = []
               for k in range(M):
                   tmp.append(words[j+k][i])
               if tmp == my_reverse(tmp):
                   return tmp
   ```

   

## 3. 회문2

🤷‍♀️ 어떻게 풀어야하나?

1. 위의 풀이법과 비슷

2. 패턴의 길이가 정해져있지 않아서 시간초과 발생 가능

3. 회문의 길이가 가장 긴 것부터 확인

4. 1, 2 ..길이의 짧은 회문을 검사할 필요 x

   ```python
   N=100 # 배열 사이즈
   words # 배열
   
   def my_find(M):
       for i in range(N):
           # 부분 문자열의 시작점
           for j in range(N-M+1):
               # 스왑을 응용한 회문검사
               
               # 가로검사
               for k in range(M//2):
                   if words[i][j+k] != words[i][j+M-1-k]:
                       break
                   elif k == M//2 -1:
                       return M
               # 세로 검사
               for k in range(M//2):
                   if words[j+k][i] != words[j+M-1-k][i]:
                       break
                   elif k == M//2 -1:
                       return M
        return 0
   
   # 거꾸로 회문 검사를 한다.
   for i in range(N, 0, -1):
       ans = my_find(i)
       
       if ans !=0:
           break
   ```

   ```python
   # 2차 배열에서 열을 행으로 뒤집기
   my_list = [[1,2,3],
              [4,5,6],
              [7,8,9]]
   
   new_list = list(zip(*my_list))
   # * : unpacking
   # zip : 각 리스트의 i번째 항목끼리 묶음
   ```

   

## 4. 글자수

> 두 문자열 str1과 str2가 있다. str2에 str1(각 문자마다 몇번 들어있는지)이 몇번 포함되는지 확인해보자.

🤷‍♀️  어떻게 풀어야하나?

1. 딕셔너리 이용 /  카운팅 이용

2. str1의 중복된 문자는 필요없음

   ```python
   # 카운팅 이용
   cnt = [0] * len(str1)
   
   for i in range(len(str1)):
       for j in range(len(str2)):
           if str1[i] == str2[j]:
               cnt[i] +=1
   # 가장 큰 값 찾기
   ans = 0
   for i in range(len(cnt)):
       if ans < cnt[i]:
           ans= cnt[i]
           
   #========================================================
   
   # 딕셔너리 이용
   my_dict = {}
   
   for key in set(str1): # set이용하여 중복된 문자열 지워주기
       my_dict[key] = 0
       
   for key in str2:
       if key in my_dict:
           my_dict[key] += 1
           
   ans=0
   for i in my_dict.values():
       if ans < i:
           ans = i
   ```

   ```python
   # 만약 제약조건에서 주어진 문자가 모두 대문자만 들어온다라는 제약이 있다면
   # 아스키코드 이용
   
   check_arr = [0]*26 # str1 해당 글자가 있는지 체크
   cnt_arr = [0]*26 # 해당 글자 카운트
   
   # str1을 순회하면서 알파벳 체크
   for i in str1:
       check_arr[ord(i)-ord('A')] = 1
       
   # 체크된 알파벳의 카운팅
   for i in str2:
       if check_arr[ord(i)-ord('A')]:
           cnt_arr[ord(i)-ord('A')] +=1
   ```

