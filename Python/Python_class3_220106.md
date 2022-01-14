# Python

## 다중상속

> 여러 클래스에서 상속을 받는 상태. 즉 부모 클래스가 여러개인 경우



- 형식: class 클래스이름 (부모클래스1, 부모클래스2, ...)

```python
class Person:
	def __init__(self, name, age = 0):
        self.name = name
        self.age = age
        
    def greeting(self):
        print(f'안녕하세요 {self.name}')

class University:
    def __init__(self, department):
        self.department = department
        self.grade = grade
    
    def manageScore(self):
        print('학점관리')
        
class Undergraduate(Person, University): # 두개의 클래스로부터 상속
    def study(self):
        print('공부')
```



- 만약 다이아몬드모양처럼, a가 최상위클래스, b와 c가 a의 자식클래스, d가 b와 c의 자식클래스라면? overriding의 경우에 어떤 클래스의 함수를 호출할까?

```python
class A:
	def greeting(self):
        print('A')

class B(A):
	def greeting(self):
        print('B')    

class C(A):
	def greeting(self):
    print('C') 
    
class D(B,C): #여기가 만약 C가먼저였다면 결과값이 'C'가 나옴
    pass

d1 = D()
d1.greeting() 
결과 : 'B'가 나옴
    
```

:heavy_check_mark:클래스.mro() : 메서드 호출순서를 확인 가능. ;method resolution order



## 정적 메서드(static method) & 클래스 메서드(class method)

- 정적 메서드 : 
  - 인스턴스를 통하지 않고 클래스에서 바로 호출해서 사용
  - 메서드 위에 @staticmethod를 붙이고 self를 삭제한다 ; 인스턴스 변수와 메서드가 불필요하기 때문에
  - 클래스의 용도가 순수 함수만 사용일 때 유용

```python
class Calc:
    @staticmethod
    def add(a,b):
        print(a + b)
    
    def mul(a,b):
        print(a * b)

calc1 = Calc() #이제 이거 불필요
Calc.mul(5,6)
```



- 클래스 메서드:
  - 정적메서드와 유사하지만, 클래스 메서드는 클래스의 첫번째 인자가 'cls'임. (반드시)

```python
class Person:
    count = 0
    
    def __init__(self, name):
        self.name = name
        Person.count += 1 #클래스 변수의 count를 추가하기 때문에 인스턴스가 여러개더라도 셀 수 있음!

    @classmethod
    def printCount(cls):
        print(f'{Person.count}명이 태어났습니다') 
        #여기는 self.count든 person.count든 정상적으로 작동
        
man1 = Person('Kim')
man2 = Person('Lee')
Person.printCount()

결과 : 2명이 태어났습니다
```



## 기타

- 하위클래스에서 \_\_init\_\_메서드가 없다면 상위 클래스의 \_\_init\_\_메서드가 호출되어 상위 클래스의 속성을 자유롭게 사용가능. 즉 \_\_init\_\_는 필수가 아님!
- but, 하위클래스에서 \_\_init\_\_메서드가 호출되었다면 super().\_\_init\_\_를 작성해야함
- super().\_\_init\_\_는 상위클래스의 인자순서와 같게 작성해야함
- object는 모든 클래스의 부모 클래스로서 작성 유무는 관계없음
  - 예) class Student(object)
- 비공개 변수 클래스는 외부에서 사용 가능:
  - @property (aka 데코레이터)를 활용하면 숨겨진 변수를 반환시킴



