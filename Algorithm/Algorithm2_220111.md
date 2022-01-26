# 자료구조

> 자료를 효율적으로 관리하는 방법

| 단순 자료구조 | 선형 자료구조 | 비선형 자료구조 | 파일 자료구조 |
| ------------- | ------------- | --------------- | ------------- |
| 정수          | 리스트(배열)  | 트리            | 순차 파일     |
| 실수          | 스택          | 그래프          | 색인 파일     |
| 문자열        | 큐            |                 | 직접 파일     |

- 선형 자료구조 : 

  - 데이터를 한 줄로, 일정한 순서대로 나열한 자료구조
  - 선형 리스트, 단순 연결 리스트, 원형 연결리스트, 스택, 일반 큐, 원형 큐
- 비선형 자료구조:

  - 하나의 데이터 뒤에 여러 데이터가 이어지는 형태
  - 트리, 그래프
- 순차파일(Sequential File):

  - 파일 내용을 논리적인 처리 순서에 따라 연속해서 저장
  - 공간 효율은 좋지만, 삭제하면 파일 구성을 다시해야하기 때문에 시간 소요가 높다
- 직접파일(Direct File):

  - 파일 내용을 물리적 위치에 기록하는 방식
  - '해시 함수'라는 계산식으로 내용을 저장하는 위치를 결정한다
- 색인순차파일 :

  - 순차 파일과 직접 파일이 결합된 자료구조




## 선형 자료구조:

### 1. 선형(순차) 리스트(Linear List)

- 데이터를 일정한 순서로 나열한 리스트(여기서의 리스트는 Python의 리스트와 다름)
- 각 데이터의 위치는 순차적이다
- __삽입과 삭제 때 오버헤드 발생__ 가능성이 있다
- 링크없이 데이터만 있으니 공간 차지가 적다는 장점이 있다
- 접근속도도 더 빠름.(각 데이터가 링크 없이 붙어있어서)
- 얘네는 삽입 삭제가 없는 데이터에 적당!
  - 예) 시계열 데이터(시간 순서로 생성) : 신문의 날짜별 기사, 고전 소설 연대별
- 변화를 주기 전에 항상 빈 공간(None)을 만들어야한다!

```python
## 함수
# 베프 리스트 추가
def add_bf(friend):
    bf.append(None) # 빈칸 추가
    bfLen = len(bf)
    bf[bfLen-1] = friend

# 베프 리스트 중간에 데이터 삽입
def insert_bf(friend,position):
    bf.append(None)
    bfLen = len(bf)
    for i in range(bfLen-1,position,-1): # for i in range(4,4,-1)이니까 그냥 실행하지 않고 넘어간다
        bf[i] = bf[i - 1]
        bf[i - 1] = None
    bf[position] = friend
    return

# 베프 리스트 내의 데이터 삭제
def delete_bf(position):
    bf[position] = None
    bfLen = len(bf)
    for i in range(position+1,bfLen):
        bf[i - 1] = bf[i]
        bf[i] = None
    del(bf[-1])

## 전역
bf = []

## 메인
add_bf('병규')
add_bf('한나')
add_bf('찬혁')
add_bf('재욱')
insert_bf('김설',4)
print(bf)
delete_bf(0)
print(bf)
delete_bf(2)
print(bf)
```



### 2. 단순 연결 리스트(Linked List)

- 논리적으로는 붙어 있지만, 물리적 or 실제로는 떨어져있음
- 노드로 표현한다. 노드 : 데이터 + __링크__(화살표)
- 각 노드의 위치도 순차적이지 않고 그저 '링크'로 순서가 표현된다

:question: 만약 데이터가 천만개가 있는데, 선형 리스트에서 2등을 삭제하면 3등부터 천만명까지 앞당겨야하는 번거로운 일이 생길 수 있음. 그래서 오버헤드(되기는 되는데 무리하게 실행)가 발생할 가능성이 있는데, 이때! 단순 연결 리스트를 사용하면 오버헤드 안생긴다

- head변수 : 제일 첫번째 노드
- current변수 : 현재 작업하는 노드
- pre변수 : current이전 노드

