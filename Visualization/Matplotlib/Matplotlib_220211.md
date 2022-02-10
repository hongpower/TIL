# Matplotlib

`import matplotlib.pyplot as plt`



```python
import matplotlib.pyplot as plt

# empty figure 가져오기
fig = plt.figure()

# a subplot in a figure. subplot의 nrows, ncols가 안적혀있으면 디폴트는 1이다.
ax = fig.subplots()

# axes에 데이터를 plot한다. y값만 입력되었다 지금은. x는 index array로 지정된다 (0부터 시작)
ax.plot([1,2,3,4,5])

plt.show()
```

- `fig = plt.figure` : empty figure 생성



#### subplots

- subplots는 앞에 객체가 fig냐, 아니면 plt냐 따라서 다르게 이용된다(반환값도 다름)

- `fig, ax = plt.subplots(nrows, ncols)`  : 해당 로우와 컬럼만큼의 subplot을 생성한다
  - `fig, ax = plt.subplots(2,2)`
    - `ax[0,0]` 이런식으로 접근
- `변수 = fig.subplot(nrows, ncols)` : fig 안에 해당 로우와 컬럼만큼의 subplot 생성한다. __리턴 값이 지정한 subplot의 개수만큼이다__
  - ax1, ax2 =`fig.subplot(1,2)` : 각 ax에 하나의 subplot 지정
  - axes = `fig.subplot(2,2)` : axes라는 리스트가 생성된것임
    - `axes[0,0].plot(x,y)` 이런식으로 하나씩 접근해야한다

```python
import matplotlib.pyplot as plt

# subplots는 figure랑 ax를 return한다. ax는 array of Axes다.
fig, ax = plt.subplots()

x = [1,2,3,4,5]
y01 = list(map(lambda x: x**2, x))
y02 = list(map(lambda x: x**3, x))

ax.plot(x,y01,color='red',label='pow2')
ax.plot(x,y02,color='blue',label='pow3')

plt.legend()

plt.show()
```

- `plt.legend()` : 범례표시
  - __범례는 label이 없으면 표시 되지 않는다__
- `ax.plot(y)` : x 필수아님.



```python
import matplotlib.pyplot as plt

# 기본 단위가 inch이다
fig = plt.figure(figsize=(10,5))

# subplots(x개수, y개수), add_subplot(x개수, y개수, 위치)
ax01 = fig.add_subplot(1,2,1)
ax02 = fig.add_subplot(1,2,2)

x = [1,2,3,4,5]
y01 = list(map(lambda x: x**2, x))
y02 = list(map(lambda x: x**3, x))

ax01.plot(x,y01,color='r',label='pow2')
ax02.plot(x,y02,color='b',label='pow3')

ax01.legend()
ax02.legend()

# 제목 쓰기
ax01.set_title('x**2')
ax02.set_title('x**3')
plt.show()
```

- `set_title()` : 제목 설정

- `fig.add_subplot(row,column,cnt)` => 해당 figure내에 subplot 추가 (한개 리턴)
- `fig.suplots(row, column)` => 해당 figure 내에 해당 로우와 컬럼만큼의 subplots추가 (여러개 리턴)



```python
import matplotlib.pyplot as plt
import random

fig, ax = plt.subplots(2,2,figsize=(10,10))

x = list(range(50))
y01 = list(random.randint(1,10) for _ in range(50))
y02 = list(random.randint(1,10) for _ in range(50))
y03 = list(random.randint(1,10) for _ in range(50))
y04 = list(random.randint(1,10) for _ in range(50))

# 산점도 그리기
ax[0,0].scatter(x,y01)
ax[0,1].scatter(x,y02,color='r')
ax[1,0].scatter(x,y03)
ax[1,1].scatter(x,y04)

fig.tight_layout()

plt.show()
```

- `scatter(x,y)` => 산점도 그리기



