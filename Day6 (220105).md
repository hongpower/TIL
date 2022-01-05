# Python

## Class

- 생성자(\_\_init\_\_)

- 생성자 형식 :

  ```python
  class 클래스명:
  	def __init__(self, *args): #이렇게 init을 쓰면 인스턴스 생성하면서 속성도 지정 가능!
  		# 필드 초기화 코드 입력
  ```

  

- self: 클래스에서 생성된 인스턴스 즉 자기 자신에 접근.

- 기본 생성자 : 생성자에 self만 있고 다른 매개변수가 없는경우

  

- '_' : 변수에 특별한 이름을 부여하지 않고 사용할 때 :

  - for _ in range 이렇게 작성도 가능

- '__': 특수한 목적에 의해서 만들어진 함수/변수에 사용



- isinstance('인스턴스명','클래스명') : 인스턴스가 클래스의 인스턴스인지 확인



- 필드값은 직접 정의할 수 있지만, 메소드를 통해서 간접적으로 필드값을 변경/조회할 수 있도록 설정하는걸 권고.
- 필드값 변경 메소드 : 

```
def setModel(self, modelname):
	self.model = modelname
```

- 필드값 조회 메소드:

```
def getModel(self):
	return self.model
```



- 비공개 필드 : 필드를 외부에서 직접 사용하고 보지 못하도록 하는 방법. 즉 클래스 내부에서만 조작할 수 있게 도와줌
- 비공개 필드 형식 : __필드명

```python
def __init__(self,modeln,speed=0,color='white'):
	self.modeln = modeln
	self.speed = speed
	self.__color = color #비공개 설정

def __modeln(self): #여기서도 비공개 설정 가능 이 함수는 찾으려해도 찾아지지 않음!
	pass

def setColor(self, color):
	self.__color = color #이렇게 뒤에 함수에도 __이렇게 정의해줘야함!
```



### 특별한 메서드 (매직 메서드)

> \_\_메서드이름\_\_() 의 형식으로 미리 정의되어 있는 메서드. 객체와 객체끼리 연산을 가능케 해줌
>
> 일반사용자들도 이 매직 메서드를 사용해서 편하게 이해하고 실행할 수 있도록

```
# __ge__() : >= : great equal
# __gt__() : >  : greater than
# __lt__() : <
# __le__() : <=
# __ne__() : != : not equal
# __eq__() : ==
# __init__() : 생성자
# __repr__() : 인스턴스 print()문으로 출력. 
# __add__() : +
# __del__() : 소멸자 메서드, 인스턴스 삭제 (메모리에서 지움)
```



### 클래스 상속

> 슈퍼클래스의 함수와 변수들은 서브클래스가 모두 이용가능

- 클래스 상속 방법: 

```python
class Animal: #superclass
# 클래스 변수들 :
age = 0
color = '' 

    def __init__(self,age,color):
        self.age = age
        self.color = color
        
    def talk():
        print('조잘조잘')
        
class Cat(Animal): #subclass, subclass명(superclass명)
    def __init__(self,age,color,type): #상위클래스의 변수 외에 다른 변수는 여기 추가
        Animal.__init__(age,color) #or
        super().__init__(age,color) #둘다 가능. 이거는 self 안하고 상위클래스에 변수만 작성
        self.type = type
	
    def talk():
        print('냐옹') # 매소드 재정의(overriding): 같은 함수명을 subclass에서 작성
    
```



- 다형성(polymorphism): 같은 이름의 메서드를 호출해서 각자 다른 기능을 수행하도록 한 것

```
#리스트 선언
animals = [하위클래스명(변수들), 하위클래스2(변수들), ...]

#리스트 요소 하나씩 꺼내서 함수 시행
for animal in animals:
	animal.메서드()
```



- 포함관계(has-a)

```python
class Person:
	def __init__(self, name):
        self.name = name
    
    def __repr__(self):
        return self.name
    
class PersonList:
    def __init__(self):
        self.lst = [] #각 인스턴스의 리스트 생성
        
    def appendList(self,name):
        self.lst.append(name) #각 인스턴스의 리스트에 name이라는 매개변수를 받아서 입력
    def showList(self):
        for i in self.lst:
            print(i) #리스트의 요소들을 print해서 보여준다
            
personlist1 = PersonList() #PersonList의 인스턴스 생성
namelist = ['jinx','vi', 'jayce']
for i in range(len(namelist)):
    person1 = Person(namelist[i])
    personlist1.appendList(person1) #person1을 name의 인자로 받아서 self.lst에 추가

personlist1.showList()

```

