# Machine Learning 

- 함수 기반 -> 데이터 기반 프로그래밍
- 컴퓨터가 스스로 학습
- x,y를 주면 학습하면서 model(가설)을 만듦. 그리고 테스트를 해봄 x를 넣었을 때 모델을 활용해서 y가 잘 나오는지
- 대표적으로 scikit-learn, tensorflow, pytorch 등 다양한 머신러닝 관련 라이브러리 존재



## Sklearn

- scikit-learn으로 파이썬의 머신러닝 라이브러리

### Machine Learning steps

- __데이터 준비 -> 데이터 분할 -> (학습)준비 -> 학습 ->예측 및 평가__



### Types of Learning

- 지도 학습(Supervised Learning) 
  - 컴퓨터한테 학습시킬 때 문제와 답 (x, y)을 제공하고 컴퓨터가 스스로 지도하게하는 학습 방식
  - 예측(linear) 
    - **연속형 데이터**의 수치 계산

  - 분류(logistic) 
    - **범주형 데이터**의 확률 계산
    - binary classification : True/False
    - multi-label classfication : 여러 범주로 분류

- 비지도 학습(Unsupervised Learning) : 
  - 군집(clustering) 

- 강화 학습(Reinforcement Learning) : 
  - 보상(reward base)



## Supervised Learning

:heavy_check_mark: sklearn는 2d array만 받음 ; 그래서 만약 2d array로 바꾸려면 numpy의 array를 reshape(-1,1)로 변경. 리스트를 reshape 쓸 수는 없음 

:heavy_check_mark: pd.read_csv()의 __header__ : column이름으로 사용할 row number. 만약 작성을 안한다면 csv파일 읽어왔을 때 어떤게 column인지 분간을 못할 수도 있음.

:heavy_check_mark: pd.read_csv()의 __names__ : 사용할 컬럼 이름 지정 (작성한 순서대로 컬럼명 바뀜). 만약 header row를 포함하고 있다면 `header=0`를 꼭 작성.

:heavy_check_mark: pd.read_csv()의 __low_memory__ : mixed type(영어 + 한국어, 한국어 + 숫자처럼 여러 데이터 type이 섞인 것)있을 때 `low_memory = False`사용



### Linear

:heavy_check_mark: reshape에서 -1 : 변경된 배열의 -1 위치의 차원(행 or 열)은 원래 배열의 길이와 남은 차원으로부터 추정. 즉 행의 위치에 -1이 있다면 행은 지정한 열의 숫자로 자동으로 결정 된다. 그래서 `.reshape(-1,1)`을 하면 1개의 열만큼 만들어지고 행은 자연스럽게 만들어짐

- `.fit(x, y)` : x와 y를 주면 학습 모델 생성
- `.predict(x)` : x를 제공하면 y 예측. x는 2차원 데이터 형태여야함

1) X가 2차원이 아닐 때 :

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

## 데이터 준비
df = pd.read_csv('soldiers.csv', encoding='euc-kr', names=['순번', 'date', '가슴둘레', '소매길이', 'height', '허리둘레', '샅높이', '머리둘레', '발길이', 'weight'], header=0, low_memory=False)
# 데이터 전처리 :
df = df[['date', 'height', 'weight']]

df.dropna(inplace=True) # dropna해도 None이 아니라 ''라면 dropna가 안됌

# 년도만 출력
df['date'] = list(map(lambda x: int(str(x[:4])) if len(str)x > 4 else x, df['date']))
# 키는 소수점 첫번 째 자리까지
df['height'] = list(map(lambda x: float(str(x).split(' ')[0]), df['height']))
# 몸무게는 float로 변환 (''은 None으로 변환)
df['weight'] = list(map(lambda x: float(x) if x else None, df['weight']))
df.dropna(inplace=True)

X = df['weight']
y = df['height']

## 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=1)
# 2차원으로 만들기 (y말고 X만 하면 됌)
train_X = train_X.values.reshape(-1,1)
test_X = test_X.values.reshape(-1,1)

