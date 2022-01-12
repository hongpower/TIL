# 자료구조

## 선형 구조:

### 원형큐(Circular Queue):

:question: 만약에 일반 큐에서 rear만 SIZE-1(뒤는 공간이 없음)이고, front가 -1이 아니라면:

​	뒤는 꽉 찼지만 앞에는 공간이 필요하다는 뜻이여서, 뒤에 큐들을 앞으로 땡겨야 한다.

​	but 이것의 문제점은 큐의 개수가 수만개, 수십만개라면 '오버헤드'가 발생할 수 있다.

​	-> 이 문제를 해결할 수 있는게 원형 큐이다!

​	예를 들어 rear가 99999(마지막 큐)번째 큐의 위치일 때, 원형 큐는 rear + 1만 하면 0번째로 돌아와서 바로 새로운 요소를 추가할 수 있기 때문이다! --> 한칸을 낭비

- 일반 큐와 마찬가지로 FIFO(First In, First Out) 구조

- __원형큐에서는 항상 공백이 최소 1개는 존재해야한다__ ; 그래야 배열이 꽉 차있는지 아닌지 구분할 수 있어서

  - 그래서 만약 enQueue를 하다가 배열이 꽉차면(1개의 공백) enQueue는 더이상 수행 못한다

  

- 원형 큐 초기화 :
  - 원형큐 = [None * 5]
  - front = rear = __0__ (-1의 위치는 존재하지 않기 때문에)
  
  
  
- 원형 큐가 빈 경우 : front와 rear가 동일할 때(일반 큐와 동일), 꽉찬(포화상태) 경우는 고려 X

:heavy_check_mark: 원형 큐가 공백이거나 포화일 때 front와 rear는 동일하다

- 원형 큐가 꽉 찬 경우 (front가 빈칸이고 나머지가 다 차있으면 원형 큐가 꽉 찼다고 간주함) : rear와 front의 차이가 1일 때 ((rear + 1) = front)
  - 만약에 front가 0이고 rear가 SIZE-1이라면? 이것도 같은 경우인데 조건에 안맞기 때문에 : _(rear + 1) % 5 = front_ 이렇게 작성!

```python
## 함수
def isQueueEmpty():
    global SIZE, queue, front, rear
    if rear == front:
        return True
    else:
        return False

def isQueueFull():
    global SIZE, queue, front, rear
    if ((rear+1)%SIZE == front):
        return True
    else:
        return False

def enQueue(data):
    global SIZE, queue, front, rear
    if (isQueueFull()):
        print('Queue가 꽉 찼음')
        return
    rear = (rear + 1) % SIZE
    queue[rear] = data

def deQueue():
    global SIZE, queue, front, rear
    if (isQueueEmpty()):
        print('Queue에 남은 것이 없음')
        return None
    front = (front + 1) % SIZE
    data = queue[front]
    queue[front] = None
    return data

def peek():
    global SIZE, queue, front, rear
    if (isQueueEmpty()):
        print('Queue에 남은 것이 없음')
        return None
    return queue[(front + 1) % SIZE] 
# 중요중요! peek는 실제 front값을 바꾸면 안되기 때문에 deQueue는 front를 바꿔서 그것을 인덱스로 넣었지만 peek에서는 실제 front값을 바꾸지 않도록 식을 작성!

## 전역
SIZE = 6
queue = [None for _ in range(SIZE)]
front = rear = 0 # front와 rear는 circular queue에서는 0이다

## 메인
enQueue('지윤')
enQueue('지수')
enQueue('재욱')
enQueue('찬혁')
enQueue('한나')
print(queue)
deQueue()
print(queue)
deQueue()
print(queue)
deQueue()
print(queue)
enQueue('병규')
print(queue)
enQueue('김설')
print(queue)
deQueue() 
print(queue)

# 결과값:
# [None, '지윤', '지수', '재욱', '찬혁', '한나']
# [None, None, '지수', '재욱', '찬혁', '한나']
# [None, None, None, '재욱', '찬혁', '한나']
# [None, None, None, None, '찬혁', '한나']
# ['병규', None, None, None, '찬혁', '한나']
# ['병규', '김설', None, None, '찬혁', '한나']
# ['병규', '김설', None, None, None, '한나'] --> deQueue()를 했을 때 원형 모양이 아니라서 '병규'가 삭제될 것 같지만 실제로는 선입이 되었던 '찬혁'이가 추출된다!
```



