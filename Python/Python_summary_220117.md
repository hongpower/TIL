# Python for test:

## 기본 기능들:

- \ 넣으면 줄바꾸고 이어서 작성 가능 (이어줌)
- ; 넣으면 2개의 명령을 한줄에 
- \ 넣으면  'don't' --> `'don\'t'` 이렇게 작성하면 '출력가능
- 반대로 \처럼 특수문자 의미를 제거 하고 싶다면? r 작성 `print(r':\python\name)`



## Format:

1. print('총점 : %.2f 과목명 : %d' %(80.55555, '수학') )

   - 소수점 2자리까지 반올림!
   - print(format'총점 : %.2f 과목명 : %d'  % ...) : 포맷만 넣으면 똑같은 결과값. 굳이 format쓸 필요 없다. 그래서 이거는 잊자

2. print(format(1234,567, '10.2f'))

3. print('총점 : {0}, 핸드폰 번호 : {1}'.format(100, '010-2712-3702'))

   - print('{0:d}, {1:5d}'.format()) : 1:5d는 1번째 인덱스에 있는 값을 최대 5자리로 불러온다

    ```python
    print('%s는 배가 %.2f%%만큼 고파요' %('지수',95.2034))
    print('{0}는 배가 {1:.2f}%만큼 배가 고파요'.format('지수',95.0333)
    ```

   - 문자열 정렬 문자(<,>,^) 사용 가능. 순서대로 왼쪽, 오른쪽, 중간

   ```python
   string = '졸려'
   num = 99
   
   print('{1:>10}%만큼 {0:>}'.format(string,num))
   >>> 오른쪽 정렬이 되어서 앞에 10정도 띄어져있음
   
   print('{:-^10}'.format(string))
   >>> 중간 정렬이 되는데 빈칸들은 -로 채워짐
   ```

   

## Datatype:

- int('숫자',진수) : `int('1010',2)`: 1010을 2진수라고 생각하고 이거를 10진수로 변경

- bin(10진수) : 10진수를 2진수로 변환. 2진수는 0b
- oct(10진수) : 10진수를 8진수로 변환 0o
- hex(16진수) : 10진수를 16진수로 변환 0x

```python
# 2진수를 16진수로 변환하고 싶다면
hex(0b1010)
```



## While:

- continue: 조건에 만족하면 break를 하고 반복문 초기로 돌아간다. this makes loop to continue as if it had just restarted

```python
for i in range(10) :
    if i % 2 == 0 :
        continue
    print(i)
    
>>>
1
3
5
7
9
```

- pass: 단순히 실행할 코드가 없다. continue는 조건을 만족하면 restart하고 만족하지 않으면 다음문장 실행한다. 근데 pass에서는 그냥 계속 진행한다

```python
for i in range(10):
    if i % 2 == 0:
        pass
    	print('pass 사용 {}'.format(i))
     else:
        print(i)
        
>>>>
pass 사용 0
1
pass 사용 2
3
pass 사용 4
5
pass 사용 6
7
pass 사용 8
9
        
```



## 내장 인덱싱:

- 리스트[-2] : 뒤에서 두번째 요소 반환
- 리스트[3][0] : 4번째 요소의 0번째 요소
- 리스트[n:\m:step] : n부터 m부터 step만큼씩 띄우면서



## Mutable:

- list,dictionary,set 수정하는 함수를 시행했을 때 __원본 변경__
  - `a=[1,2,3,4]`하고 `b=a`하면 얕은 복사가 되어서 b에 변경을 가하면 a까지 변경됨

- mutable을 immutable하게 만들기 위해서

- list와 딕셔너리는 shallow copy(진짜 복사를 해서 새로운 object를 만들지 못하고 같은 주소를 가리켜버리는)를 하기 때문에 만약 shallow copy를 피하고 싶다면 
  - 1) 한번더 변수를 만들자
    2) copy.deepcopy()를 쓰자!
    
    


## 리스트 연산 :

- lst1 + lst2 : 하나의 리스트로 이어진다
- lst1 * num : lst1을 세번 반복한 하나의 리스트로 출력
- lst1 = lst2

- 하나의 리스트로 반환



## 딕셔너리 :