## 모델 준비
model = LinearRegression()

## 모델 학습
model.fit(train_X, train_y)

## 예측
# 테스트 데이터로 예측 :
predict = model.predict(test_X)

# 몸무게가 70kg인 사람의 키 예측 :
print(model.predict([[70]]))
```



```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt

## 1. 데이터 준비
df = pd.read_csv('weight_height.csv', encoding='euc-kr')
# 학교명, 학년, 성별, 키, 몸무게 데이터 가져오기
df = df[['학교명','학년','성별','키','몸무게']]
df.dropna(inplace=True)

# grade = 초등학교: 0/, 중학교 : 6/ 고등학교 : 9 + 학년
# df['grade'] = df['학교명']
df['grade'] = list(map(lambda x: x if x[-4:] == '초등학교' else 6 if x[-3:] == '중학교' else 9, df[['학교명']])) + df['학년']
# print(df[6000:7000])

# !! 학교명과 학년은 이제 필요없으니 삭제하기 : axis는 0이 디폴트(행)
df.drop(['학교명','학년'], axis='colunms', inplace=True)

# column명 영어로 바꾸기
df.columns = ['gender', 'height', 'weight','grade']

# 남자는 0 여자는 1로 바꾸기
# df['gender'] = list(map(lambda x: 0 if x == '남' else 1, df['gender']))
# df['gender'] = df['gender'].map(lambda x: 0 if x == '남' else 1, df['gender'])
df['gender'] = df['gender'].map({'남' : 0, '여' : 1})

# !! 조건 만들어서 분리 :
is_boy = df['gender'] == 0
# 조건은 [] 안에 작성
boy_df, girl_df = df[is_boy], df[~is_boy]

X = boy_df['weight']
y = boy_df['height']

## 2. 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=1)
train_X = train_X.values.reshape(-1,1)
test_X = test_X.values.reshape(-1,1)

## 3. 준비
# 값이 연속적 데이터일 때 쓰는 regression
linear = LinearRegression()

## 4. 학습
linear.fit(train_X, train_y)

## 5. 예측
predict = linear.predict(test_X)
print(test_X)
print(predict)
plt.plot(test_X, test_y, 'b.')
plt.plot(test_X, predict, 'r.')

plt.xlim(10, 140)
plt.ylim(100, 220)

plt.show()
```



2. X가 이미 2차원일 때 :

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt
import numpy as np

## 데이터 준비
df = pd.read_csv('soldiers.csv', encoding='euc-kr', names=['순번', 'date', '가슴둘레', '소매길이', 'height', '허리둘레', '샅높이', '머리둘레', '발길이', 'weight'], header=0, low_memory=False)
# 데이터 전처리 :
df = df[['date', 'height', 'weight']]

df.dropna(inplace=True) # dropna해도 None이 아니라 ''라면 dropna가 안됌

# 년도만 출력
df['date'] = list(map(lambda x: int(str(x[:4])) if len(str)x > 4 else x, df['date']))
# !!! 여기 추가 !!!
df['date_new'] = list(map(lambda x: 0 if x == 2013 else 1 if x == 2014 else 2 if x == 2015 else 3 if x == 2016 else 4, df['date']))
# 키는 소수점 첫번 째 자리까지
df['height'] = list(map(lambda x: float(str(x).split(' ')[0]), df['height']))
# 몸무게는 float로 변환 (''은 None으로 변환)
df['weight'] = list(map(lambda x: float(x) if x else None, df['weight']))
df.dropna(inplace=True)

# !!! 여기 변경 !!!
X = df[['weight', 'date_new']]
y = df['height']

## 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=1)
# !!! 이미 2차원이라면 reshape할 필요 X !!!
#train_X = train_X.values.reshape(-1,1)
#test_X = test_X.values.reshape(-1,1)

## 모델 준비
model = LinearRegression()

## 모델 학습
model.fit(train_X, train_y)

## 예측
# 테스트 데이터로 예측 :
predict = model.predict(test_X)

# 몸무게가 70kg인 사람의 원하는 연도의 모델로 키 예측 :
print(model.predict([[70, 4]]))
```



