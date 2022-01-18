## 왜도(Skewness)

> 데이터가 기울어진 정도

- Right-skewed: 오른쪽으로 꼬리가 길고, 작은 값이 분포가 크다, 왜도가 0보다 크다(Positive)
  - 최빈값<중앙값<평균
- Left-skewed: 왼쪽으로 꼬리가 길다. 왜도가 음수이다(Negative)
- Symmetric: 왜도가 0에 가까울수록 좌우대칭이다

- 일반적으로 왜도의 절대값이 1.5이상이면 많이 치우쳤다고 판단한다

- scipy의 skew 활용

```python
from matplotlib import pyplot as plt
%matplotlib inline

# positive분포 => 현재 리스트 형태
x1 = [1] * 30 + [2] * 20 + [3] * 20 + [4] * 15 + [5] * 15

# symmetric 좌우대칭
x2 = [1] * 15 + [2] * 20 + [3] * 30 + [4] * 20 + [5] * 15

# negative
x3 = [1] * 15 + [2] * 15 + [3] * 20 + [4] * 20 + [5] * 30

## 빈도수 series 생성
pd.Series(x1).value_counts()

## 빈도수는 막대그래프로 표현
pd.Series(x1).value_counts(sort=False).plot(kind='bar') # 만약 sort=False를 안하면 막대그래프가 큰 순서대로 출력된다. sort=True가 default라서

## sp.skew()
skew(x1) # 0보다 큰값. positive
skew(x2) # 0에가까움
skew(x3) # 0보다 작은값. 왼쪽으로 긴 모양

```



## 첨도(Kurtosis)

- 데이터가 뾰족한 정도
- 3보다 크면 일반적으로 뾰족하다고 본다
- 정규분포는 첨도가 3임
- Fisher's definition는 정규분포의 첨도를 0으로 만들어서 다시 파악하기 쉽게 만듬. -> scipy 의 kurtosis 사용한다

- 균일분포(즉 뾰족하지 않고 퍼져있으면) 음수, 뾰족할 수록 양수 숫자가 커짐

```python
# 균일분포 ( 빈도가 같음, 뭉툭하고 0 이하)
x1 = [1] * 20 + [2] * 20 + [3] * 20 + [4] * 20 + [5] * 20

# 정규분포에 가까운 값
x2 = [1] * 10 + [2] * 20 + [3] * 40 + [4] * 20 + [5] * 10

# 뾰족한값
x3 = [1] * 5 + [2] * 15 + [3] * 60 + [4] * 15 + [5] * 5

## 막대그래프 생성
# 균일분포
pd.Series(x1).value_counts(sort=False).plot(kind='bar')

# 정규분포에 가까운 그래프
pd.Series(x2).value_counts(sort=False).plot(kind='bar')

# 뾰족한 그래프
pd.Series(x3).value_counts(sort=False).plot(kind='bar')

## sp.kurtosis() 활용
kurtosis(x1) # 평평하다
kurtosis(x2) # 0 에가까워서 정규분포와 비슷
kurtosis(x3) # 뾰족하다
```

