# Pandas

<저번수업 복습>

- series.values -> values는 series의 값만 출력. 동일한 위치에 있는 값들끼리 예산해서 array형으로 출력. 대신 key는 출력하지 않음
- s2[''부산'':'대구'] 이렇게 슬라이싱도 가능!
- s3[s3 >= 3e6] : masking방법인데, 대괄호 안에만 작성하면 true와 false만 반환하는데 이걸 다시 s3를 앞에 작성하면 해당 값만 추출
- 'index명' in Series : 이렇게 index명이 series에 있는지 확인 가능



## Dataframe:

✔️ 변수명이 2개 이상일 때 한 셀에 여러코드를 한번에 작성해서 실행할 수 있게끔 하는 함수 : 

```
from IPython.core.interactiveshell import InteractiveShell InteractiveShell.ast_node_interactivity="all"
```

==> 여기서는 all이 작성되었지만 'all', 'last', 'last_expr', 'none'이 있다

#### 리스트, 딕셔너리, series로 dataframe 생성

```
## 리스트로 dataframe 생성
df1 = pd.DataFrame([['a','b','c'],['d','e']]) #괄호 잊지말기!

## 딕셔너리로 dataframe 생성
df1 = pd.DataFrame({'A':[90,80,70],
                    'B':[85,98,75],
                    'C':[88,99,77],
                    'D':[87,89,86]})
# 딕셔너리로 생성할 때 NaN 불가

## 딕셔너리로 dataframe 생성2
# dictionary 생성
data = {
    "2015": [9904312, 3448737, 2890451, 2466052],
    "2010": [9631482, 3393191, 2632035, 2000002],
    "2005": [9762546, 3512547, 2517680, 2456016],
    "2000": [9853972, 3655437, 2466338, 2473990],
    "지역": ["수도권", "경상권", "수도권", "경상권"],
    "2010-2015 증가율":[0.0283, 0.0163, 0.0982,0.0141]}

# 열방향 인덱스 columns = 
columns=['지역','2015','2010','2005','2000','2010-2015 증가율']

# 행방향 인덱스 index =
index = ['서울','부산','인천','대구']

# DataFrame(데이터, index=, columns=)
df2 = pd.DataFrame(data, index=index, columns=columns)

## 시리즈로 dataframe 생성
a = pd.Series([100,200,300],['a','b','d'])
b = pd.Series([101,201,301],['a','b','k'])
c = pd.Series([110,210,310],['a','b','c'])

df3 = pd.DataFrame([a,b,c],index=[100,101,102])
```

- 첫번째 예시처럼 만약 행의 원소 개수가 다르면 None값으로 저장됨
- 자동으로 인덱스와 컬럼명이 0부터 생성된다
- 만약 위치를 바꾸고 싶다면 columns=으로 재배열이 가능하다!
- 시리즈의 인덱스는 dataframe에서는 columns로 변경됨



#### 데이터파일로 Dataframe 생성

1. CSV로 Dataframe 생성

- pandas.read_csv('상대주소') 함수 사용
- https://www.kaggle.com/hesh97/titanicdataset-traincsv/data
- 위 링크에서 자료 다운



#### 함수/인수 모음

- index_col : 원하는 컬럼을 Index로 설정한다
  - df2 = pd.DataFrame(np.arange(13).reshape(3,4), index_col=0)
- usecols : 원하는 컬럼만 뽑아낸다
  - df2 = pd.DataFrame(np.arange(13).reshape(3,4), usecols=[0,1])
- index.name = ''
- columns.name = ''
- df.values : 데이터만 접근할 때, array형태로 반환
- info(): dataframe의 개요를 뽑아낸다
  - 4 non-null이라는 결과는 non-null값이 4개라는 뜻
- describe(): dataframe의 수치적/통계적 개요를 뽑아낸다
- df.shape : 행과 열을 뽑아낸다 (행,열)
- df.T : 행과 열을 바꾸는 전치를 수행 ; T는 Transpose
  - 예) dfT = df.T -> 원본 데이터 안바꾸기 위해서 dfT라는 변수 따로 만듬



#### 데이터 프레임 내용 변경

1. 열 값 변경 : df[열이름(key)] = values

```
df['2010-2015 증가율'] = df['2010-2015 증가율'] * 100     
```

1. 열 추가 :

- 참고로 열에 문자형이 섞여있으면 숫자도 문자형이 된다

```
p = ((df['2015'] - df['2005'])/df2['2005']*100).round(2)
df2['2005-2015 증가율']=p.map('{}%'.format)
```

1. 열 삭제 :

```
del df['2010-2015 증가율']
```



#### 데이터 프레임 인덱싱 (*)

> 항상 대괄호는 '열' 기준이다.

- 대괄호 안에 열이 한가지만 쓰일 때 :
  - 만약 대괄호에 하나에 열이 한가지만 쓰인다면, series형태로 출력한다
  - 대괄호를 두번 작성하면 열이 한가지 쓰이더라도, dataframe형태로 리턴한다

- 열이 두가지 이상이 쓰일 때 :
  - 열이 두가지 이상이 작성되면 dataframe형태로 리턴한다; 어짜피 대괄호를 2개를 써야하니. 만약 대괄호 하나에 쓰면 오류 발생

