# Pandas

> Pandas stands for 'Python Data Analysis Library'. 데이터 분석 및 가공을 위한 파이썬 라이브러리로서 1차원 배열인 series와 2차원 배열인 datafram 등의 자료형을 활용한다. Numpy를 활용하고 있어 계산이 빠르고 Numpy와 다르게 다른 데이터형을 한 array에 넣고 계산이 가능하다.

목적 : 다른 유형의 데이터를 공통된 포맷으로 정리하고 특히 '데이터프레임'이 유용해 실무에서 자주 쓰인다 

```python
#판다스 불러오기
import pandas as pd
#버전 확인
pd.__version__
```



## Series:

> 1차원 데이터형으로 index가 0부터 시작하지만, index명을 변경이 가능하다. 딕셔너리와 비슷한 구조를 띄고있다

- pd.Series(seq_data) : Series 생성

  :heavy_check_mark:Series는 대문자로 작성하는 것 주의!

  :heavy_check_mark:판다스는 numpy와 다르게 default가 int64이다!

  :heavy_check_mark:문자열을 object라고 출력

  - seq_data => list, dict, tuple 다 해당함

- range() / np.arange() : 정수범위 자료를 시리즈로 생성

- np.nan : 결측값을 포함시킬 수 있음

- .values : 값만 1차원 배열로 출력



## Index 활용:

```python
# 숫자 인덱스 지정
s1 = pd.Series([10,20,30], index=[1,2,3]) #or
	 pd.Series([10,20,30],[1,2,3]) #index= 생략 가능!

# 문자 인덱스 지정
s2 = pd.Series([95,100,99],['영어','수학','국어'])

# 인덱스 불러오기 : .index
s1.index => Int64Index([1, 2, 3], dtype='int64')
s2.index => Index(['영어', '수학', '국어'], dtype='object')

# 인덱스에 이름 붙이기 : .index.name
s2.index.name = '과목명'
s2 =>
과목명
영어     95
수학    100
국어     99
dtype: int64
    
# 시리즈에 이름 붙이기 : .name
s2.name = '점수'
```



인덱싱 :

  ```python
s = pd.Series([9904312, 3448737, 289045, 2466052],index=['서울','부산','인천','대구'])

# 여러 개 접근할 때 :
s[[1,2]] or s[['부산','인천']] or s.부산, s.인천(이거는 출력형태가 튜플)=> 1번째, 2번째 값을 출력함
도시
부산    3448737
인천     289045
Name: 인구수, dtype: int64
        
# 데이터 변경:
s['서울'] = 10e5 #e5는 0의 개수가 5개라는 뜻

# 인덱스 재사용
r1 = pd.Series(np.arange(4),index=s.index) #s.index를 빼껴옴
=>서울    0
부산    1
인천    2
대구    3
dtype: int32 #!!!!!!!int32인 이유는 numpy의 arange를 사용했기 때문이다
  ```



#### 슬라이싱

파이썬/numpy와 비슷하게:

- data[m:n]형식으로 숫자형 데이터를 작성하면 m부터 n - 1자료까지 나오지만,
- __data['a','e'] 형식으로 '문자형' 데이터를 작성하면 'e'까지 다 출력이 된다!!__



#### 시리즈와 스칼라의 연산(백터라이징, 백터화 연산)

```python
# 합
pd.Series([1,2,3]) + 4

# 나눗셈
pd.Series([1000,500,20])/10
```



### 조건문

Numpy처럼 조건을 [] 안에 작성하면 된다.

```python
s0 = pd.Series(np.arange(10),np.arange(10) + 1)
# 짝수
s0[s0 % 2 ==0]
1    0
3    2
5    4
7    6
9    8
dtype: int32
    
# 인덱스가 5 이상 8 미만인 데이터 출력
s1[(s1.index >= 5) & (s1.index < 8)]

# 7 이상 개수
(s1 >= 7).sum() #조건에맞는(값이 True인)애들의 개수를 셈

# 7 이상의 값들의 합
s1[s1 >= 7].sum() #filtering된 값들을 합침
```





## 두 시리즈 간의 연산

:heavy_check_mark:주의할 것은 앞서 배웠던 Numpy의 연산과 헷갈리지 말 것!



Pandas에서는 Numpy처럼 같은 인덱스가 아니라, __같은 인덱스 명__을 가진 값끼리 계산하고 그 이외의 것들은 NaN처리 

