# Python

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

   