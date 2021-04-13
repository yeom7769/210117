# 0408



1. 서브트리

   ```python
   def preorder(node):
       global cnt
       if node != 0:
           cnt += 1
           preorder(tree[node][0]) # 왼쪽 자식
           preorder(tree[node][1]) # 오른쪽 자식
           
   
   E , N = map(int, input().split()) # 간선수, 시작노드
   tree = [[0]*3 for _ in range(E+1)] # 왼쪽 오른쪽 부모
   temp = list(map(int, input().split()))
   cnt = 0 # 서브트리의 노드수
   
   for i in range(E):
       p = temp[i*2] # 부모 노드
       c = temp[i*2+1] # 자식 노드
       if tree[p][0] == 0:
           tree[p][0] = c
       else:
           tree[p][1] == c
           
       tree[c][2] == p
       
   preorder(N)
   ```

   

2. 이진 탐색 트리에 저장한 경우

   ```python
   def inorder(node):
       global idx
       if node <= N: # 완전이진트리 정점의 수보다 작아야함
           inorder(2*node)
           tree[node] = idx
           idx += 1
           inorder(2*node+1)
   
   N = int(input()) # 마지막 정점번호(1~N)
   tree = [o] * (N+1) # 완전이진트리 리스트
   idx = 1 # 포화이진트리의 번호
   
   inorder(1)
   
   ```

   

3. 힙

   ```python
   def heapPush(value):
       global heapCount
       heapCount += 1 # 마지막 노드번호 증가
       heap[heapCount] = value # 마지막 노드에 값 저장
       child = heapCount # 마지막 노드가 child
       parent = child//2 # 마지막 노드의 parent
       
       while parent and heap[parent]>heap[child]:
           heap[parent], heap[child] = heap[child], heap[parent]
           child = parent
           parent = child//2
   
           
   N # 정점의 수
   heap = [0]*(N+1) #완전이진트리 일차원 리스트
   heapCount = 0
   temp = list(map(int, input().split()))
   
   for i in range(temp):
       heapPush(temp[i])
   ```

   

4. 리프노드

   ```python
   def postOrder(node):
       if node <= N: # 유효한 노드
           # 앞노드이 경우
           if 2*node >N :
               return tree[node]
           # 가지노드의 경우
       	else:
           	# 왼쪽, 오른쪽 받아서 계산하고 리턴
               l = postOrder(2*node)
               r = postOrder(2*node+1)
               tree[node] = l+r
               return tree[node]
       else:
           return 0
   
   N, M, L = map(int, input()) # 노드수, 리프수, 출력노드
   tree = [0]*(N+1)
   
   for i in range(M):
       idx, value = map(int, input().split())
       tree[idx] = value
       
   postOrder(1)
   ```

   