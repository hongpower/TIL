# Pandas

## 그룹 분석

분할-> 적용(각 그룹에 대해서 연산 수행) -> 조합(결과 합치기)



### 데이터 분할 :

- data.groupby(열/행) : 입력한 열이나 행기준으로 group을 짓고 __groupby__객체 반환한다

  - data.groupby([열1,열2]) : 복수 컬럼 그룹화
- :heavy_check_mark:  복수 컬럼일 때는 앞에 data 써줘야함 ;`data.groupby([data.열1, data.열2])`
- groupby를 수행하면 groupby객체를 반환한다
- 그룹화의 기준이 되는 열/행은 __범주형__이여야한다

​    

- group.groups : 딕셔너리 형태로 {그룹명 : [values], 그룹명2 : [values2]} 반환. 각 그룹에 무슨 값이 들어있는지 확인 가능

- group.ngroups : group개수 확인가능

- 그룹 출력 함수 정의 :

```python
def print_group(group):
	for name, values in group: # 그룹 객체는 그룹명과 그룹의 값들을 가진 2개의 인자로 구성
		print(name)
		print(values) # 만약 모든 데이터 안꺼내고 상위 5개만 꺼내고 싶으면 print(values[:5])
```

- 그룹 데이터 프레임으로 저장:
  - `pd.DataFrame(group)` => 그룹별로 데이터가 나뉘어서 입력된 것 확인 가능
- 첫번 째 그룹 출력(상위 데이터만 일부 출력):
  - `pd.DataFrame(group).loc[0]`
- 두번 째 그룹 출력(상위 데이터만 일부 출력):
  - `pd.DataFrame(group).loc[1]`
- 한 그룹내의 모든 값 출력:
  - `pd.DataFrame(group).loc[0].values`



- 함수 :

  - group.size() : 각 그룹 크기 반환

  - group.count() : 각 그룹의 컬럼마다의 value(item) 개수 반환

  - group.sum() : 수치형 데이터만 계산을 하는데, 각 그룹의 컬럼끼리 합계 반환

    - 지정한 컬럼명만 그룹끼리 sum을 수행하고 sum한 값들 __시리즈__로 반환:

      - group[컬럼명].sum() 
      - group.sum()[컬럼명]  

    - 지정한 컬럼명만 그룹끼리  sum 수행하고 sum한 값들 __dataframe__으로 반환:

      - group[[컬럼명]].sum()
      - group.sum()[[컬럼명]] 


```
# 데이터프레임의 컬럼들을 멀티 인덱스로 변경:
df.set_index(['컬럼1','컬럼2'])

# 멀티 인덱스의 상위 인덱스(레벨0)를 기준으로 그룹하고 groupby객체로 반환:
df.groupby(level=0)
df.groupby('상위인덱스명')

# groupby객체 알아보기 쉽게 series로 출력:
name, data in group:
	print(name)
	print(data)

```



#### 그룹별 집계 함수:

> groupby객체의 함수들인 apply(),agg(),aggregate()사용

##### agg()/aggregate() :

> 각 그룹에 대해 집계함수를 모두 적용하는 함수이다. 

- group.agg(사용자정의함수) : 인수나 ()는 작성할 필요가 없다

  - 예) group.agg(countN)

- N개의 __내장함수__를 agg에 사용하고 싶을 때 : [] 사용

  - 예) group.agg([np.sum,np.mean])
  - 예) group.agg(['sum','mean'])

  :heavy_check_mark: 그냥 sum만쓰는게 아니라 __내장함수 쓸 때는 np.를 꼭 붙여주기 or ''씌워주기__



##### apply()

> 숫자 타입의 스칼라만 리턴

- agg()와 형식은 같다



##### agg() VS apply():

- agg()는 반환값이 수치 스칼라인 경우만 사용 가능
- apply()는 반환값이 수치 집합인 경우와 스칼라인 경우 둘다  적용 가능

```python
## 반환 값이 수치 집합인 (정렬)함수 :
def top3_petal_length(df):
    return df.sort_values(by='petal_length',ascending=False)[:3]
    
# agg()사용 => 오류 : return값이 수치 집합이라서
group.agg(top3_petal_length)

# apply()사용 => 정상적으로 작동
group.apply(top3_petal_length)


## 반환 값이 스칼라인 (계산)함수 :
def peak_to_peak_ratio(x):
    return x.max() / x.min() 

# agg()사용 => 정상
group.agg(peak_to_peak_ratio)

# apply()사용 => 정상
group.apply(peak_to_peak_ratio)
```



#### qcut() :

> 수치형 자료 => 범주형으로 변환

- pd.qcut(data,n,labels=[]):

  - data를 n개의 분위로 나누는데 각분위의 이름은 label이다

  ```python
  def q3cut(data):
  	qcut(data,3,labels=['상','중','하'])
  ```

  ```python
  ## group.열이름.apply(q3cut)
  iris.groupby(iris.species).petal_length.apply(q3cut)
  
  # 결과값은 petal_length의 수치들을 3등분해서 각각 label을 주어서 Series로 출력
  ```

  

### 데이터 그룹 변형 :

#### transform():

> df의 모든 값에 함수 적용해서 새df 반환

- transform(함수)실행하면:

  - 모든 그룹의 인덱스가 합쳐진 새로운 인덱스를 갖는다
  - row개수는 모든 그룹의 row개수의 총합과 동일하다
  - 그룹화 대상이 아닌 컬럼도 정상적으로 적용된다면 결과에 포함되고, 그렇지 않은 컬럼은 삭제될 수도 있다. 예를들어 합,곱하기 등의 수치형 연산을 함수로 적는다면, 범주형 데이터들은 포함되지 않고 출력된다
  - 이름은 transform이지만 실제 값을 변경하진 않기 때문에 변경을 원한다면 변수에 넣어주기

  

- transform의 매개변수인 함수는 lambda로도 적용가능하다

```python
# 결측치 평균값으로 대체하기
group.transform(lambda x: x.fillna(x.mean()))
```





#### 그룹 필터링 filter():

> 데이터 그룹 선택적으로 삭제

- filter(함수)
- filter도 lambda와 잘 쓰인다

```python
# df를 Label끼리 group화 한 다음, 각 그룹의 'Values'라는 열이름의 item개수가 1개 이상인 데이터만 출력
df.groupby('Label').filter(lambda x : x.Values.count() > 1)

# df를 Label끼리 group화 한 다음, 각 그룹의 'Values'라는 열이름에 하나라도 NaN값이 있으면 제외
df.groupby('Label').filter(lambda x : x.Values.isnull().sum() == 0)

# df를 Label끼리 group화 한 다음, 각 그룹의 'Values'의 평균이 전체 그룹의 'Values'의 평균과의 차이가 2가 넘는 그룹들만 출력
gMean = df.groupby('Label').mean().mean() #그룹 평균, 열이 하나라서 Values만 지정할 필요 없음
df.groupby('Label').filter(x:lambda x.Values.mean() - gMean > 2)
```