3. 다항회귀(PolynormialFeatures) : 
- 특성이 두 개, 특성을 곱해서 다차원으로 바꿈

- [1, a, b, a^2, ab, b^2]

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import Polynomial Features
import matplotlib.pyplot as plt
import numpy as np

## 데이터 준비
df = pd.read_csv('soldiers.csv', encoding='euc-kr', names=['순번', 'date', '가슴둘레', '소매길이', 'height', '허리둘레', '샅높이', '머리둘레', '발길이', 'weight'], header=0, low_memory=False)
# 데이터 전처리 :
df = df[['date', 'height', 'weight']]

df.dropna(inplace=True) # dropna해도 None이 아니라 ''라면 dropna가 안됌

# 년도만 출력
df['date'] = list(map(lambda x: int(str(x[:4])) if len(str)x > 4 else x, df['date']))
# !!! 여기 추가 !!!
df['date_new'] = list(map(lambda x: 0 if x == 2013 else 1 if x == 2014 else 2 if x == 2015 else 3 if x == 2016 else 4, df['date']))
# 키는 소수점 첫번 째 자리까지
df['height'] = list(map(lambda x: float(str(x).split(' ')[0]), df['height']))
# 몸무게는 float로 변환 (''은 None으로 변환)
df['weight'] = list(map(lambda x: float(x) if x else None, df['weight']))
df.dropna(inplace=True)

X = df[['weight', 'date_new']]
y = df['height']

# !!! 다항회귀 생성
poly = PolynomialFeatures()
# x
X = poly.fit(x).transform(x)

## 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=1)
#train_X = train_X.values.reshape(-1,1)
#test_X = test_X.values.reshape(-1,1)

## 모델 준비
model = LinearRegression()

## 모델 학습
model.fit(train_X, train_y)

## 예측
# 테스트 데이터로 예측 :
predict = model.predict(test_X)

## !! polinomrmial 형태로 넣기 :
print(model.predict(poly.fit(np.array([[70, 0]])).transform(np.array([[70, 0]]))))
```

- `fit(x).transform(x)`:
  - training data에 쓰임
  - scaling parameter을 학습할 수 있음

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt
from sklearn.preprocessing import PolynomialFeatures

## 1. 데이터 준비
df = pd.read_csv('weight_height.csv', encoding='euc-kr')
# 학교명, 학년, 성별, 키, 몸무게 데이터 가져오기
df = df[['학교명','학년','성별','키','몸무게']]
df.dropna(inplace=True)

# grade = 초등학교: 0/, 중학교 : 6/ 고등학교 : 9 + 학년
# df['grade'] = df['학교명']
df['grade'] = list(map(lambda x: x if x[-4:] == '초등학교' else 6 if x[-3:] == '중학교' else 9, df[['학교명']])) + df['학년']
# print(df[6000:7000])

# !! 학교명과 학년은 이제 필요없으니 삭제하기 : axis는 0이 디폴트(행)
df.drop(['학교명','학년'], axis='colunms', inplace=True)

# column명 영어로 바꾸기
df.columns = ['gender', 'height', 'weight','grade']

# 남자는 0 여자는 1로 바꾸기
# df['gender'] = list(map(lambda x: 0 if x == '남' else 1, df['gender']))
# df['gender'] = df['gender'].map(lambda x: 0 if x == '남' else 1, df['gender'])
df['gender'] = df['gender'].map({'남' : 0, '여' : 1})

# !! 조건 만들어서 분리 :
is_boy = df['gender'] == 0
# 조건은 [] 안에 작성
boy_df, girl_df = df[is_boy], df[~is_boy]

X = boy_df[['weight', 'gender']]
y = boy_df['height']

poly = PolynomialFeatures()
X = poly.fit(X).transform(X)
## 2. 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=1)

## 3. 준비
# 값이 연속적 데이터일 때 쓰는 regression
linear = LinearRegression()

## 4. 학습
linear.fit(train_X, train_y)

## 5. 예측
predict = linear.predict(test_X)
print(test_X)
print(predict)
plt.plot(test_X, test_y, 'b.')
plt.plot(test_X, predict, 'r.')

plt.xlim(10, 140)
plt.ylim(100, 220)

plt.show()

# 평가!
acc = linear.score(test_X, test_y)
print(acc)
```