- 선형리스트의 함수: 노드출력함수, 노드추가함수, 노드삭제함수, 노드확인함수

 ```python
 ## 함수
 # 클래스 선언:
 class Friend():
     def __init__(self):
         self.data = None
         self.link = None
 
 # 노두 출력 함수 : 원하는 위치부터 친구 리스트 소환
 def show_friend(start):
     global memory, head, current
     current = start
     if current == None:
         return
 
     print(current.data,end=' ')
     while current.link != None:
         current = current.link
         print(current.data, end=' ')
 
 # 노두 추가 함수 : 원하는 위치에 친구 삽입
 def add_friend(searchName,insertName): #searchName 앞에 insertName추가
     global memory, head, current, pre
 
     # 첫번 째 노드에 삽입할 때:
     if searchName == head.data:
         friend = Friend()
         friend.data = insertName
         friend.link = head
         head = friend
         return
 
     # 두번째 노드 이후 앞에 삽입할 때:
     current = head
     while current.link != None:
         pre = current
         current = current.link
         if current.data == searchName:
             friend = Friend()
             friend.data = insertName
             friend.link = current
             pre.link = friend
             return
 
     # findData가 없어서 insertData를 마지막에 추가할 때 :
     friend = Friend()
     friend.data = insertName
     current.link = friend
     return
 
 # 노드삭제 함수
 def delete_friend(name):
     global memory, head, current, pre
 
     # 삭제하고싶은 친구가 head일때:
     if name == head.data:
         current = head
         head = head.link
         del(current)
         return
 
     # 아닌 경우에:
     current = head
     while current.link != None:
         pre = current
         current = current.link
         if current.data == name:
             pre.link = current.link
             del(current)
             return
 
 # 노드 확인 함수 : 사용자가 원하는 친구 node가 존재하면 반환하고 없으면 빈 노드를 반환
 def find_friend(findName):
     global memory, head, current, pre
 
     # 헤드라면
     current = head
     if findName == current.data:
         return current
 
     while current.link != None:
         current = current.link
         if current.data == findName:
             return current
     return Friend() # 빈노드 반환
 
 
 ## 전역
 memory = []
 head, current, pre = None, None, None
 friendList = ['병규','한나','찬혁','재욱','김설','정현']
 
 ## 메인
 friend = Friend()
 friend.data = friendList[0]
 head = friend
 memory.append(friend)
 
 for i in friendList[1:]:
     pre = friend
     friend = Friend()
     friend.data = i
     pre.link = friend
 	memory.append(friend)
 show_friend(head)
 
 add_friend('병규','희지')
 show_friend(head)
 add_friend('희지','제훈')
 show_friend(head)
 add_friend('미영','수성')
 show_friend(head)
 
 delete_friend('희지')
 show_friend(head)
 delete_friend('병규')
 show_friend(head)
 
 goneFriend = find_friend('제훈')
 
  #노드의 link(화살표)는 노드(이름 + 화살표)를 가르킴!
 ```

참고 : ';' 쓰면 한줄에 쓸 수 있음



### 3. 원형 연결 리스트:

- 단순 연결 리스트와 비슷하지만 원 형태라서 계속 회전. 즉 마지막 노드의 링크는 head를 가리킨다
- 삭제,추가 등 왠만한 함수는 일반 연결 리스트와 비슷하다
- 다른 점은 새로운 노드가 만약 마지막 노드라면, head를 가리키는 링크를 또 추가해야한다는 것
- 잘 쓰는 리스트는 아니다



### 4. 스택:

- 선입후출

- top이 -1이면 스택이 하나도 없다; 데이터가 없다
- push : stack의 top인덱스에 데이터를 추가한다
- pop : stack의 top인덱스에 데이터를 삭제하지만, delete와 다른 점은 삭제 된 데이터를 특정 변수를 저장/이용

- 스택이 많이 꽉 찼는데 데이터를 추가하면 (즉 SIZE를 초과하게 데이터를 push하는 경우) 오버플로우 일어난다

- 그래서 꼭!꼭!꼭! __'스택이 꽉 차지 않았을 경우'__를 조건으로 걸고 push를 해야한다 -> by additionally defining 'isStackFull()' function
- 반대로 스택이 텅비었는데 pop을 하지 않도록 __'스택이 텅비지 않았을경우'__를 조건으로 걸고 pop을한다 -> by additionally defining 'isStackEmpty()' function
- peek() : 만약 추가로 pop을 수행한다면 어떤 스택이 추출될지 정보만 보여주기

```python
## 함수
def isStackFull():
    if n-1 == top: #if top >= n-1 이렇게 정의해도 괜찮다
        return True
    else:
        return False

def push(flavor):
    global n, stack, top
    if (isStackFull()):
        print('아이스크림은 더이상 못쌓아요')
        return
    top += 1
    stack[top] = flavor
    return

def isStackEmpty():
    if top == -1:
        return True
    else:
        return False

def pop():
    global n, stack, top
    if (isStackEmpty()):
        print('아이스크림이 텅 비었어요')
        return
    trash = stack[top]
    stack[top] = None
    top -= 1
    return trash

def peek():
    if (isStackEmpty()):
        print('아이스크림이 텅 비었어요')
        return
    print(stack[top])

## 전역
n = 4
stack = [None for _ in range(n)]
top = -1

## 메인
top += 1
stack[top] = 'mintchoco'
top += 1
stack[top] = 'strawberry'
push('vanilla')
pop()
pop()
print(stack)
peek()
```



### 5. 큐:

- 선입선출/삽입추출/ 양쪽이 뚫린 구조가 특징

- enQueue : 데이터 추가 (스택의 push)

- deQueue : 데이터 제외 (스택의 pop). pop과 마찬가지로 제외한 데이터를 별개의 변수에 저장한다