- 인덱스 x

- 딕셔너리에서 value, key, item은 리스트 형태(실제로는 리스트는 아니고 dict_key)로 나오기 때문에
  - for value in dict.values() 이런식으로 진행해야함
  - 만약 리스트로 하고 싶다면 : list(dict.values())
- dict.get('key','') : key의 해당 value 반환. 만약 해당 key가 없으면 뒤에 문자열 반환. 뒤에 문자열 없으면 None 반환
  - 활용: if dict.get('key') is None:
  - print('문자열' in member) : value는 확인안하고 key만 보고 bool값 반환



## 튜플 :

- 원소 추가, 삭제, 변경 불가능.
- 인덱스로 __접근__은 가능하긴함! 그래서 .index, .count는 가능!!
- tuple([1,2,3]) => 튜플 형태로 나옴
- 요소말고 튜플 자체 삭제는 del(튜플명)
- append 사용 가능
- 원소가 한개인 튜플 만들고 싶다면 (요소,) or 요소, 이렇게



## Set:

- value값만 있는데 중복이 불가능하다. 딕셔너리의 key만 모아놓은 곳
- {} or set()
- __인덱스__가 없다 : 입력 순서랑 출력되는 순서가 달라진다
- 추가, 삭제 다 가능!
- .add(하나인자)
- .update(여러인자) : set.update([5,6])
- .remove() : 없으면 key에러뜸. key처럼 인식해서
  - 퀴즈.! .remove(1) 이면 인덱스일까? ㄴㄴ set는 인덱스가 x
- .discard() : 없으면 에러 뜨지가 않는다
- .clear() : 요소 전체 삭제, 빈집합
  - del(set) : 아예 삭제 메모리에서
- 집합 안에 __변경 가능한 항목은 포함이 불가능__하다 예 ) 리스트 : s5 = {1,2[3,4]}==> 오류
- 반대로 튜플은 변경이 불가능해서 포함이 가능하다

- Set operation:

  - {1,2,3} & {3,5,6} ==> {3} 교집합 OR {1,2,3}.intersection({3,5,6})
  - {1,2,3} | {3,5,6} ==> {1,2,3,5,6} 합집합 or {1,2,3}.union({3,5,6})
  - {1,2,3} - {3,5,6} ==> {1,2} 차집합  or {1,2,3}.difference({3,5,6})

  

## 내장메서드:

> 사용 가능한 메서드 찾기 : dir() 
>
> print(dir([]))

- 문자열.find('찾는문자') : 있으면 첫 글자의 index, 없으면 -1. __리스트는 find가 없다__

- .is():
  - isalpha()
  - isdigit()
  - isspace()
  - isalnum() : 문자 또는 숫자인지
  - islower()
  - isupper()

- 문자열.join('문자열') : '문자열'은 그대로 있구 앞에 객체가 맨앞과 뒤를 제외한 사이에 들어간다

- 문자열.replace('바꾸고싶은 문자열','바꿀문자열') : 없으면 아무것도 안함

  - 요소 변경은 문자열에서는 replace, 리스트에서는 lst[index]=''

- 문자열.strip() : 반환값이지 원본을 바꾸지는 x

- 문자열.upper()

- 문자열.lower()

- 문자열.capitalize()

- 문자열.swapcase()

- 문자열.title()

- 문자열/리스트.count() : 숫자는 안되고 개수를 세어준다

- 문자열/리스트.index('찾는문자열') : 없으면 오류

- 리스트.insert(인덱스,'') : 원하는 위치에 '' 입력

- 리스트.remove(값) : 지정한 값중 처음 만나는 값을 제거 : 없다면 error

  - 만약 다 지우고 싶다면 for구문 사용

  ```python
  for i in range(lst.count('값')):
  	lst.remove('값')
     
  ```

- 리스트.pop(값 or 인덱스) : 마지막값 저장하면서 제거, 값없으면 오류

- 리스트.extend() : 리스트 확장

  ```python
  # 한 리스트로 반환
  a = [1,2,3]
  a.extend([4,5]) 
  ```