## 비선형 구조:

### 트리:

- 루트 : 최상위 부모노드
- 에지 : 노드와 노드를 잇는 선
- 차수 : 해당 노드 바로 밑 자식 노드 개수 (이진 트리에서는 최대 2개)



#### 이진트리:

- 이진트리는 모든 노드의 자식이 최대 2개

- 서브트리는 내 자식의 자식 까지도 합침!

- 포화 이진 트리(full binary tree) : 자리마다 꽉 참, 가로로 왼쪽에서 오른쪽으로 번호 부여

- 완전 이진 트리(complete binary tree): 이빨이 빠졌지만 번호는 맞음

- 일반 이진 트리 : 이빨도 빠졌고 번호도 틀림

- 편향 이진 트리(skewed binary tree) : 한쪽으로만 쏠린 tree 

- 이진트리는 링크가 2개.

  

#### 이진 탐색 트리(*):

- 활용도가 높은 트리로 탐색을 위해 쓰임

- 루트의 왼쪽노드는 루트보다 작고, 오른쪽노드는 루트보다 큼

  ```python
  ## 함수
  class NodeTree():
      def __init__(self):
          self.left = None
          self.right = None
          self.data = None
  
  ## 전역
  root = current = None
  dataArray = ['지수','병규','한나','찬혁','제훈','정현']
  memory = []
  
  ## 메인
  # root 생성
  tree = NodeTree()
  tree.data = dataArray[0]
  root = tree
  memory.append(tree)
  
  # 나머지 시행
  current = root
  for name in dataArray[1:]:
      tree = NodeTree()
      tree.data = name
      while True:
          if name < current.data :
              if current.left == None:
                  current.left = tree
                  break
              current = current.left
  
          else :
              if current.right == None:
                  current.right = tree
                  break
              current = current.right
  
      memory.append(tree)
  
  # 데이터 찾기:
  findName = '미영'
  current = root
  while True:
      if current.data == findName:
          print('찾았음')
          break
      elif current.data > findName:
          if current.left == None:
              print('없음')
              break
          current = current.left
      else:
          if current.right == None:
              print('없음')
              break
          current = current.right
          
  #--> 결과값
  없음
  
  ```

  :heavy_check_mark: 참고로 문자로 이진트리를 표현하면 '가나다라...' 오름차순으로 이진트리 구성!



### 그래프:

- 여러 데이터가 서로 다 연결되어있음
- 그래프는 루트도 노드라는 개념도 없음.
- 지하철 노선도, ktx노선도 등
- Vertex(정점): 노드대신 Vertex 존재

#### 1. 무방향 그래프

```python
# 정점 집합
V(G1) = {A,B,C,D}
V(G2) = {A,B,C,D}

# 간선 집합
E(G1) = {(A,B),(C,D)...}
E(G2) = {(A,B),(C,D)...}
```

- 가중치 그래프 : 간선마다 가중치가 다르게 부여된 그래프

- 그래프의 탐색 : 1) 깊이우선 2)너비우선

  - 탐색 : 모든 정점을 한번씩 방문하는 것

  

  ##### 인접 행렬: 정점의 개수를 가지고 생성한 정방형 행렬
  - 정점이 4개라면 4*4 그래프 준비한다. 
  - 자신이 자신과 만나는 점은 선이 100%없으니 0으로 채운다
  - 그리고 a행부터 d행까지 내려오면서 차례차례 0 또는 1을 채운다. 연결이 되면 1, 안되면 0을 채운다.
  - 인접 행렬을 보고 그래프를 그릴 수 있을 정도로 해석 가능해야함

