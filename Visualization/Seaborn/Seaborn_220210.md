# Seaborn

- `import seaborn as sns` , `import pandas as pd`



- `sns.get_dataset_names()` => dataset 이름들 보기
- 긴 column이나 row 명 ..으로 숨김처리 없애기:
  - `pd.options.display.max_columns = None`  
  - `pd.options.display.max_rows = None`

- `변수이름 = sns.load_dataset('데이터셋이름')` => 타이타닉이나, 펭귄처럼 seaborn 안에 있는 데이터 셋 불러오기



## Seaborn Graphs:

- `sns.barplot(data=데이터이름,x='컬럼명',y='컬럼명')` => plt.figure를 통해서 띄운 화면 창에 sns 모듈 사용해서 bar 그래프 그릴 수 있음. data매개변수는 내가 불러온 데이터 셋으로 지정해주고 x랑 y를 data내의 컬럼으로 지정해주기
  - `facecolor=` 바 내부 색깔
  - `edgecolor=` 테두리 색깔
  - `alpha=` 투명도 
- `sns.lineplot(data=,x=,y=)` => 선그래프 그리기
  - `linewidth` => 선 굵기
  - `label` => 범례에 보일 이름
- `sns.histplot(data=,x=)` => 히스토그램은 x만 작성 ㅇㅋ
  - `hue=`  color encoding을 하기위한 컬럼명 지정 (즉 카테고리 별로 결과값을 나누고 싶고 컬러로 표현하고 싶을 때)
  - `multiple=fill` => 비율을 그림
- `sns.boxplot(data=,x=,y=)` => x는 필수가 아니나, x를 주면 x카테고리별로 나눠서 결과 띄워줌
- `sns.swarmplot(data=,x=,y=)` => boxplot과 다르게 x에 값이 들어가고 y가 카테고리명이 들어감. 카테고리별 x에 있는 수치를 원으로 그려서  보여줌.
  - y별로 카테고리가 나뉘어져있는 상황에서 hue를 지정하면 각 카테고리 별 내에 또 카테고리를 색깔로 나뉘어 표현
- `sns.scatterplot(data=,x=,y=)` => 산점도
  - `style=컬럼명` : 해당 산점도 그래프의 지표모양(dot)을 컬럼 내 카테고리별로 변경.
    - 예) `style=sex` => 남,여로 그래프의 지표모양이 세모, 동그라미 이런식으로 다르게 지정
  - `size=컬럼명` : 해당 산점도 그래프의 지표모양 크기를 컬럼 내 카테고리별로 변경. 예를들어 여성은 작은 동그라미 남성은 큰 동그라미...
- `sns.violinplot(data=,x=,y=)`
  - `split=True` : hue가 지정되어있을 때(2개로만), split을 True로 바꾸면 바이올린의 절반을 하나의 hue로, 다른 절반을 다른 hue로 지정해서 그림. 비교가 쉬워짐
- `sns.regplot(data=,x=,y=)`=> scatter와 추세선이 포함된 그래프
- `sns.ecdfplot(data=)`=> 경험적 누적분포함수로, 집단의 백분위를 추정할 수 있음. 즉 데이터가 전체 데이터에서 차지하는 비율을 나타냄
  - pandas의 filter 함수 함께 사용하면:
    - like
    - axis : 필터를 가할 axis로 데이터프레임이라면 columns 나 1, series라면 0 또는 index 작성.
  - 예) `sns.ecdfplot(data=penguins.filter(like='bill_', axis='columns'))` => bill로 시작하는 데이터 column을 filter해서 담아노는 penugins data를 plot해라.
- `sns.pairplot(data=)` => 모든 가능한 column들 끼리 조합한 그래프들을 한번에 보여줌
  - `kind=` 만들 pairplot의 그래프 종류, 디폴트는 scatter임!!
    - kind는 그래서 scatter, hist, reg 등 다양함
- `sns.displot(data=,x=,y=)` => dispersion plot으로 모자이크처럼 생겼는데 박스 색깔로 어떤 위치에 데이터가 많은지 보여줌



## 기타:

- `ax` 속성은 한 fig 내에 `add_subplot`을 통해 여러 subplot이 존재할 때 어느 위치의 subplot을 선택할건지 보여줌
  - `sns.histplot(data=penguins,x='body_mass_g', ax=ax01)` 여기서 이 histplot은 ax01에 그려짐 ( 참고로 ax01 은 앞서 `ax01=fig.add_subplot(1,2,1)`이였음
- `penguins['열이름'].fillna(penguins['열이름'].mean())` => 해당 열이름의 값들 중 na값은 해당 열의 평균으로 대체한 데이터
- `plt.tight_layout()` => 레이아웃 타이트하게
- `데이터셋.sort_values('기준컬럼')` : 해당 컬럼 기준으로 정렬
  - `ascending=` => 오름차순/내림차순
  - `inplace=` => 해당 데이터셋에 반영된 결과 저장하기. 즉 inplace가 없으면 정렬한 결과가 저장 X.