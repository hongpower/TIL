# Python 

## 함수 :

> 12월 29일 복습 내용

mutable : list & dictionary -> 수정을 하는 함수를 시행했을 때 복사를 하지 않고 원본을 수정함. myList.appned('new')를 작성하면 즉 원본이 수정됌.

만약 shallow copy를 피하고 싶다면 함수 내에서 복사해서 한번더 지역변수를 변경하는 식으로 진행( 예를 들어서, myList = [100, 200, 300])

immutable : 매개변수의 인수로 가져오게 되면 수정을 가하는 함수더라도 복사본을 만들어 복사본을 수정함.



### 팩토리얼 계산

1. 반복문 사용:

   ```python
   def factorial(n):
       fac = 1
       for i in range(n, 0, -1):
           fac *= i
       return fac
   ```

   

2. 재귀 함수 사용 :

```python
def factorial(n):
    if n == 1:
        return 1
    else:
        return n * factorial(n-1)
```

- RecursionError : 자기호출 에러의 일종으로, limit없이 시행할시 나는 오류



### 내장함수(built-in functions)

- abs() : 절대값
- all([iterable]) : 모든 요소가 참이면 True, 하나라도 거짓이면 False

:heavy_check_mark:iterable은 for 반복문을 활용해서 요소 하나하나를 작업가능한 자료형. 리스트, 튜플, 딕셔너리, 집합이 여기 해당됨

:heavy_check_mark:0은 False, 0을 제외한 값은 True

- any([iterable]) : 하나라도 참이면 True
- bool() : True/False로 변환. None, 빈문자열, 빈리스트, 빈딕셔너리 등 빈 자리인 경우에 False 반환
- chr() : 아스키코드 값에 해당하는 문자 반환
- ord() : 문자에 대한 아스키 코드 값 반환
- divmod(a,b): a를 b로 나눈 몫과 나머지를 튜플 형태로 (몫, 나머지) 이렇게 반환
- enumerate() : 시퀀스의 각 값에 인덱스 값에 포함해서 enumerate 객체로 반환

:heavy_check_mark: 시퀀스 : range(), 문자열, 리스트, 튜플

```python
for i, c in enumerate('hello'):
    print(i, c)
    
0 h
1 e
2 l
3 l
4 o
```

enumerate 객체로 반환하기 때문에 앞에 i 와 c처럼 변수나 type을 지정해 줘야함.

- eval(표현식): 표현식의 연산 결과 정수형으로 반환. 즉 문자열로 되어있는 계산식을 계산해줌
- filter(function, iterable): iterable자료의 요소들을 function으로 걸러냄

```python
def positive(x):
    return x>0

print(list(filter(positive,[0, 1, -1, 10])))
```

- help(): 도움말 불러오기
- pow(x,y): x의 y승
- range(start,stop,step) : 지정한 범위의 값을 반복 가능한 객체로 반환

```python
print(range(0,5))
print(list(range(0,5)))
```

:heavy_check_mark: range도 앞에 iterable을 지정해줘야함! 위 식에서 만약 list없이 수행했다면 'range(0,5)'가 그대로 출력됨

- map(함수, 데이터형(리스트 or 튜플)) :iterable한 데이터 형의 요소별로 지정된 함수를 적용하여 처리. 원본 변경은 하지 않으며 list, tuple 형태로 반환

```python
numbers = [1, 3.5, 7, 8]

print(list(map(int,numbers)))
```



:heavy_check_mark:filter와 다른 점은 filter는 함수를 활용해서 조건에 맞는 것만 걸러내고, map은 모든 요소의 결과값을 불러냄.

- zip(iterable1, iterable2) : iterable에서 동일한 인덱스 요소를 추출하여 묶어서 반환

```python
print(list(zip([1,2,3],[4,5,6])))

[(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```



### Lambda(람다) 함수

> 함수를 한줄로 간단하게 작성 가능케하는 함수

- 형식 : 변수(인수들) = lambda 매개변수들 : 식

<기존의 방식>

```python
def add(x, y):
    result = x + y
    return result
```

<람다>

1.

```python
add_up = lambda x,y : x + y
```

-> 훨씬 간단하고 일회용적인 함수를 만들어서 사용할 때 유용!

2.

```python
add_up2 = lambda x=10, y=20 : x + y
```

-> 이렇게 매개변수에 기본값을 설정해서도 가능

3.

```python
multipy = (lambda x : x * 10)(5)
```

-> (lambda 매개변수들 : 식)(인수들)

이런식으로 인수가 매개변수 값으로 들어가도록 설정도 가능

대신 이렇게 되면 2번 방법 처럼 표현식안에 변수 생성( 예 : x = 10)은 불가

- lambda & map

```python
num2 = [1, 2, 4, 10]
num3 = list(map(lambda num : num + 10, num2)) 
```

:heavy_check_mark: num3안에 있는 'num'이라는 변수는 lambda의 변수이므로 __map과 함께 쓰게 되면, map의 두번째 변수가 lambda의 변수가 되는 것임__ 즉, num말고 x를 쓰든 무엇을 쓰든간에 num2를 인자로 받게 됨



## 모듈(Module)

1. 모듈 전체 참조 : import 모듈명
2. 모듈 내에서 일부 참조(추천): from 모듈명 import 변수명 or 함수명

​	ex) from random import randint, randrange

3. 스페셜 변수/매직 메서드를 제외한 모든 것을 참조 : from 모듈명 import *
