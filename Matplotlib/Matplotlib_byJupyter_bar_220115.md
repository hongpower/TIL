# Matplotlib

## 막대그래프 : bar(), barh()

### bar():

- 이전에 했던 plot이 x값은 생략가능했던 것과 다르게, bar는 x와 height 둘다 작성해야한다
- 형식 : bar(x,y,alpha=) ; alpha는 불투명도

### barh():

- 가로막대 그리기
- 형식: barh(y,height) ; y축이름을 입력하고, height에 값을 입력



### DataFrame을 막대그래프로:

1) df.plot(kind=,grid=,figsize=):
   - kind는 그래프의 종류 : kind='bar','barh',...
   - 한번 이렇게 쓰고 난 이후에는 그냥 이전처럼 plt.xticks.. 이런식으로 진행
   - 만약 하나의 df에서 하나의 열만 가지고 그래프를 그리고 싶다면:
     - `df.[['열이름']].plot()`
   - 정렬한 막대그래프 그리기 : 
     - `df.sort_value(by='나이').plot()`



2. plt.bar(df.변수1,df.변수2):
   - df에서 원하는 변수만 그래프로 만드는 방법
   - df의 변수 개수만큼 n개의 그래프가 생성된다
   - 만약 하나의 변수만 원한다면:
     - `plt.bar(x, df1.키)` : plt.bar는 x와 y축 값이 무조건 필요하기 대문에 



### 유용한 함수들

- plt.text(x,y,'') : x,y좌표에서 시작해서 원하는 텍스트 작성

- plt.yticks(sorted(y)) : y 값들이 오름차순 정렬되어서 tick으로 됨 

- plt.xticks(rotation='') : x축의 레이블을 세로방향이 아닌 가로 방향으로 쓰고 싶다면 rotation='horizontal'이라고 작성

  