```python
num_s1 = pd.Series([1,2,3,4], index = ['a','b','c','d'])  
num_s2 = pd.Series([5,6,7,8], index = ['b','c','d','a'])

# 합 계산 :
num_s1 + num_s2
# 순서는 오름차순!
a 9
b 7
c 9
d 11

num_s3 = pd.Series([5,6,7,8],['e','b','f','g'])
num_s4 = pd.Series([1,2,3,4],['a','b','c','d'])
# 차 계산 :
num_s3 - num_s4
a NaN
b 4.0
c NaN
d NaN
e NaN
f NaN
g NaN

#만약 같은 인덱스 명끼리 말고, 같은 위치의 값들끼리 계산을 하고 싶다면?
#1) 내가 한 방법:
np.array(num_s3) + np.array(num_s4)
#2) 강사님 방법:
num_s3.values + num_s4.values
#결과는 같음 :
array([ 6,  8, 10, 12], dtype=int64) #array형으로 받아온다
```



## in 연산자 / for 반복문

- '서울' in s 

- '대전' not in s

- .items() : 딕셔너리의 items()와 같지만, zip함수로 뽑아오기 때문에 추가적인 형태 지정이 필요하다

  - ex) list(s.items()) : [('서울', 10000000), ('부산', 3448737), ('인천', 289045), ('대구', 2466052)]

- for k, v in s.items() : 

  ​	print(k,v) 이렇게 작성할시 zip함수로 뽑아오진 않는다!

:heavy_check_mark:만약 for k,v in s로 작성하면 non-interable int 객체는 unpacking이 불가능하다는 오류가 뜬다



### 딕셔너리로 Series 생성

```python
s = pd.Series({'홍길동': 96, '이몽룡':100, '성춘향':88})
#리스트가 아니라 딕셔너리로 생성하면 key가 index가 됨

# 데이터 삭제:
del s['홍길동'] # 인덱스 명을 통해서 삭제. Python의 del함수와 유사
```



### Series 함수

- size: Series의 원소 개수 반환
- shape: Numpy처럼 Series의 shape을 튜플 형태로 반환 ex) (7,) 
- unique(): Series의 value를 중복값없이 출력
- count(): NaN을 제외한 value의 개수를 출력
- mean(): NaN을 제외한 평균 출력
- value_counts(): 중복을 제외하고 값들을 출력하고 각 값의 빈도수를 함께 출력

```python
s1 = pd.Series([1, 1, 2, 1, 2, 2, 2, 1, 1 ,3, 3, 4, 5, 5, 7, np.NaN])

# size (len도 활용 가능)
s1.size #or len(s1)
=> 16

# shape
s1.shape
(16,)

# unique()
s1.unique()
=> array([1., 2., 3., 4., 5., 7.,])

# count()
s1.count()
=>15 #NaN제외함

# mean()
s1.mean()
=> 2.0 #NaN제외

# value_counts()
1.0    5
2.0    4
3.0    2
5.0    2
4.0    1
7.0    1
#마찬가지로 NaN은 제외한다
```



### 날짜 인덱스

- date_range(start='date형식',end='date형식',periods=n,[freq='']):

  - date형식은 'yyyy-mm-dd'이다
  - periods는 출력할 개수
  - freq='D'가 디폴트값으로 하루 start부터 end까지 하루마다 출력한다
  - type은 datetimes module의 datetime이라고 나온다

- freq옵션:

  - D : 하루주기;day
  - B : 업무일(월~금) : business
    - 활용해서 3B, 2B 가능
  - W : 일주일 ; week
    - W-SUN : 매주 일요일마다. Default값
  - M : 달, 월말 default ; Month
  - BM :  업무달, 월말 default : Business Month
  - MS : 달, 월초 ; Month Starting
    - 4MS: 4달 주기로 월초 출력
  - BMS : 업무일 4달 주기로 월초

  :heavy_check_mark:중요한 것은 B는 앞에, S는 뒤에, 숫자는 매애애앤앞에

  - Q : 분기 ; Quarter
  - A : 1년; Annual
  - H : 1시간;Hour
  - BH : 09:00 ~ 17:00까지 ;Business Hour
  - T.min : 10분 ;Ten minute
    - 30분 : 30min
  - S : 1초 ; Second