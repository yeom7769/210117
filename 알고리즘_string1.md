# 문자열(string)

- 문자열
- 패턴매칭
- 문자열 암호화
- 문자열 압축
- 실습 1,2



## 1. 문자열

> 문자의 표현 : 비트맵으로 저장하는 방법을 사용하지 않는 한(이 방법은 메모리 낭비가 심함) 각 문자에 대응하는 수자를 정해 놓고 이것을 메모리에 저장한다 => 유니 코드



1. 문자열의 분류
   
   - fixed length vs variable length(length controlled vs delimited)
   
2. 파이썬에서의 문자열 처리

   -  char 타입 없으며 str타입있음
   - ``+`` 연결 : 문자열을 이어 붙여줌
   - `*` 반복 : 곱하기연산처럼 문자열을 반복시킴
   - 요소값을 변경 할 수 없음(immutable)

3. 문자열 뒤집기

   - 리스트로 변환

   - 반복 횟수 : 몫만 수행
   - swap 을 위한 임시 변수 필요

   🤷‍♀️ 연습문제1

```python
# 문자열 뒤집기 구현하기
s='abcd'
s=s[::-1] # slicing으로 구현
arr=list(s) # str->list
arr.reverse() #뒤집기
s2=''.join(arr) # list -> str

# 1. 뒤에서 읽어와서 작성하는 법
# 2. swap하는 법
def my_reverse(s):
	s=list(s)
    n=len(s)
    
    for i in range(n//2):
        s[i], s[n-i-1] = s[n-i-1], s[i]
    reverse_s=''.join(s)
    
    return reverse_s
```

4. 문자열 비교

   - == 연상자 vs is연산자

     : 값이 같은지  vs 객체가 같은지

5. 타입 변환

   ```python
   num_str='1234'
   
   value=0
   for i in range(len(num_str)):
       value*=10
       value+=ord(num_str[i])-ord('0')
       # ord('0')=48
   # value는 num_str의 int형으로 변환
   ```
```
   
🤷‍♀️ 연습문제2
   
   ```python
   # str() 사용하지 않고, itoa() 구현하기
   # 즉 숫자를 str로 변환하기
def itoa(num):
       num_list=[]
   
       while num > 0:
           y=num%10
           num_list.append(chr(y + ord('0'))) #문자로 넣어짐
           num//=10
           
       itoa_num=''.join(num_list)
       return itoa_num
```

   

## 2. 패턴 매칭

1. brute force

   > 고지식한 패턴 검색 알고리즘
   >
   > 처음부터 순회하면서 패턴 내의 문자들을 일일이 비교

   ```python
   def BruteForce(pattern,text):
       N = len(text)
       M = len(pattern)
       pattern_idx = 0
       text_idx = 0
       
       while pattern_idx < M and text_idx < N:
           if text[text_idx] != pattern[pattern_idx]:
               text_idx -= pattern_idx
               pattern_idx = -1
           text_idx += 1
           pattern_idx += 1
           
       if pattern_idx == M : return text_idx - M # text_idx부터 같은 패턴 검색 성공
       else: return -1
   ```

   

2. KMP 알고리즘

   > 패턴을 전처리하여 배열 next[M]을 구해서 잘못된 시작을 최소화함



3. 보이어-무어 알고리즘

   > 오른쪽 끝에 있는 문자가 불일치하고 그 문자가 패턴 내에 존재하지 않는 경우, 이동 거리는 패턴의 길이만큼 줄어듬