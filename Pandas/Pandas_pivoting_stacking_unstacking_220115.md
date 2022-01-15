# Pandas

## Pivoting

- pivoting method:
  - df.pivot(index,columns,values): 
  - pd.pivot(data,index,colums,values)
  - pd.pivot_table(df,values,index,columns,aggfunc)
  - df.pivot_table(values,index,columns,aggfunc)

:heavy_check_mark: pivot()은 하나의 index/column으로 여러개의 value 처리가 불가능. aggregate없이 처리하기 때문에 같은 값이 있으면 오류가 떠서 왠만하면 pivot_table()을 사용하자

:heavy_check_mark: pivot_table()은 aggregate기능이 있어서 두가지 value홀드 가능. 

:heavy_check_mark: pivot()과 pivot_table()의 인자들 쓰는 순서는 다르니, 헷갈린다면 `index='',colums=''`이렇게 명시해주자

- df = 사용할 데이터 프레임
- index = 키열 or 키열 리스트
- columns = 키열 or 키열 리스트
- values = __수치 data__만 사용
- aggfunc = '집계함수', default는 평균(mean)함수
  - aggfunc=['sum','count']



## 스태킹(stacking) & 언스태킹(unstacking)

> 계층형 인덱스의 특정 수준도 회전이 가능하다?

### 스태킹 :

- 컬럼 레이블과 그 값을 로우 인덱스(하위 인덱스)와 값으로 회전
- df.stack(level=,dropna=) :
  - level : 스태킹을 적용하는 레벨로서 default는 마지막 레벨로 설정되어있다; 즉 스태킹 결과는 항상 마지막 레벨로 이동한다
    - int, str, list
  - dropna : default는 True로서 결측치를 제외한다. 스태킹 결과 중 결측치를 어떻게 처리할지를 정한다
- Series로 반환



- 멀티 인덱스/ 멀티 컬럼 생성 방법:

```python
multi = pd.MultiIndex.from_tuples([('weight','kg'),('weight','pounds')])
df = pd.DataFrame([[1,3],[2,4]],index=['cat','dog'],columns=multi)

## 스태킹 실행 하면:
df2 = df.stack()
df2
df2.columns
df2.index
```

![멀티컬럼_스태킹](C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220115070509362.png)

==> 멀티 컬럼의 __하위__컬럼만 __하위__인덱스로 들어가고, 상위컬럼은 그대로 컬럼으로 남아있다



- 하위 컬럼이 아니라 상위 컬럼을 하위인덱스로 변경하고 싶다면:

  `df2.stack(0)` : 디폴트값은 -1이므로

- 하위,상위 컬럼 모두 하위 인덱스로 변경하고 싶다면:

  `df2.stack([0,1])`: 상위 컬럼이 먼저나옴



- value에 결측값을 넣고 싶다면 None 입력

- 결측값이 존재하는 상태에서, stack()을 실행하면 결측값이 있는 행은 삭제된다. 디폴트가 dropna = True라서.

<img src="C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220115071837927.png" alt="stack_dropna_false" style="zoom: 80%;" />

<img src="C:\Users\jisuh\AppData\Roaming\Typora\typora-user-images\image-20220115071809425.png" alt="stack_dropna_true" style="zoom:80%;" />

- 첫번째 그림은 dropna가 False일 때인데, cat의 kg인덱스에 두 열 모두 NaN인 것을 알 수 있다. 한 열이라도 결측값이 아닌 값이 있다면 dropna가 True더라도 두번째 그림처럼 삭제되지 않는다! ==> 즉 각 열에 모두 결측값이 있는 경우 삭제



### 언스태킹 :

- 로우 인덱스와 그 값을 컬럼 레이블과 값으로 회전 ( 즉 스태킹의 반대 순서)
- df.unstack(level=,fill_value):
  - fill_value : 언스태킹 결과 중 결측치는 NaN으로 대체
- 마찬가지로 level default는 -1이지만, 0으로 작성한다면 상위인덱스가 컬럼으로 올라간다.