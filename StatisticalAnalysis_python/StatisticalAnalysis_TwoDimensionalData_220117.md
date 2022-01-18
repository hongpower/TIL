# 2차원 데이터

## 공분산(Covariance):

- Sxy 로 표시
- 데이터들의 관계성을 수치화한 것으로, 분산에 가가운 지표이다
- 이전에 배웠던 분산은 x축과 y축이 같아서 평균으로부터의 편차를 표기해 그린 도형이 정사각형이지만, 공분산은 직사각형이고 음의 면적도 얻을 수 있다는 점이 다른 점이다
- 양의 관계 : 공분산이 +
- 음의 관계 : 공분산이 -
- 무상관 : 공분산이 0

- Numpy의 cov함수 사용 : 대신 반환값이 공분산 행렬/분산 공분산 행렬임

```python
# 공분산 행렬 구하기 : np.cov()
>>> covScore = np.cov(a, b, ddof=0)

array([c,d],[e,f])
#d랑 e가 공분산에 해당. d와 e는 같은 값이 나온다
covScore[0,1], covScore[1,0]이 공분산이다

#c랑 f는 각각 a끼리의 공분산, b끼리의 공분산으로 a의 분산, b의 분산값과 같다
```



## 상관계수:

- rxy로 표시
- 분산도 제곱때문에 단위가 직관적이지 못하다는 단점을 가지고 있었는데, 공분산도 두 지표를 곱하기 때문에 단위에 대한 문제점이 있다.
- 단위에 의존하지 않는 상관을 나타내는 지표가 상관계수이다.
- 공분산/각 데이터의 표준편차
- 상관계수가 1에 가까울 수록 양의 상관관계
- 0에 가까울수록 무관계
- -1일수록 음의 상관관계
- np.corrcoef() -> 상관행렬 반환값

```python
## 수식으로 상관계수 구하기
np.cov(a,b,ddof=0)/np.std(a)*np.std(b)

## numpy의 corrcoef()로 구하기
>>>np.corrcoef(a,b)
array([1,c],[d,1])
# 어짜피 같은 데이터끼리의 상관관계는 무조건 1이기 때문에..

## dataframe도 corrcoef()로
df.corr()
```



### 산점도

- matplotlib의 scatter 사용



### 히트맵

- 히스토그램의 2차원 버전으로 색을 이용해서 표현
- hist2d 메서드 사용 (인수도 hist랑 비슷)

```python
fig = plt.figure(figsize=(n,m))
x = fig.add_subplot(111) # 1행 1열의 섭플랏의 첫번째

x.hist2d(a, b, bins=[a.bins, b.bins],range=[(a.range1,a.range2),(b.range1,b.range2)])

fig.colorbar(c[3], x=x)
```

