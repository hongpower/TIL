# 변이통계량

:heavy_check_mark: import _ from _ 를 사용하면 모듈명안쓰고 바로 함수쓰면 된다



### 범위(range)

> 최대값 - 최소값

```python
# numpy 활용
np.max(data)-np.min(data)

# numpy의 ptp활용 ; peek to peek
np.ptp(data)
```



#### 중간범위(middle range)

> 최대값과 최소값의 평균

```python
# numpy 활용
np.max(data)+np.min(data)/2
```



### 사분위간 범위(IOR)

> Q3 - Q1

```python
# numpy quantile 활용
np.quantile(data,0.75)-np.quantile(data,0.25)

# scipy 활용
sp.iqr(data)
```



#### 사분위수 편차

> IOR을 2로 나눈 값으로 사분위수의 평균값. 범위의 문제점을 보완한 척도이다

```python
# numpy quantile 활용
(np.quantile(data,0.75)-np.quantile(data,0.25))/2

# scipy 활용
sp.iqr(data)/2
```



### 편차(Deviation)

> 자료값 - 평균

```python
data-np.mean(data)
```



### 분산(Variance)

> 편차를 제곱한 데이터를 다 더하고 그것의 평균

- 분산되어 있는 정도를 편차를 다 더해서 구하게  되면 0이 나와 파악하기 힘드니, 편차를 제곱해서 더한 후 그것을 N or N-1로 나눠준 것이다
- 모분산은 N으로 나누고(시그마제곱), 표본 분산은 N-1로 나눈다.
- ddof : 디폴트 = 0이고 표본 분산은 1로 변경한다
- 표분산보다 모분산이 편차가 더 크다!

:heavy_check_mark:pandas는 ddos default가 1 이고, np는 0이다

```python
#var(data,ddof=0)활용
x=[1,2,3,4,5]

# 표본 분산:
np.var(x,ddof=1)

# 모분산:
np.var(x) #어짜피 ddof 0, 모분산이 default라
np.array(x).var()
pd.Series(x).var(ddof=0) # pdandas는 default가 1인다!!!!
```



### 표준편차

- 분산에 제곱근을 씌우면 표준편차 (제곱하면 단위가 cm^2이 되니까, 다시 cm를 만들기위해)
- 편차를 제곱해서 더하고 n/n-1을 나누어서 분산을 구하고, 다시 제곱근을 씌워서 표준편차를 만든다
- 분산보다는 표준편차가 좋다

```python
x=[1,2,3,4,5]

# 표본의 표준편차 (S):
np.std(x)

# 모표준편차(sigma):
np.std(x)
pd.Series(x).std(ddof=0)
np.array(x).std()
```



### 변동계수

- 표준편차를 평균으로 나눈 값 or 그 값에 100을 곱한 값
- 표본 변동계수 : S/평균 
- 모변동계수 : 시그마/평균
- 두 자료의 평균이 다를 때, 두 자료의 분산 정도를 비교하기 위해서 사용
- 예를 들어서 A의 표준편차가 5이고, B의 표준편차가 4이면 A가 표준편차가 크니까 분산정도가 크다고 생각이들겠지만 만약 A의 평균이 100이고 B가 평균이 10이라면? 상대적인 비율로 따졌을 때 B의 분산정도가 더 큰 것을 알수있다. 그래서 변동계수를 따지고 보면 실제로는 A(0.05)보다 B(0.4)가 더 크다

```python
# 변동계수 : np.std(x, axis=axis, ddof=ddof) / np.mean(x) 

mend = np.std(men, ddof=1)/np.mean(men)
womend = np.std(women, ddof=1)/np.mean(women)
mend
womend

# 변동계수 : sp의 variation활용 : 위에값과 살짝 다르긴하지만 괜찮은듯
variation(men)
variation(women)
```



## 데이터의 정규화

Scaling(표준화) : 값들을 상대적인 값으로 변화시키는 기법

1. Standard scaling(Z) : 표준 스케일링. 
   - 평균은 0, 표준편차는 1

```python
# Z-scaling한 값(편차/표준편차) <- 이값들이 z1, z2에 변경되어서 들어감
z1 = (df['english']-df['english'].mean())/df['english'].std()
z2 = (df['mathematics']-df['mathematics'].mean())/df['mathematics'].std()

print(z1.min(),z1.max())
print(z2.min(),z2.max())
# 결과값보면 -3~3 사이의 값으로 분포된것을 볼 수 있지
```



2. Min-Max scaling : 
   - 데이터값을 0~1 사이로 변환

```python
# min-max scaling (값 - 최소값/범위) : 0과 1사이로 값을 바꿈
s1 = (df['english']-df['english'].min())/np.ptp(df['english'])
s2 = (df['mathematics']-df['mathematics'].min())/np.ptp(df['mathematics'])
print(s1.min(),s1.max())
print(s2.min(),s2.max())
```

- Min/max는 minmaxscaler사용해서 쉽게 계산 가능!

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler() # 객체 생성
S = scaler.fit_transform(df) # 데이터를 min-max한값으로 변경 : 0과 1사이로 변경
pd.DataFrame(S, columns=df.columns, index=df.index).head() #데이터프레임으로 출력
```

