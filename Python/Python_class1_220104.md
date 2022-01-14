# Python

## file

- os 모듈:

  - os.mkdir('디렉토리명')
  - os.path.isdir('디렉토리명') : 해당이름의 디렉토리가 존재하는지 bool값으로 반환
  - os.walk(디렉토리주소): 디렉토리 목록보기
    - 활용: for root, dirs, files in os.walk('python/Day7')
  - os.path.exists(디렉터리주소): 해당 이름의 파일 또는 디렉토리가 존재하는지 bool 값으로 반환. isdir은 디렉토리만 가능하지만 exits는 파일도 가능
  - os.path.isfile or isdir(주소): 디렉토리인지 파일인지 구분
  -  os.remove('주소'): 파일 삭제
  - os.path.getsize('주소'): 파일 크기 확인. byte단위

  

- shutil 모듈: 디렉토리에 대한 복사, 삭제 등을 수행할 수 있는 모듈

  - shutil.rmtree('디렉토리명')

  

- zipfile 모듈:

  - zipfile.Zipfile('파일경로/파일이름', 'w') : 해당 파일을 zip한다

    - 활용: new = zipfile.Zipfile('c:/pythonStudy/Day8/Day8.zip', 'w')

    - new.write('파일이름명',compress_type=zipfile.ZIP_DELATED)
    - new.close()

  - zipfile.ZipfFile('파일경로/파일이름','r')

    - ext = zipfile.ZipfFile('c:/pythonStudy/Day8/Day8.zip', 'r')
    - ext.extractall('파일이름')
    - ext.close()
    
    

- 이진파일(binary file): 글자가 아닌 비트 단위로 의미가 있는 파일 : 그림파일, 음악파일, 동영상파일, 엑셀파일, 한글파일, 실행(exe)파일

  - binary file 읽기 or 쓰기: open('이진파일이름','rb'/'wb')

  - 이진파일 복사 (읽고 쓰기):

    ```python
    f1 = open('c:/windows/notepad.exe','rb')
    f2 = open('c:/Temp/notepad.exe','wb')
    
    while True:
        inStr = f1.read(1) #1바이트 읽어라
        if not inStr: #빈값이라면
            break
        f2.write(inStr)
        #f2에 글을 씀
        
    f1.close()
    f2.close()
    ```

- pickle모듈 : 메모리에 로딩된 객체나 정의된 클래스를 파일로 저장하여 사용

  - pickle.dump(객체,f) : 객체를 binary file에 저장

    - 활용: 

    ```python
    f = open('list.pickle','wb')
    result = [1,2,3,4,5]
    pickle.dump(result,f)
    f.close()
    ```

    

  - pickle.load() : 객체 로딩  

    ```python
    f = open('list.pickle','rb')
    result2 = pickle.load(f)
    print(result2)
    result2.append('데이터')
    f.close()
    ```



## Package

> 모듈들을 뫃아놓은 directory로서 패키지 directory 안에 \_\_init\_\_.py라는 빈 파일이 존재하는 directory. 

- 호출방식:

  - import 패키지.모듈
  - import 패키지.모듈 as 별명
  - from 패키지.모듈 import 함수

  
  

## Error

참고: [ErrorTypes](https://docs.python.org/ko/3/library/exceptions.html)

- ZeroDivisionError : 0으로 나눈 경우
- TypeError: 부적절한 형의 연산이나 함수 실행
- NameError : 존재하지 않는 변수를 선언할때
- ValueError: 연산이나 함수가 올바른 형이지만, 부적절한 값을 가진 인자를 받았을 때
  - 예) print('%d%' %b)
- SyntaxError : 문법 오류, 콜론 빼먹기
- IndexError: 없는 인덱스 추출
- UnboundLocalError: NameError의 서브 클래스로, 함수나 메서드에서 특정 지역 변수를 참고하는데 그 변수에 값이 연결되어있지 않은 경우
- ModuleNotFoundError : module이 존재하지 않거나 경로가 잘못된 경우
- FileNotFoundError : 열고자 하는 파일이 없는 경우
- OSError : 경로명을 잘못 쓴 경우
  - 예) f = open('c:\\test.txt') -> 역슬래쉬가 쓰임. 역슬래쉬를 쓰고 싶으면 2번 연속 작성!



