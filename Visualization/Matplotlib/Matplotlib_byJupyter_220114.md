# Matplotlib

> 파이썬의 시각화 도구

- 기본설정:

```python
import matplotlib.pyplot as plt

# 매직 명령어 : 주피터 노트북 사용시에 out[]에 그래프/그림을 출력함:
%matplotlib inline

# 한글 문제 해결 (한국어를 지원하는 폰트로 변경):
import platform

from matplotlib import font_manager, rc
plt.rcParams['axes.unicode_minus'] = False

if platform.system() == 'Darwin':  # 맥OS 
    rc('font', family='AppleGothic')
elif platform.system() == 'Windows':  # 윈도우
    path = "c:/Windows/Fonts/malgun.ttf"
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc('font', family=font_name)
else:
    print('Unknown system...  sorry~~~')
```



## Plot

- plt.plot(x,y) 

  - y축만 작성해도 가능
    - plt.plot([10,20,30])
  - x축은 자동으로 1씩 증가

  

- 관련 함수:

  - plt.show() : print()처럼 그래프 화면 보여주는 함수로서 주피터에서는 생략해도 출력함

  - plt.title('',loc=,pad=,fontsize=) : 제목 설정. loc은 위치로 center,left,right가 있다. pad는 제목 칸의 크기라 생각하면 된다

  - plt.yticks([a],[b]) : y의 눈금 설정. 초기 [a]값만 작성 가능

  - plt.xticks([a],[b]) : x의 눈금 설정. 마찬가지로 [a]값만 설정 가능. 만약에 실제 눈금말고 눈금에 값말고 레이블명을 주고 싶다면 b에 작성한다

    - `plt.xticks([10,20,30,40,50,60],[str(i)+'대' for i in range(10,61,10)])` : 첫번째 가로 안에 값을 눈금으로 설정하고 보여지는 값은 10대, 20대, 30대...

  - plt.figure(figsize=(m,n)) : 그래프 크기설정

  - plt.grid(True) : 그리드 생성. True생략 가능

  - plt.xlabel() : x축 이름 설정

  - plt.ylabel() : y축 이름 설정

    :label: 만약에 실수로 `plt.xlabel = ''` 이렇게 작성을 해버리면 xlabel()함수가 작동하지 않는 오류가 생길 수 있으니 꼭 조심하고 만약 생긴다면 주피터 노트북 재실행 하면 됨..(stackoverflow 감사합니다......)

  - plt.legend() : 범례표시, loc 설정 가능 : upper right, upper left, best 등 다양함

  

- 모양관련 속성:

  - plt.plot(x,y,color='') : 선 색깔

  - plt.plot(x,y,linewidth='') : 선의 굵기

  - plt.plot(x,y,linestyle='') : 선의 스타일 4가지 지정 가능 ; dotted,(.) solid(-), dashed,(--) dash_dot(-.)

  - plt.plot(x,y,marker='') : 그래프 선상에서 (x,y)값들 표시 ; 'o','^','>' 등등

  - plt.plot(x,y,markersize=) :

  - plt.plot(x,y,markeredgecolor='') ;mec 라고 단축어 가능 (테두리컬러)

  - plt.plot(x,y,markerdegewidth='') ; mew 라고 단축어가능

  - plt.plot(x,y,markerfacecolor='') ; mfc 라고 단축어 가능 (마커내 컬러)

  - plt.plot(x,y,label='') : 해당 그래프에 이름을 줄 수 있음. 나중에 범례에 나오는 label명임

    

​			

- 여러 플롯(subplot) 생성 방법

  1. plt.subplot() 함수 사용:

     - 일일이 하나씩 subplot을 설정하는 방법으로 간단하나 양이 많을 때 힘들다
     - subplot(인수1,인수2,인수3) : 인수1과 인수2는 각각 행과 열로 몇개의 subplot을 만들지 결정하고, 인수3은 subplot의 위치를 지정한다
       - 예) subplot(2,2,1) : 가로2개 세로2개의 subplot을 생성하고 0번째 (왼쪽위) subplot으로 이동 ; __1부터 시작__
         - subplot(221) 이렇게 콤마없이 작성도가능!

     ```python
     plt.subplot(221)
     plt.plot(np.random.rand(5))
     plt.title('sub1')
     
     plt.subplot(222)
     plt.plot(np.random.rand(5))
     plt.title('sub2')
     
     plt.subplot(223)
     plt.plot(np.random.rand(5))
     plt.title('sub3')
     
     plt.subplot(224)
     plt.plot(np.random.rand(5))
     plt.title('sub4')
     
     
     # 간격 좁히는 함수 tight_layout(n) : n이 커질수로 덜 타이트해짐
     plt.tight_layout(3)
     plt.show()
     ```

     

 2. plt.subplots(m,n) 함수 사용:

    1. subplots()는 두 개의 값을 return하는데 첫번째 값은 그래프 객체 전체의 이름으로 잘 사용하지는 않음 두번째 값은 Axes(섭plot) 객체로 반환한다

       - 형식 : fig, axes = plt.subplots(m,n)
       - axes[m,n] : 섭그래프 위치 지정

       ```python
       fig, axes = plt.subplots(2,2)
       
       axes[0,0].plot(x,y) # 얘는 0이 처음이다
       axes[0,0].set_title('axes1') # axes객체는 제목 설정 함수가 일반 plt에서 하는 것과 다르다!
       
       axes[0,1].plot(x,y)
       axes[0,1].set_title('axes2')
       
       .
       .
       .
       
       ```

       