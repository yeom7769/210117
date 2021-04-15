# 알고리즘 START 문제 풀이(이진)



## 1. 단순 2진 암호코드

🙄 어떻게 푸나?

1. 뒤에서 부터 찾으면 쉽다(모든 숫자가 1로 끝나기 때문)
2. 비교하는 과정을 맵핑테이블 사용하여 쉽게 구현 => 7차원 구현

```python
import sys
sys.stdin = open("암호1.txt", "r")

T = int(input())

# 전역 => 공통으로 사용하기 위해
code = [[[[[[[[0]*2 for _ in range(2)] for _ in range(2)] for _ in range(2)] for _ in range(2)] for _ in range(2)] for _ in range(2)] for _ in range(2)]
code[0][0][0][1][1][0][1] = 0
code[0][0][1][1][0][0][1] = 1
code[0][0][1][0][0][1][1] = 2
code[0][1][1][1][1][0][1] = 3
code[0][1][0][0][0][1][1] = 4
code[0][1][1][0][0][0][1] = 5
code[0][1][0][1][1][1][1] = 6
code[0][1][1][1][0][1][1] = 7
code[0][1][1][0][1][1][1] = 8
code[0][0][0][1][0][1][1] = 9



# 뒤에서 1찾는 함수
def find_start(arr):
    for i in range(r):
        for j in range(c-1, -1, -1):
            if arr[i][j] == 1: return i, j


for tc in range(1, T+1):
    r, c = map(int, input().split())
    arr = [list(map(int, input())) for _ in range(r)]

    # 마지막 1의 자리 찾기
    sx, sy = find_start(arr)
    # 7개의 이진숫자가 하나의 숫자 * 8자리 = 56
    sy -= 55

    p_code = []
    for i in range(8):
        p_code.append(code[arr[sx][sy]][arr[sx][sy+1]][arr[sx][sy+2]][arr[sx][sy+3]][arr[sx][sy+4]][arr[sx][sy+5]][arr[sx][sy+6]])
        sy += 7

    # 암호 검증
    p_value = (p_code[0]+p_code[2]+p_code[4]+p_code[6])*3 + p_code[1]+p_code[3]+p_code[5]+p_code[7]

    if p_value % 10 == 0:
        print('#{} {}'.format(tc, sum(p_code)))
    else:
        print('#{} {}'.format(tc, 0))


```

