# 기술통계량

유용한사이트:

https://docs.scipy.org/doc/scipy/reference/stats.html


https://numpy.org/doc/stable/reference/routines.statistics.html


https://docs.python.org/ko/3/library/math.html#number-theoretic-and-representation-functions

- 기본설정:

```python
import numpy as np
import pandas as pd
import math
from scipy.stats import *
import scipy as sp

# Jupyter Notebook의 출력을 소수점 이하 3자리로 제한
%precision 3
# Dataframe의 출력을 소수점 이하 3자리로 제한
pd.set_option('precision', 3)

```

## 위치통계량(Measure of Location):

#### 평균 :

- 산술평균 : 일반적으로 생각하는 평균
- 기하평균 : n개의 양수 값을 모두 곱한 것의 n제곱근. 성장률의 평균과 같이 비율로 구성된 경우 유용
- 조화평균 : 갈때 시속 x 돌아올 때 시속 y의 평균 시속 구할 때. 비율 및 변화율에 대한 평균 계산에 유용
- 가중평균
- 절사평균 : 일부를 절사 

```python
## 산술평균 :
# np와 pd, sp 모두 mean()제공
np.array(x).mean()
pd.Series(x).mean()
np.mean(df.열이름)
df.열이름.mean()
sp.mean(df.열이름)

# sum(), len() 활용
(df.열이름.sum())/len(df.열이름)

## 기하평균:
# math.prod() 사용
math.prod(data) ** (1/len(data))
# sp.gmean()사용
sp.gmean(data)

## 조화평균:
# np활용
len(data)/np.sum(data)
# sp.heman()사용
sp.hmean(data)

## 가중평균:
# average활용 : sum과의 차이점은 weight라는 parameter 받을 수 있다
np.average(np.arange(1,5).weights=np.arange(10,0,-1))

## 절사평균 :
# scp의 trim_mean활용 : 뒤 인자는 양쪽 끝20프로 절사한다는 것임
sp.trim_mean(data, 0.2) 
```



##### 중앙값(median):

> 이상치에 약한 평균의 약점 보완

- sorted를 하고 계산한다

```
# 데이터프레임의 특정 열 array로 변환
scores = np.array(df['english'])

# 순서 통계량으로 변경
scores_sorted = np.sort(scores)

## medain 계산
# 계산식
n = len(sorted_scores)
if n&2 == 0: #짝수라면
    x1 = sorted_scores[n//2-1]
    x2 = sorted_scores[n//2]
    median = (x1 + x2) /2 #중간에 있는 두 값을 2로 나눔
else:
    median = sorted_scores[(n+1)//2-1]
median

# np활용
np.median(scores)

# sp 사용
sp.df.영어.median()
```



#### 최빈값(mode), 최빈수

- 명목자료, 서열자료에 자주 사용

```python
# 모드 객체로 반환
>>> mode(Data)
ModeResult(mode=array(['A'], dtype='<U1'), count=array([350]))

>>> mode(data).mode
array(['A'], dtype='<U1') #최빈값은 'A'이다

>>> mode(data).count
array([350]) #최빈값 'A'의 개수(최빈수)는 350이다

# 모드 객체 반환없이 구하기
# 최빈값
>>> pd.Series(data).value_counts().index[0] # 가장 count가 높은 값이 index가 0이 되어서 무엇이 최빈값인지 출력
'A'

# 최빈수
>>> pd.Series(data).value_counts()[0] # 최빈값의 개수는?
350
```



#### 최대값, 최소값

```python
# 함수이용:
sorted(data)[0],sorted(data)[-1]

# np이용:
np.min(data)
np.max(data)
```



### 사분위수(quartile), 백분위수(percentile)

```python
# 1사분위수
np.percentile(data,25)

# 3사분위수
np.percentile(data,75)
```





### 5가 통계량

- 최소값, 제 1사분위수, 중위수 , 제 3분위수, 최대값 5개 다 출력
- upper whisker, lower whisker 밖에 값들은 이상치임

```python
# matplotlib을 통해서
import matplotlib.pyplot as plt

plt.boxplot(data)

# pd.describe() 활용
df['english'].describe() 

# 만약에 모집단이라면: 
>>>describe(df['english'],ddof=0)
DescribeResult(nobs=50, minmax=(37, 79), mean=58.38, variance=96.03632653061224, skewness=-0.31679325324962426, kurtosis=-0.38870454364589113)

# 만약에 표본이라면:
>>>describe(df['english'],ddof=1)
DescribeResult(nobs=50, minmax=(37, 79), mean=58.38, variance=94.1156, skewness=-0.31679325324962426, kurtosis=-0.38870454364589113)
#==> variance차이를 보면 표본이 더 작다
```