```
df[0] --> 시리즈 형태로 출력
df[[0]] --> dataframe 형태로 출력
df[[0,1]] --> dataframe형태로 출력, 0과 1이 dataframe의 열로 추출된다
# try, except 사용해서 예외 발생시키기 
try : 
    df2[0] # 만약 df2에 0이라는 key가 없으면 keyerror 발생
except Exception as e :
    print(type(e)) # error발생 명 return
```



#### 데이터 프레임 슬라이싱 (*)

> 슬라이싱은 항상 '행' 기준이다.

- 첫 행만 추출 : df[:1] --> 데이터 프레임으로 추출
- 첫 두행만 추출 : df[:2] --> 데이터 프레임으로 추출
- 슬라이싱은 **데이터프레임**형태로 반환한다!

✔️ 슬라이싱은 [a:b]고 a와 b가 숫자라면 range처럼 b-1까지 출력된다. 만약 문자라면 이상,이하로 b까지 포함되어서 출력 (random.randint(a,b)는 b까지 출력)



#### 개별요소 접근(*)

- [열][행]을 작성해서 개별요소 접근이 가능
- [열][행1:행2] 이렇게 작성도 가능하다. **하지만 열은 슬라이싱이 절대 불가능하다. 그래서 여러개에 접근하고 싶다면 하나씩 작성하는 수밖에 없다**

==> 즉! 개별요소 접근할 때 앞 대괄호가 슬라이싱이 작성되는 경우는 즐대즐대 없다!!



### 데이터 프레임 인덱서(loc, iloc)

> 기본적으로 데이터프레임에서는 열이 항상 우선이였는데(슬라이싱 제외), 인덱서를 활용하면 열이 아니라 '행'을 우선적으로 활용할 수 있다.
>
> 더 좋은건 이제 열한테도 슬라이싱을 이용할 수 있다는 것!

- loc : 라벨값 기반의 2차원 인덱싱 (location) --> 숫자형은 들어가지 않는다. 만약 라벨값이 3이라면 '3'으로 작성!
- iloc : 순서를 나타내는 정수 기반의 2차원 인덱싱 --> 라벨값은 사용 불가능하다.



##### 1) df.loc[행 인덱스 레이블/값]

- 행 우선 인덱서로 행을 먼저 찾게 해주는 함수이다

```
#데이터 프레임 생성
df0 = pd.DataFrame(np.arange(10,22).reshape(3,4), index=['a','b','c'],columns='A B C D'.split()) 

#df.loc['행']
df0.loc['b'] ==> b행 추출

#df.loc[['행1','행2']]
df0.loc[['a','b']] --> 슬라이싱 없이 dataframe으로 추출하고 싶을때

#df.loc['a':'b']
df0.loc['a':'b'] ==> a랑 b행 둘다 추출한다. 왜냐하면 슬라이싱에다가 loc이니까
```

✔️ loc을 사용하면 인덱스가 숫자더라도 '문자형'으로 인식해서 slicing해도 미만이 아니라 이하가 되는 것임



```
# 데이터 프레임 생성
df2 = pd.DataFrame(np.arange(10,26).reshape(4,4),
                   columns=('a','b','c','d'))

# 메인
df2.loc[1:2] ==> 숫자형이지만 loc과 슬라이싱이 만나서 index label'1'이렇게 인식을 해서 2까지 포함해서 출력
df2.loc['1':'2'] ==> 같은 결과
df2[1:2]==> 2출력 X

## loc으로 개별요소 접근
# df.loc[행인덱스, 열인덱스]
df.loc['a','A']

# df.loc[행인덱스, 열인덱스1:열인덱스2]
df.loc['a','B':'C'] ==> series로 출력. 행인덱스가 슬라이싱이 아니라서

## 개별요소 접근을 dataframe으로 출력하는 방법
# 1)
df.loc[['a':'b'],['B','D']]
# 2)
df.loc['a':'b',['B','D']]
```





##### 2) df.iloc[행 위치 인덱스]

> 라벨(name)은 안받고 위치 인덱스 정수만 받음

- loc과 같이 [행,열]이다
- 문자열로 인식하지 않기 때문에 loc과 다르게 슬라이싱은 이상 미만이다!

```
df0.iloc[0:2,[1]] 
# => 하나만 슬라이싱이지만 열부분을 []씌워서 dataframe로 반환

df0.iloc[0:2,1] 
# => 하나만 슬라이싱이라 series로 출력
# 0행의 끝에서 두번째 열부터 끝까지 출력
df0.iloc[0,-2:] 
# => 시리즈로

df0.iloc[:1,-2:] 
# => 데이터 프레임으로
```



```
# 만약 loc이나 iloc안쓰고 dataframe을 추출하고 싶다면?
df[:1][['D','E']]

# 밑에는 오류 나는 이유가 행의 원소가 2개인데 2개를 입력하면 무조건 가로 두개 씌워야하기 때문에
df[:1]['D','E']

# df만 단독으로 쓰이고 행은 짜피 슬라이싱 되어있다면, 
df[행1:행2][[열1,열2]]
df[[열1,열2]][행1:행2]
# 둘다 관계없다! 즉 순서는 상관없다!
```



##### boolean selection:

- series처럼 [조건]을 쓴다
- 대신 조건에 맞는 **'행'**을 반환한다!

```
# df의 A열이 15보다 큰 '행'을 RETURN
df[df.A > 15]
df.loc[df.A > 15]

```