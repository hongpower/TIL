# Pandas

## 데이터 조작:

> 데이터 집계(빈도,비율,합계, 평균 등), 데이터 정렬, 결측치 처리 방법, 데이터 범주화, 인덱스 변경

### 데이터 집계

#### 데이터 개수 세기:

1. count(): 

   - NaN값은 세지 않는다
   - Series: 결측값을 제외한 value개수를 센다
   - Dataframe: 결측치를 제외한 각 __열__에 있는 데이터 총 개수를 센다
     - 예) 결측값이 없는 4*4 dataframe이면 모든 열이 4개씩 데이터를 가지고 있다

2. value_counts(): 범주별 빈도/비율 계산

   - Series: value의 빈도수를 출력해준다
     - 예) s = 'a','b','b','c' 라는 series에서 value_counts()를 하면 value(a,b,c)의 빈도수 출력 : a' : 1 ; 'b' : 2 ; 'c' : 1
   - 범주형 데이터: 똑같이 value의 빈도수를 series로 출력해준다
   - Dataframe: 
     - df.value_counts(): 모든 열을 한 인덱스로 보고 그것에 대한 빈도수를 series로 출력
     - df.value_counts('열이름'): 해당 열의 value의 빈도수 series로 출력하고 index명은 전혀 관계이 출력.
     - df['열이름'].value_counts() : 이것도 위와 같이 출력

   - value_counts(normalize=True) : 비율로 출력

:white_circle: np.random.seed(n) : seed값이 같은 상태에서 random.randint()와 같은 난수 발생 함수를 실행하면 같은 난수 값이 나온다

:white_circle: 계속 변경되는 난수 생성 : np.random.seed(int(time.time())) (time()은 실수값이 나오니까)

:white_circle: seaborn패키지 내에 titanic 데이터셋이 존재해서 불러오기: import seaborn as sns ; titanic = sns.load_dataset('titanic')



#### 행/열 합계:

- df.sum()

- 데이터 프레임에서 df.sum()을 쓴다면 axis를 작성해야한다:

  - axis = 0 : 열 기준으로 더한다
  - axis = 1 : 행 기준으로 더한다

  

#### 평균, 최소, 최대값:

- 마찬가지로 axis를 설정 가능.

- mean() : 각 열마다의 평균을 출력한다 ==> series로 인덱스명은 각 df의 column명이 된다
- min() : 각 열마다의 최소값  ==> series로 인덱스명은 각 df의 column명이 된다
- max() : 각 열마다의 평균값  ==> series로 인덱스명은 각 df의 column명이 된다



#### 데이터 삭제:

- df.drop('행이름',0) : 행 삭제
- df.drop('열이름',1) : 열 삭제

:heavy_check_mark: __axis에서는 0이 열이였지만, drop에서는 0이 행이다__ !!!!!!!!

:heavy_check_mark: 원본에 반영되지 않기 때문에 원본을 바꾸고 싶다면 변수명을 저장해줘야함



### 데이터 정렬:

#### 데이터 오름차순/내림차순으로 정렬

1. sort_index():
   - Series: 말그대로 인덱스 기준으로 정렬
   - Dataframe:
     - sort_values와 다르게, 인덱스는 하나 뿐이니, by가 필요없다
     - 인덱스 이름을 오름차순으로 정렬
   - sort_index(ascending=False) : 내림차순. 디폴트는 오름차순이다

2. sort_values():

   - Series: 말그대로 value 기준으로 정렬

   - Dataframe:

     - 정렬시 무조건 무조건 !!! __by__로 기준열을 줘야한다!

       - 예) df.sort_values(by='열이름1') : 다른 열은 고려하지 않고, 열1의 value들을 오름차순으로 정렬하겠다!!
       - 예) df.sort_values(by=['열1','열2]) : 열1을 우선적으로 고려하고 같을시에 열2 고려한다

       

### 결측치 처리:

#### NaN 값 처리 함수들

1. df.dropna([axis=0/1]) : 결측값을 삭제

   - 만약 axis가 0이라면, nan을 포함한 __행__이 삭제된다

   :heavy_check_mark: 여기서도 axis가 0이면 행인데, 외울 때 drop과 관련된거는 0이 다 행이라고 생각하자!

   - 만약 axis가 1이라면, nan을 포함한 열 삭제

   

2. df.fillna('a') : 결측값을 대체

   - 결측값을 'a'로 대체
   - 이 친구도 변경값을 저장해줘야지 변경 가능

   :heavy_check_mark: 결측값은 실수라서, 결측값을 다른 숫자값으로 바꾸게되면, 그 해당 열의 모든 데이터도 같이 실수로 바뀌기 때문에 .astype(int)를 붙여주는 것이 좋다

   

### apply(*):

> 반복적으로 Dataframe의 행이나 열에 연산이 vectorizing이 가능하게끔 해줌

- apply(함수, axis=0/1)

- 0 : 열마다 반복, 1 : 행마다 반복, 생략: 기본값(which is 0)
- apply도 axis의 0이 열이다 ^^
- apply함수 내에 쓸 반복함수는 '()' 생략!

```python
df.apply(np.sum, axis=0)
=> df의 열 별로 sum을 한 후에 series로 출력

df.apply(np.square, axis = 1)
=> df의 행 별로 square를 하지만, square함수는 사실 어느 방향이든 값 같음
```



- apply는 lambda와도 함께 쓰임

```python
# diff라는 최대값과 최소값을 빼는 lambda함수를 정의한다음, 열별로 계산하기
diff = lambda x : x.max() - x.min()
df.apply(diff,axis=0)

# value_counts() 활용
df.apply(value_counts, axis = 0).fillna(0).astype(int)
```





### 데이터 범주화(*)

> 수치형 데이터 ==> 범주형 데이터로 변환
>
> 데이터값 ==> 카테고리값
>
> 범주형 ==> 수치형은 불가하다!

- cut(data,bins,label) : 카테고리 생성 함수

    - data는 구간을 나눌 실제 값, 관측 데이터

    - bins는 구간 경계값 : __초과 이하__로 범주가 나눠짐!

    - label은 범주 이름, 레이블 




- Series/list 데이터를 범주형으로 변환:

```python
category1 = pd.cut(data,bins,labels=labels)
=> category1의 type은 categorial 객체가 나온다
=> 출력하면 bins 기준에 맞춰서 data에 label 들이 매겨져있다. 즉 label명들의 집합처럼 생겼다

list(category1): 각 데이터에 알맞은 label명들을 차례로 출력

```



- Dataframe 데이터를 범주형으로 변환:

```python
df = pd.Dataframe(ages, columns=['age']) # 여기서 ages는 리스트 데이터

# 범주형 데이터 추가:
df['age_cut'] = pd.cut(ages,bins,labels=labels)
```



- qcut(data,n,label) : 구간 경계선은 지정하지 않고 사분위수로 분할

```python
category2 = pd.qcut(data, 4, labels=['Q1','Q2','Q3','Q4']])
```



- 카테고리 클래스 객체 :
  - category1.categories => 카테고리값, 즉 label 출력
  - category1.codes => 각 카테고리(label)마다 카테고리가 생성될 때 0부터 자동으로 숫자가 부여되는데 codes는 그 숫자를 보게 해줌. 결측치는 __-1__로 출력