- df와 같은 2차원 데이터일 때는 reshape 노필요. **만약 series라면 `series.values.reshape()`사용하고, array라면 `array.reshape()` 사용**



### Logistic

- 데이터 타입이 __숫자더라도__, 그 숫자가 의미하는 바가 True/False와 같이 __분류__하는 개념이라면 categorical data임
- x는 데이터고, y는 category 

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
import numpy as np

## 1. 데이터 준비
# [3, 2000] : 3끼 밥먹고, 2000칼로리 먹었다 -> [1] : 살 찜
# [1, 1700] : 1끼 밥먹고, 1700칼로리 먹었다 -> [0] : 살 안찜
X = [
    [3, 2000],
    [1, 1700],
    [2, 700],
    [2, 3000],
    [1, 3000],
    [5, 1000],
    [1, 500]
]
y = [
    [1],
    [0],
    [0],
    [1],
    [1],
    [0],
    [0]
]

## 2. 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X,y, test_size=0.3, random_state=1)

## 3. 준비
logistic = LogisticRegression()

## 4. 학습
logistic.fit(train_X, np.ravel(train_y))

## 5. 예측
pred = logistic.predict(test_X)

for i in range(len(test_X)):
    print('{}번 밥을 먹었고 총 {} 칼로리 섭취 : {}'.format(test_X[i][0], test_X[i][1], '살안찜' if pred[i] == 1 else '살찜'))

print('acc : ', logistic.score(test_X, test_y))
```

- `np.ravel` : 다차원 데이터 1차원으로 변경.
  - y는 1차원 데이터를 활용하므로 np.ravel 활용

#### iris 데이터 :

- sklearn.datasets에서 가져온 iris 데이터의
  - `feature_names` 는 열과 유사
  - `target` 은 classfication target을 모든 데이터에 한 모습
  - `target_names`는 target class의 이름들

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
import pandas as pd
import matplotlib.pyplot as plt

## 1. 데이터 준비
iris = load_iris()

# feature_names : 열과 유사
# print(iris.feature_names)
# target : 모양과 길이를 통해서 꽃의 품종을 나눔
# print(iris.target)
# target_name : 나눈 품종의 이름 확인
# print(iris.target_names)

X = iris.data
y = iris.target

# feature_names : ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
# target_name : ['setosa' 'versicolor' 'virginica']

## 2. 데이터 분할
train_X, test_X, train_y, test_y = train_test_split(X, y, test_size=0.3, random_state=1)

## 3. 준비
logistic = LogisticRegression()

## 4. 학습
logistic.fit(train_X, train_y)

## 5. 예측
pred = logistic.predict(test_X)
for i in range(len(test_X)):
    # !! 여기서 pred[i]는 target_names가 아니라 target이기 때문에...
    print(f'{test_X[i]} 예측 : {iris.target_names[pred[i]]} / 실제 : {iris.target_names[test_y[i]]}')

print(f'acc : {logistic.score(test_X, test_y)}')
```



- iris data가지고 df만들고 category라는 컬럼도 만들기 :

```python
df = pd.DataFrame(iris.data)
df.columns = ['꽃받침 길이', '꽃받침 넓이', '꽃잎 길이', '꽃잎 넓이']
# 여기 케이스에서는 -1 없이도 같은 모습
df['category'] = pd.Dataframe(iris.target_names[iris.target].reshape(-1))

grps = df.groupsby('category')
fig, ax = plt.subplots()
for name, group in groups:
    ax.scatter(group.sepal_length, group.sepal_width, label=name)
plt.show()
```