```python
import matplotlib.pyplot as plt
from random import randint

fig, ax = plt.subplots()

# 100개의 데이터가 있고 각 데이터의 숫자는 0부터 9까지다
x = list(randint(0,10) for _ in range(100))

# hist는 x의 히스토그램을 그림. y가 노필요. 
# bins는 히스토그램의 한 세로줄의 개수라고 생각하면돼
ax.hist(x, bins=10)

plt.xticks(list(range(0,50)))

plt.yticks(list(range(0,101,5)))

# xlim, ylim은 현재의 xlim,ylim을 get하거나 설정한다
plt.xlim(0,24)
plt.ylim(0,10)
plt.show()
```

- `hist` : x값만 있으면 된다(plot과 반대), x는 값들이고 y는 빈도수의 개수이다
  - x는 array값을 받는다.
  - `bins`는 숫자일 때는 number of equal-width bins in the range고 sequence라면 bin edge를 지정
  - 예를들어 bins가 [1,2,3,4]라면, 1에서 2미만까지, 2에서 3미만까지 이렇게...

- `xticks()`, `yticks()` : 눈금 지정
- `xlim(left,right)`, `ylim(left,right)` : 범위 지정
- 만약 xlim과 xticks가 충들하면 xlim까지만 범위가 나온다 : 그래서 내가 만약에 xticks를 50까지 해도 xlim이 24면 24까지만 그래프에 보여준다

```python
import matplotlib.pyplot as plt
from random import randint

fig, ax = plt.subplots()

x = list(randint(0,10) for _ in range(100))

#### 이거 같은 그래프그리면 cumulative가 먼저나와야지 그 그래프가 보인다
ax.hist(x,bins=10,color='blue',cumulative=True)
ax.hist(x,bins=10,color='orange')

# range는 항상 list로 만들어주기
# yticks, xticks는 함수다
### 중요한 거는, ylim은 이상이하고, xticks는 range로 하면 이상 미만이되어버리기 때문에 내가 100이라는 tick을 가지고 싶다면 101까지 작성
plt.xticks(list(range(0,11)))
plt.yticks(list(range(0,101,5)))

# 만약 xlim을 안걸어놓으면 양옆에 빈공간이 생긴다
plt.xlim(0,10)
plt.ylim(0,100)

plt.show()
```



```python
import matplotlib.pyplot as plt
from random import randint
import numpy as np

fig, ax = plt.subplots()

x = list(randint(0,1000) for _ in range(20))

print(x)

# boxplot의 중간 선은 mean이아니라 median이다
print(np.mean(x))
print(np.median(x))
ax.boxplot(x)
plt.show()
```

- `boxplot` 



```python
import matplotlib.pyplot as plt
from random import randint
import numpy as np

fig, ax = plt.subplots()

ages = list(randint(0,101) for _ in range(200))

child = list(x for x in ages if x < 20)
young = list(x for x in ages if 20 <= x < 40)
middle = list(x for x in ages if 40 <= x < 60)
old = list(x for x in ages if 60 <= x)

labels = ['child', 'young', 'middle', 'old']
count = [len(child), len(young), len(middle), len(old)]

# startangle의 default는 0이다. counterclockwise from the x-axis
# 출력 순서는 x 리스트의 순서대로
ax.pie(count, labels=labels, autopct='%1.1f%%', counterclock=False, startangle=180)
plt.show()
```

- `pie` : 원그래프 생성
  - `labels` : 레이블 지정
  - `autopct` : 레이블 형태 지정
  - `counterclock` : 디폴트는 true이다
  - `startangle` : 오른쪽 x축을 기준으로 0부터 시작한다



```python
import matplotlib.pyplot as plt
from random import randint

fig = plt.figure()

# add_subplot고, subplots랑 헷갈 ㄴㄴ
ax01 = fig.add_subplot(1,2,1)
ax02 = fig.add_subplot(1,2,2)

x = [1,2,3,4,5]
y = list(randint(1,100) for _ in range(5))

ax01.bar(x,y,color='red')
ax02.barh(x,y,color='blue')

plt.show()
```

- hist vs bar
  - hist : 바그래프들이 빈도수를 보여줌
  - bar : 다른 카테고리의 데이터 비교를 위해 사용