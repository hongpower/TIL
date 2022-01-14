# Pandas

TMI : 원래는 오늘은 쉬려했지만, 7일내내 매일 올린게 아까워서 조금이라도 공부하고 올리는 TIL이므로 짧을 수도 있다..

## Dataframe:

- 생성 방법 : pd.DataFrame(data [, index = index_data, columns = colums_data])

- DataFrame의 data는 리스트, 딕셔너리, array, series, dataframe의 형태의 데이터를 입력할 수 있음

- index와 columns는 선택사항이라 입력하지않을 시 둘다 0부터 시작

  ```python
  import numpy as np
  import pandas as pd
  
  data1 = np.array([[10,20,30],[40,50,60]])
  date1 = pd.date_range('2022-01-08',periods=2)
  col_list = [1, 2, 3]
  
  # dataframe 생성
  df1 = pd.DataFrame(data1,index=date1,columns=col_list)
  df1
  
  # Index, Columns, values확인
  df1.index
  df1.columns
  df1.values
  ```



- 딕셔너리 형태로 dataframe형성하면 key값은 __index가 아니라 column명으로_ 간다. data도 위에서 아래로 채워진다!

```python
import pandas as pd

df2 = pd.DataFrame({'봄': (20,25,30), '여름' : (30,35,40), '가을' : (22,17,14), '겨울' : (0,5,-7)})
df2

	봄  여름 가을 겨울
0	20	30	22	0
1	25	35	17	5
2	30	40	14	-7
```

 

:heavy_check_mark:pandas는 Numpy와 달리 데이터의 크기가 달라도 연산은 할 수 있으나, __연산을 할 수 있는 항목만__연산을 수행한다! 즉 NaN값이 포함된다

- DataFrame끼리 연산을 수행하면, Series와 비슷하게 크기가 다르다면 같은 인덱스끼리 계산하고 나머지는 NaN(결측값) 처리.

- data.describe(): count, mean, max, min, 25 % 등등 한번에 다양한 연산 결과들을 출력해줌

- head(n) : 처음 n개의 행의 데이터 반환 
- tail(n) : 마지막 n개의 행의 데이터 반환
  - 만약 n개를 지정하지 않으면 default는 5이다
- data[시작위치:끝위치] :series와 마찬가지로 해당 인덱스의 행들을 뽑아낸다(인덱스는 0부터 시작)