- 리스트.sort() : 오름차순 정렬. __원본 변경한다!!!__ 알파벳도 오름차순, 대문자 먼저 정렬 ACbe 이 순서로
  - 리스트.sort(reverse=True) : 내림차순 정렬
  - 리스트.sort(key=str.lower) : 대소문자 구분없이 정렬
- 리스트.reverse() : 기존값 거꾸로
- 리스트.min()
- 리스트.max()




## 내장함수:

- len() : 문자열이나 리스트의 길이. 숫자 X
- chr() : 아스키 코드값을 문자로
- ord() : 문자를 아스키 코드로 예) a를 입력하면 a가 ascii로 뭔지
- divmod(a,b) -> (몫,나머지)
- enumerate(시퀀스,start=) : (인덱스,문자/요소)이렇게 반환. start=n은 n부터 시퀀스가 시작
  - 예) enumerate([1,2,3],start=1) => (1,1), (2,2)
  - --> 시퀀스 : 리스트, 튜플, __문자열__까지
- eval() : 문자열로 되어있는 계산식을 연산 결과 정수로
- filter(function,iterable): filter객체로 출력
  - filter(lambda식, iterable)

- range(start,stop,step): 지정한 범위의 값을 반복 가능한 객체로 반환하는데 __반복 가능한 객체__설정 필요
- zip(iterable,iterable) : zip객체로 반환; 동일한 인덱스 묶기
- lambda는 인수가 하나일 때는 (식)(인수) 이렇게 작성!
- del 리스트명
- sorted(리스트명) : 원본을 수정하지 않고 복사해서 정렬
  - sorted(리스트명,reverse=True)
- sum(리스트명)
- all(iterable) : 모든 요소가 참이면 true. -1이 있어도 true. None이나 ' ' 빈문자열, 빈리스트도 False취급
- any(iterable) 



## 함수 :

- 반환값이 여러개인데 하나의 변수만 지정하면 tuple형태다
- 디폴트 매개변수 : def order(price, quantity=100) 이런식으로 지정되어 있음

- 키워드 인수 방식 : 매개변수의 이름과 인수를 연결시키는 방식
  - 일반적 '위치인수' 방식 sumN(10,20,30)
  - sumN(a=10, b=20, c=30)
  - sumN(a=10,b,c) ==> 오류. 키워드 인수 뒤에 위치 인수 작성 x





## 전역변수 & 지역변수:

- 지역변수는 밖에서 사용 불가능하다. (매개변수로 받은 인수 포함)
- 배정연산자(값을 변경하는 연산자)를 사용하면 전역 변수로 인식을 못하고 지역변수로 인식
- 함수 내부에서 전역 변수를 변경한다면 global꼭 언급 (함수 내에서만!)
- 인덱싱을 통한 변경은 mutable는 재할당을 하지 않고 원본 값을 변경한다.
- int, str처럼 immutable을 변경하고 싶으면 전역 변수로 저장한다 =>
- 그러면 a +=1 이건 뭐냐? 새로운 a객체를 생성하여 해당 값을 할당하기 때문에 __=연산자 사용전의 a의 id와 사용후의 a의 id가 다르다__



## 모듈 :

- from random import * : 모듈내에서 __로 시작하는 매직메서드를 제외한 모든 것을 참조:



## File:

- f = open('file명/경로','w/r')
- f.write('')
- f.read() : read안에 1을 쓰면 1바이트 읽어라
- f.readline() :첫째줄만
  - 첨부터 끝까지 한줄씩만 : while True
- readlines() : 다 읽어오기, 리스트로 반환
- f.close __항상 작성__
- f.seek(offset,from_what) : file을 어디서부터 읽거나 쓸건지 커서를 지정가능. 1바이트씩 읽고, 한글은 2바이트다 그래서 짝수로 변경
  - offset = num of positions to move forward
  - from what : point of reference 0(beginning,default),1(current),2(end)
  - 어떻게 쓰이냐면 f.seek로 커서옮겨서 거기서 터 readline()하기 인덱스와 같고 20이면 20번째 부터읽음
  - 만약에 whence를 2로 offset은 -10이다 그러면 whence에서 시작해서 -10으로 간다 거기서 오른쪽으로 다시읽기