- 스택과 가장 다른 점은 top대신에 front & rear라는 두 변수를 사용한다. 초기값은 둘다 -1이다

- front : 저장된 데이터 중 첫번 째 데이터의 앞, 앞에 데이터를 삭제할 때 사용한다

- rear : 저장된 데이터 중 마지막 데이터, 데이터를 추가할 때 사용한다

- rear가 SIZE-1과 같다면 큐가 꽉 차있다는 뜻이다 ->isQueueFull()

  - BUT, 여기서 만약 앞쪽이 비어있고 뒤에가 꽉찼다면? 그래서 추가로 isQueueFull()에 코드 작성이 필요.

  ```python
  def isQueueFull():
  	# queue가 full이 아닐 때 먼저 정의:
  	if rear != SIZE-1:
          return False
      # queue가 확실히 full일 때를 정의 : rear가 마지막 위치에 있는 동시에 앞에도 자리가 없을 때
      elif (rear == SIZE-1) & (front == -1):
          return True
      # queue가 empty지만 rear가 마지막 위치에 있어서 자료를 추가 못하는 것을 방지하기 위해 앞으로 땡기기
      else:
          for i in range(front + 1,SIZE):
              queue[i-1] = queue[i] # 맨 앞 데이터부터 끝까지 한칸 앞으로 땡기기
              queue[i] = None
          front -= 1 # front 수정
          rear -= 1 # rear 수정
          return False        
  ```

  

- __front와 rear의 숫자가 같으면 큐가 비어있다__는 뜻이다 -> isQueueEmpty()

- peek()는 그 다음으로 삭제될 친구를 조회하게 해준다

  

```python
## 함수
def isQueueFull():
    if (n-1 == rear) & (front == -1):
        return True
    elif rear != SIZE-1:
        return False
    else:
        for i in range(front+1,SIZE):
            queue[i-1] = queue[i]
            queue[i] = None
        front -= 1
        rear -= 1
        return False

def enQueue(cosmetic):
    global n, queue, front, rear
    if (isQueueFull()):
        print('화장품 칸이 다 찼습니다')
        return
    rear += 1
    queue[rear] = cosmetic
    return

def isQueueEmpty():
    if rear == front:
        return True
    else:
        return False

def deQueue():
    global n, queue, front, rear
    if (isQueueEmpty()):
        print('화장품 칸에 화장품이 없습니다')
        return None 
    front += 1
    data = queue[front] #마음대로 버리면 안된다!
    queue[front] = None
    return data # 이거 까먹지말기

def peek(): # 그 다음 삭제될 친구
    global n, queue, front, rear
    if (isQueueEmpty()):
        print('화장품 칸에 화장품이 없습니다')
        return
    return queue[front + 1]  # 왜냐하면 front가 있는 곳은 empty기 때문에

## 전역
n = 6
queue = [None for _ in range(n)]
front = rear = -1

## 메인
enQueue('Lotion')
enQueue('Foundation')
enQueue('Mascara')
enQueue('Lipgloss')
deQueue()
enQueue('Lipstick')
print(queue)
print(front)
```

:white_check_mark: enQueue에서는 return을 함수를 종료하기 위한 목적으로만 쓰였는데, deQueue에서 return None을 하는 이유: deQueue를 통해 삭제된 데이터를 앞서 특정 변수에 저장한다고 말했다. 통일감을 주기 위해, 화장품 칸에 화장품이 없는 경우에도(삭제할 친구가 없는 경우에도) 값을 return하기 위해서 None을 작성한 것이다. 



# 알고리즘

> 특정 문제를 단계적으로 해결해 가는 논리적인 과정이다. 

- 의사코드 표현법 : 프로그래밍 언어와 일반 언어의 중간 형태



## 알고리즘의 성능

> 알고리즘의 소요 시간이 얼마나 걸렸는지 시간 복잡도(Time complexity)를 통해 표현한다. 하지만 여기서 말하는 '시간'은 일반적인 시간이 아니라 알고리즘이 수행하는 계산 수를 칭한다고 보는 것이 맞다. 컴퓨터마다 성능이 다르기 때문에 정확한 시간의 수치를 파악하기는 어렵기 때문이다.

### 빅-오 표기법(Big-Oh Notation)

- O(f(n)) 형태로 표기
- 대표적인 함수: O(1), O(log n), O(n) 등
- O(1) : y, 즉 시간이 1이라서 n이 몇개든 시간1로 유지한다는 뜻
- O(log n) : n개수의 비례하긴 하지만 비례 정도가 굉장히 낮다
- 데이터 개수에 따라 , 또 프로그래밍을 하는 데에 소요되는 시간을 고려해서 제일 적절한 방법을 선택한다



# Python

- if (a == 0) & (b== 0) --> if a == b == 0 이렇게 표현 가능
- runtime Error가 떴을 때, try, except 구문은 while문에도 쓸 수 있음

```python
while True:
    try:
        a, b = map(int,input().split())
        print(a + b)
    except: #에러가 뜨면 break
        break
```



