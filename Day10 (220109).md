# Python

백준을 하다가 며칠 파이썬을 안했다고 많이 잊은것 같아서 새로 배운다는 느낌으로 작성하는 복습 TIL

## 함수:

### 문자열 메소드

- find('',start,end) : 찾을문자열의 인덱스 반환. 없을시 -1 반환
- index('',start,end): 찾을 문자열의 인덱스 반환. 없을시 오류
- title() : 각 단어 첫문자를 대문자로 반환
- capitalize() : 문장의 첫 단어만 대문자로
- swapcase() : 대문자와 소문자 교환
- rstrip/lstrip/strip : 문자열 양끝 빈공간 or either of them삭제
- isalpha : 문자 논리값
- isdigit : 숫자 논리값
- isalnum : 문자/숫자 논리값
- islower : 소문자인지 논리값



### Datetime 메소드

- today()
- now()
- timedelta(days/hours = x) : 지정한 날짜/시간의 x일/시간 이후를 반환
- strftime(형식쓰기) : 날짜형식을 문자로
- strptime(바꿀문자열, 형식쓰기) : 문자를 날짜형식으로



### List 함수

- copy.deepcopy() : __copy모듈__ 필요!
- insert(위치, 값) : 원하는 위치에 지정한 값 삽입
- sum() : 총합
- max() : 최대값
- min() : 최소값
- del('리스트명') : 리스트 자체 삭제
- .remove(값) : 원하는 값을 리스트로부터 삭제

```python
x = ['a','b','c','d','a','b','a']
for i in range(x.count('a')):
    x.remove('a') 
```

기본적으로 remove는 지정한 리스트에서 가장 처음 만나는 값만 제거하기 때문에, 지정한 값을 모두 삭제하고 싶다면 for구문 사용가능!

- pop() : 마지막 요소 값 제거하지만 동시에 마지막 값을 저장하기 때문에 변수를 활용해서 삭제한 값을 변수에 저장할 수 있다

```python
y = [1,3,5,7,10]
last = y.pop()
print(last) #last변수에 삭제된 10값이 저장되고
print(y)#y는 10이 빠진 상태로 출력
```



- .extend() : 두 리스트를 한 리스트로 통합.
  - 예) a = [1,2,3] \ a.extend([4,5])하면  a는 [1,2,3,4,5]
- .sort() : 원본값을 바꾸며 오름차순으로 정렬
  - .sort(reverse =True) : 마찬가지로 원본값 바꾸며 내림차순으로 정렬
  - .sort(key=str.lower, reverse = True) : 대소문자 구분없이 정렬하면서 원본값의 정반대 순서로 정렬
- .reverse() : 원본값의 정반대로 정렬을 함
- sorted() : 리스트의 각요소의 첫 문자로 오름차순 정렬. 만약 첫글자가 같다면 둘째 문자로 비교. 함수라서 원본값을 바꾸지는 않는다

:heavy_check_mark: 문자열을 슬라이싱하면 문자열로 출력, 리스트를 슬라이싱하면 리스트로 추출

:heavy_check_mark:리스트끼리 연산이 가능하기 때문에 ==, !=, >, < 등을 활용한 리스트 일치 검사 가능

​	예) list1 = [1,2,3] \ list2 = [1,2,4] \ print(list1>list2)해서 리스트끼리 비교가능

