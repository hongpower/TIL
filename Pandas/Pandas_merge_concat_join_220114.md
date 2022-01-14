# Pandas

## Pandas 데이터 합치기



### 1. 데이터 병합

> 기준이 되는 열(공통된 열)을 정해서 행을 합친다. 즉 공통된 열 이름을 가졌다면 한 데이터로 병합되어서 출력되게함

:question: key 란 ? 기준이 되는 열 데이터로서 행 인덱스 일수도, 데이터 내의 열일 수도 있다. 두 df에서 이름이 같은 열은 모두 키가 될 수 있고, 만약 열이름이 같더라도 두 df에서 쓰인 dataType이 다르다거나 이런 열이 잇으면 __on인수로 기준열 작성__해야한다. 즉 key내의 자료들은 datatype이 다 같아야함!

- merge(df1,df2,how,on,left_on,right_on,left_index,right_index) : 

  - 작성방식 : 
    - df1.merge(df2)
    - pd.merge(df1,df2)
    - 왼쪽에 있는 것이 기준 프레임 ; 공통 열이 출력된 후에 기준프레임의 열이 먼저 출력된다. 
  - 두 데이터 프레임의 공통 열이나 인덱스를 기준으로 합친 것
  - how : 병합방식 선택. how='inner' 이런식으로 작성. inner join --> default
    - inner join : 양쪽에 모두 존재하는 키의 데이터만 보여주기
  - on : on함수를 통해서 key를 명시할 필요가 있을 때 사용

  ```python
  df1 = pd.DataFrame({
      '고객명':['춘향','춘향','몽룡'],
      '날짜' : ['2018-01-01','2018-01-02','2018-01-01'],
      '데이터':[20000, 30000, 100000]
  })
  
  df2 = pd.DataFrame({
      '고객명':['춘향','몽룡'],
      '데이터':['여자','남자']
  })
  
  ## 두 df를 아무런 조건없이 합칠 때 :
  pd.merge(df1, df2) # ==> 오류가 뜬다, 고객명과 데이터 둘다 열 값이 같은데, df1의 데이터는 숫자 데이터고, df2의 데이터는 문자열 데이터기 때문
  ## 두 df를 on인수를 주고 병합할 때 :
  pd.merge(df1,df2, on='고객명') # ==> 고객명을 key로 잡아서 병합하겠다라고 명시해줘서 오류 x
  ```

  - left_on, right_on : 두 데이터의 키(기준열)의 이름(label)이 다른 경우 사용

  ```python
  df1=pd.DataFrame({
      '이름' :['영희','철수','철수'],
      '성적' :[90,80,80]
  })
  df2 = pd.DataFrame({
      '성명' :['영희','영희','철수'],
      '성적2':[100,80,90]
  })
  # ==> 지금 df1은 '이름', df2는 '성명'으로 열label이 다른 상황
  
  df1.merge(df2)
  # ==> 조건 안주고 merge 해버리면, 같은 열label이 없어서 key를 못찾고 오류(실패)가 뜸
  
  df1.merge(df2,left_on='이름',right_on='성명')
  # ==> 이렇게 왼쪽은 key는 이름, 오른쪽의 key(기준열)은 성명이라고 명시해주기
  ```

- default는 위와 같이 동일한 열이름을 찾아서 병합하는 것이 맞지만, __인덱스__를 기준(key)으로 병합할 때 :

  - left_index, right_index :

    - df의 index를 key(기준열)로 사용하겠다라고 명시하는 방법
  - 만약 df1과 df2 둘다 인덱스를 기준으로 병합하고 싶다면, `left_index = True, right_index = True` 써주기!

   ![image-20220114121515759](C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220114121515759.png)

   

    ```python
    ## df1은 다차원 인덱스를 가지고 있음
    df1 = pd.DataFrame(np.arange(8).reshape((4,2)), index = [['경상도','경상도','경상도','서울'],['울산','부산','대구','서울']])
    df1
    
    ## df2는 df1의 다차원 인덱스를 열로 가지고 있음
    df2 = pd.DataFrame({
        '도시': ['서울','울산','부산','대구'],
        '지역': ['서울','경상도','경상도','경상도'],
        '인구': [9853972, 9762546, 9631482, 3655437]
    })
    df2
    
    ## df2는 인덱스를 key로 갖고, df1은 지역과 도시의 한 세트를 key로 가져서 병합
    df1.merge(df2,left_index=True,right_on=['지역','도시'])
    ```

    출력값 :
  ![image-20220114121919799](C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220114121919799.png)

:heavy_check_mark:다차원 인덱스: 인덱스 안에 인덱스가 또 있는 것으로 인덱스가 2개일 때 

`df1 = pd.DataFrame({data}, index = [['경상도','경상도','경상도','서울'],['부산','부산','대구','서울']])`



- join():

  - 형식 : df1.join(df2, how=,on=)

  - merge()와 동일하지만 :

    - 1) __행__을 기준으로 데이터 병합 ; merge는 left_index/right_index를 주지 않는 이상 무조건 열로 병합을 했는데 (같은 열끼리 병합, 행은 신경 x) join은 같은 행끼리 병합한다

    - 2)  default가 how=left인 점도 다르다

      


### 2. 데이터 결합

> 행이나 열방향으로 그냥 합치는 것

- 일반적인 결합처럼 합칠 수 있고, merge()처럼 병합할 수도 있음 
- 아무런 설정없이 함수를 실행하면 기준열 없이 병합! ==> 인덱스 값이 중복될 수 있음
- 형식 : pd.concat([left,right],aixs=,join=,ignore_index,keys=) ==> __df를 넣는 방식이 merge()와 다름!__

- outer join --> default
- axis : 0이 default, 0은 세로로 늘어남(같은 열끼리 결합), 1은 가로로 늘어남(같은 행끼리 결합)
- ignore_index : 기존 인덱스값 유지 true or not
- keys : 계층적 index 사용하고 싶을 때

```python
# 결합 후에 인덱스 0으로 중복 된 경우 인덱싱 수행:
df.loc[0] 
==> 두 결과 출력

# 중복 인덱스 제거를 위해서 기본 인덱스로 재 설정:
df.reset_index(drop=True)

# 기존 인덱스 전부 제거 후 제로 베이스 인덱스 설정
pd.concat([df1,df2],ignore_index = True)
==> 인덱스 순서대로 출력
```



- 3개 이상의 df 결합도 가능! 아래로 붙여짐

```
## 3개의 df를 결합했을 때 2차원인덱스를 생성(상위레벨 인덱스 설정)을 통해서 정리 가능!
df1 = pd.concat([df1,df2,df3],keys=['1번째df','2번째df','3번째df']) #==> 멀티인덱스로 변환

## 멀티 인덱스인 경우 데이터 접근
df1.loc['1번째df'].loc[1:2] # ==> 1번째 df라는 인덱스의 1행부터 2행까지 출력
```