예외 처리 : Error 발생했을 때 프로그램 중단하지 않고 실행 가능

형식:

```python
try: 
	에러가 발생할 문장들, 이 문장을 시도해봐라
except:
	에러가 발생했을 때 처리하는 코드
else:
	에러가 없을 때 처리하는 코드
finally:
	에러 여부와 관계없이 수행하는 코드
```

- except 뒤에는 Error명을 작성하면 후에 이점이 많다
  - 예) except TypeError
  - ​      except NameError as e



## Class

Object Oriented Program(OOP)인 파이썬

1) 클래스 선언:

```python
class Car:
	def __init__(self, 매개변수1, 매개변수2 ...):
		self.color = 'black' #필드 or 변수
        self.speed = 0
		
	def speedUp(self):
		self.speed += 10 # 꼭 이렇게 self언급필요
```

:heavy_check_mark: class명은 구분짓기 위해 대문자로 무조건 시작. 이외에도 식별자 규칙을 따라야함



2. 객체/인스턴스 생성:

```python
car1 = Car() #car1과 car2가 클래스 Car만들어진 인스턴스인 것임
car2 = Car()
```



3. 객체/인스턴스 활용:

```python
car1.speedUp()
car1.color = 'Red'
```



- isinstance('인스턴스명','클래스명') : 특정 클래스의 인스턴스인지 확인하는 함수
- field는 def \_\_init\_\_ 에 꼭 작성안해도 된다. ''객체.변수 = 값'' 형식으로 추가 가능  (class안이든 밖에서든 둘다 가능)
- self는 기존의 함수와 구분되기위해 존재. 예를 들어서 사용자정의함수 sum이 생겼을때 내장함수 sum과 혼동이 올 수 있기 때문에

## 기타

### Asterisk(*)

> 가변길이 매개변수. iterable한 자료의 요소 하나하나씩 꺼집어내줌 (unpacking)

- *args : 리스트/튜플 형태

  ```python
  def asterisk_test(a, *args):
  	print(a, args)
  	print(a, *args) # 이 경우에는 하나씩 풀어서 전달.
  
  asterisk_test(1,2,3,4,5)
  
  # 결과 :
  1 (2, 3, 4, 5)
  1 2 3 4 5 # 요소 하나씩 unpacking해줌
  ```

  :heavy_check_mark: args만 써서 출력을 하게 되면 tuple형태로 나오지만, 실제 함수 내에서 실행될 때는 요소 하나씩 받기때문에 don't worry!

- **kwargs : 딕셔너리 형태

    ```python
    def asterisk_test3(a, **kwargs):
        print(a, kwargs)
    
    asterisk_test3(1,b=2,c=3,d=4,e=5) #=>이런게 언패킹
    
    # 결과 :
    1 {'b': 2, 'c': 3, 'd': 4}
    ```



### List Comprehension

> 리스트를 빠르고 간결하게 처리할 수 있는 Python 코드 스타일

예) 정수 0~9까지의 값을 갖는 리스트를 생성하시오

1)  리스트에 요소를 생성하거나 추가하는 경우:

```python
#기존방식
lst = list(range(10))
print(lst)

#list comprehension
lst = [i for i in range(10)]
print(lst)
```



2. 리스트 요소 필터링; 짝수만 걸러내기

```python
#기존방식
lst = []
for i in range(10):
    if i%2 == 0:
        lst.append(i)
print(lst)

#list comprehension 1
lst = [i for i in range(10) if i%2 == 0]
print(lst)

#list comprehension 2
lst = [i if i%2 == 0 else for i in range(10) if i%2 == 0] #이렇게 else써주기
print(lst)
```



3. 2차원 리스트:

   ```python
   #기존방식:
   list1 = ['a','b','c']
   list2 = ['D','E','F']
   result = []
   for i in list1:
   	for j in list2:
   		result.append(i + j)
   print(result)
   
   #list comprehension:
   result = [j + i for j in list2 for i in list1]
   ```

   