- 무방향 그래프의 인접 행렬은 대각선을 기준으로 서로 대칭됨.

```python
## 함수
class Graph():
    def __init__(self,size):
        self.size = size
        self.graph = [[0 for _ in range(size)] for _ in range(size)] # 2차원 행렬 생성

## 전역
G = None # stands for graph

## 메인
# 그래프 채워넣기:
G = Graph(4)
G.graph[0][1] = 1 # 1행 2열 채움. 1행 1열은 자기자신이니까 넘어감!
G.graph[0][2] = 1
G.graph[0][3] = 1

G.graph[1][0] = 1
G.graph[1][2] = 1
G.graph[1][3] = 1

G.graph[2][0] = 1
G.graph[2][1] = 1
G.graph[2][3] = 1

G.graph[3][0] = 1
G.graph[3][1] = 1
G.graph[3][2] = 1
# G의 graph의 모습은:
# --> [[0, 1, 1, 1], [1, 0, 1, 1], [1, 1, 0, 1], [1, 1, 1, 0]] --> 2차원 행렬!

#그래프의 요소들을 하나씩 꺼내서 정렬!
for row in range(4):
    for col in range(4):
        print(G.graph[row][col],end=' ')
    print()
    
# 결과:
0 1 1 1 
1 0 1 1 
1 1 0 1 
1 1 1 0 
```

#### 2. 방향그래프

- 간선에 방향이 있는 그래프

#### 3. 가중치 그래프

- 간선마다 가중치가 다르게 부여된 그래프

그래프 순회란? 모든 정점을 한번씩 방문



# 알고리즘

## 정렬(Sort):

#### 선택 정렬(*) : 

- 여러 데이터 중에서 가장 작은 값을 뽑는 작동을 반복해서 값을 정렬

  1. 첫번째 값을 가장 작은 값으로 지정한 후 비교

  2. 마지막 값까지 비교를 하는데 만약 중간에 더 작은값이 생기면 걔를 젤 작은값으로 변경하고 계속 진행

     :heavy_exclamation_mark: 정렬한 데이터는 다른 방을 만들어서 따로 진행!

```python
  # 선택 정렬1 : 새로운 방 만들기
  import random
  ## 함수
  def findMinIndex(array):
      minIdx = 0
      for i in range(1, len(array)):
          if array[minIdx] > array[i]:
              minIdx = i
      return minIdx
  
  ## 전역
  before = [random.randint(100,201) for _ in range(8)]
  after = []
  
  # 메인
  print('정렬 전 : ',before)
  
  for _ in range(len(before)):
      minPos = findMinIndex(before)
      after.append(before[minPos])
      del(before[minPos])
  
  print('정렬 후 : ',after)
  
  # 결과 값:
  정렬 전 :  [148, 162, 198, 196, 100, 170, 142, 100]
  정렬 후 :  [100, 100, 142, 148, 162, 170, 196, 198]
  
```



#### 계산된 선택 정렬:

:heavy_exclamation_mark: 만약 새로운 방을 만들지 않고 그 방의 내용들을 정렬하고 싶다면? 

```python
import random
## 함수
def selectionSort(array):
    n = len(array)
    for circle in range(n-1): # 한 서클
        minIndx = circle
        for i in range(circle + 1, n):
            if array[minIndx] > array[i]:
                minIndx = i
        array[minIndx], array[circle] = array[circle], array[minIndx] # 한 서클이 끝나면 한 초기 min값과 변경된 min값의 위치를 바꾼다!

## 전역
dataArray = [random.randint(0,10) for _ in range(5)]

## 메인
print('전 : ',dataArray)
selectionSort(dataArray)
print('후 : ',dataArray)
```



이외에도 삽입정렬, 버블정렬, 퀵정렬 등이 있지만 가장 중요한건 선택 정렬!



## 검색(Search):