- 메모장에서는 한줄 바꿀 때마다 실행:
  - line feed : \n (줄바꿈)
  - carriage return : \r (그 줄의 맨 첫번째로 이동)
- with open('파일명','모드') as 파일 객체 : close()를 자동 수행

- os.path.isdir/isfile
- os.path.exists
- os.remove('주소')

- 이진파일 
  - f1 = with open('파일명','rb')
- pickle: 텍스트가 아닌 자료형(예를들어 리스트 , 클래스)는 f.write를 사용해서 .txt파일에 작성이 불가능하다. 그래서 이런 경우에 자료형을 망가뜨리지않고(즉 리스트는 리스트 형태로) 파일을 저장해서 로드 가능하다
  - pickle.dump(data,file)
  - `with open('a.txt','wb') as f: \n pickle.dump(['1','2'],f)
  - data = pickle.load(f) : 한줄씩 읽어오고 더이상 로드할 데이터가 없으면 EOFError



## Error:

- TypeError : 부적절한 형, 예를 들어 문자로 숫자 계산
- ValueError : type에는 문제가 없지만 value가 틀릴 때 or incomplete한 형태

- OSError : 경로명 잘못 작성

- Try: 해봐, except TypeError as e: 에러가 발생하면 else: 정상, finally



## list comprehension

- 조건문 : lst = [i for i in range(5) if i%2 == 0] ==> condition only일 때
- 조건문2 : [i if i%2 == 0 else for i in range(5)] #if를 쓰고 else를 쓰고 for문작성
- for 구문 2개 작성 가능

```python
result = [i+j for i in list1 for j in list2 if not (i==j)]
#==>첫번째 i -> 처음부터끝까지 j만들고, 두번째 i-> 처음부터 끝까지 j

result = [(w.upper(),w.lower(),len(w)) for w in words] #이거는 튜플 형태로 가져온것임
print(result)
#[('REMEMBER', 'remember', 8), ('TO', 'to', 2), ('LET', 'let', 3), ('HER', 'her', 3), ('INTO', 'into', 4), ('YOUR', 'your', 4), ('HEART', 'heart', 5)]
```



## Class

- isinstance('인스턴스명',''클래스명'')
- 매직메서드 : 사용하면 객체와 객체끼리 연산가능 우리가 일반적으로하는 a + b도 사실은 매직메서드로 구현을 해논 것임. 그래서 내가 만약 객체1 + 객체2 하면 +기호가 `__add__`를 불러와서 그 함수를 실행

- 상속:

```python
class A:
	def __init__(self,a,b,c)
	.
	.
    
class B(A):
    def __init__(self,a,b,c,d):
        super().__init__(a,b,c)
		self.d = d
```

- class명.mro() : method resolution order -> 메서도 호출순서 확인 가능
- 정적 메서드: 인스턴스를 통하지 않고 바로 클래스에서 사용
  - 메서드 위에 일회로만 @statimethod 작성하고 self 삭제. 그냥 이 함수만 순수하게 사용할 때 사용
- 클래스 메서드:
  - 정적 메서드와 유사하지만, 첫번째 인자가 무조건 cls다
- 만약에 하위클래스에 `__init__`이 없다면 상위클래스의 `__init__` 수행
- 데코레이터라고, @property 사용하면 숨겨진 변수 찾기 가능
- class변수를 쓰고 싶으면 함수 내에서 클래스명.변수 = ''' 하면댐
- 메소드 재정의(오버라이딩)

## 기타:

- Asterisk: 가변길이 매개변수. iterable한 자료를 unpacking
- args는 for랑 같이 안쓰면 튜플 형태로 한번에 반환

```python
def practice(*args):
	for arg in args:
		print(arg)

practice(1,2,3,4,5)
>>>
1
2
3
4
5

```

- kwargs : 딕셔너리 형태로 반환, 키워드인수. x=y에서 y는 필수가 아님
  - args는 튜플, kwargs는 딕셔너리 형태로 
  - 키워드인수 : (first= 'python' ,second = 'is', third = 'shit')
  - .items() 는 튜플 형태로 출력


```python
def practice(**kwargs):
	for key, values in kwargs.item():
		print('%s의 %s입니다' %(value, key))
    for item in kwargs.items():
		print(item)
```