### KNN

> k-nearest neighbors의 약어로 입력값에서 가장 가까운 k개의 데이터를 비교한 후 k개 중 가장 많은 class로 분류

- 분류학습을 끝낸 모델에 예측값을 넣으면 예측(X)로부터 위치가 가장 가까운 k개의 데이터의 class가 무엇인지 확인하고 예측값(X)를 분류해서 return

- import sklearn.neighbors import KNeighborsClassfier

- `KNeighborsClassifier`

  - `n_neighbors` : default가 5. 즉 근처 5개를 비교한다는 것

    - 만약 `n_neighbors`의 수가 많으면 underfitting, 적으면 overfitting

      

### SVM

> 데이터 간의 거리가 최대한이 되는 선을 그어서 분류

- import sklearn.svm import SVC
- `SVC`() : Support Vector Classification
  - `kernel` : 알고리즘에 사용될 kernel type. linear, poly등이 있음
- 우리 예제에서는 `model = SVC(kernel = 'linear')` 사용

- 초평면 = margin이 최대화되는 결정 경계
- hard margin SVM : 이상치를 허용하지 않고 최대한 둘을 분리할 수 있게 하는 선
- soft margin SVM : underfitting/ 이상치 어느정도 허용
- kernel Trick : 차원을 추가(초평면)하여 분류 (원안에 원이 있을 때처럼 선으로 가를 수가 없을 때)



### Decision tree

> 질문에 대한 답(true/false) 반복해서 분류

- from sklearn.tree import DecisionTreeClassifier
- `DecisionTreeClassifier()`

- 질문(node)가 너무 적으면 underfitting이 생길 수 있음
- 질문(node)가 너무 많아지면 과적합(overfitting)생길 수 있음 : 문제 다 읽지도 않고 문제 번호만 보고 답을 맞춰버릴 수도 있는 것임



### Random Forest

> 여러 개의 decision tree를 연결 (앙상블), bagging임.

- from sklearn.ensemble import RandomForestClassifier
- `RandomForestClassifier()`



## Unsupervised Learning

### k-mean 

- from sklearn.cluster import KMeans
- `KMeans()`
  - `n_clusters` : 군집의 개수
- **unsupervised learning이기 때문에 y가 필요없다 **

```python
from sklearn.model_selection import train_test_split
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt
import pandas as pd

iris = load_iris()
X = iris.data
y = iris.target

train_X, test_X, train_y, test_y = train_test_split(X, y, test_size = 0.3)

model = KMeans(n_clusters=3)

model.fit(train_X)

pred = model.predict(test_X)
print(test_X)
print(pred)
```

- 어떻게 군집한건지 그려보기 :

```python
df = pd.DataFrame(test_x)

df.columns = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
df['category'] = pd.DataFrame(pred)


centers = pd.DataFrame(model.cluster_centers_)
# 지금 column이 네 개기 때문에 centers 변수는 한 개의 군집당 4개의 중심값을 가지고 있음
centers.columns = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']
center_X = centers['sepal_length']
center_y = centers['sepal_width']

plt.scatter(df['sepal_length'], df['sepal_width'], c=df['category'])
plt.scatter(center_X, center_y, s=100, c='r', marker='*')

plt.show()
```

- `model.cluster_centers_` : 생성한 모델 군집의 중심(centroid) 위치 출력.
  - columns의 개수만큼 중심위치를 가지고 있는 array가 군집 개수만큼 있음

- 분류하고자 하는 클래스의 개수(k)
- 각 데이터들을 가장 가까운 그룹에 할당
- 각 데이터의 그룹과 중심의 거리 차이의 분산을 최소화 -> 즉 centroid(중심값)을 찾는다