#### 1. 순차 검색(sequenceSearch): 정렬되지 않은 뒤죽박죽 섞인 자료에서 검색할 때

- 검색에 성공 : 검색 데이터를 하나씩 비교하면서 찾으면 인덱스 반환

- 검색에 실패 : 차례로 찾는데 검색 데이터가 존재하지 않을 때; 실패하면 관례적으로 '-1'을 반환

```python
# 순차 검색
import random
## 함수
def seqSearch(ary, fdata):
    n = len(ary)
    pos = -1
    for i in range(n):
        if ary[i] == fdata:
            pos = i
            break # for 구문에도 break 사용 가능
    return pos

## 전역
size = 10
dataAry = [random.randint(1,101) for _ in range(size)]
findData = dataAry[random.randint(0,size-1)] #randint는 이상 이하지?

## 메인
print('배열 : ',dataAry)
position = seqSearch(dataAry, findData)
if position == -1:
    print('데이터 없음')
else:
    print(findData,'이 ',position,'에 있음')
```



#### 2. 이진 검색(binarySearch): 정렬된 상태에서 찾을 때

- 전체를 반씩 잘라 내서 한쪽을 버리는 방식, 굉장히 빠름 ; O(log n)
- __조건 : 반드시 정렬이 된 상태에서 진행 !!__

- 중앙까지 포함해서 버리기 ! ( + 1 or - 1을 해서)

- 시작과 끝을 합쳐서 /2 하고 소수면 버림!

- 시작과 끝이 같거나 시작이 끝보다 작을 때는 계속 진행하지만

  만약 끝이 먼저, 시작이 후에 나오면 실패인 -1을 반환한다

- def binSearch() 짤 때 항상 'pos=-1'하고 return pos'부터 작성하자!

```python
import random
## 함수
def binSearch(ary,fdata):
    pos = -1 
    start = 0
    end = len(ary)-1 #인덱스니까
    while start <= end: # start와 end가 위치가 반대가 되면 fail한거니까
        mid = (start + end) //2
        if ary[mid] == fdata:
            return mid
        elif fdata > ary[mid]: # fdata가 더 크면 mid를 포함한 왼쪽 부분은 다 버려야하니, start를 뒤로 땡겨야함
            start = mid + 1
        else: # fdata가 더 작으면 mid를 포함한 오른쪽 부분은 다 버려야하니, end를 앞으로 땡겨야한다
            end = mid - 1
    return pos

## 전역
size = 10
dataAry = [random.randint(1,101) for _ in range(size)]
findData = dataAry[random.randint(0,size-1)] #randint는 이상 이하지?
dataAry.sort() # 정렬!! 필수 !! 필수 !! 필수 !!

## 메인
print('배열 : ',dataAry)
position = binSearch(dataAry, findData)
if position == -1 :
    print(findData, '없음')
else:
    print(findData, '가 ',position,'에 있음')
```





## 재귀(Recursion):

> 상자를 반복해서 여는 과정과 유사. 함수 안에 함수가 있는 구조

- 무한 반복을 하는 경우(while True) 빠져나올 수 있는 조건이 무조건 필요!
  1) 상자 열기:

```python
def openBox():
    global n
    print('상자를 열었다')
    count -= 1
    if count == 0:
        ('프로포즈 반지를 넣었다!')
        return
    openBox() # 만약 count가 0이 아니라면 밑에 상자를 닫는 구간까지 가기 직전까지 계속 수행 그러다가 return을 만나면 맨 최근 openBox()함수가 종료되고 print('상자를 닫는다') 밀린거 다 수행
    print('상자를 닫는다')

n = 10 #박스의 개수
openBox()
```



# Python

- set(iterable) : iterable을 set으로 만든다

  :heavy_check_mark: set은 중복을 허용하지 않고 순서가 없기 때문에 이렇게도 활용가능:

  ```python
  lst = []
  for i in range(10):
  	lst.append(int(input())%42)
  print(len(set(lst)))
  ```

  
