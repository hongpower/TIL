#



## AI

### Machine Learning 

- 함수 기반에서 데이터 기반 프로그래밍으로 변환
- 컴퓨터가 스스로 학습
- x,y를 주면 학습하면서 model(가설)을 만듦. 그리고 테스트를 해봄 x를 넣었을 때 모델을 활용해서 y가 잘 나오는지



#### 아나콘다 가상환경 만들기





## sklearn

scikit learn

tensorflow - 얘가 가장 많이 쓰인대

pytorch



끝!

csv : comma split value 의 약어래

순서 : __데이터 준비-데이터 분할-(학습)준비-학습-예측 및 평가__



- 지도 학습 : 컴퓨터한테 학습시킬 때 문제와 답 (x, y)을 제공하고 너가 지도해라 
  - 예측-linear, 수치 계산
  - 분류-logistic, 확률 계산
    - binary classification : True/False
    - multi ...

- 비지도 학습 : 

- 강화 학습 : 





knn

svm (데이터 간의 거리가 최대한이 되는 선을 긋기)

- 초평면 = margin이 최대화되는 결정 경계
- hard margin SVM : 이상치를 허용하지 않고 최대한 둘을 분리할 수 있게 하는 선
- soft margin SVM : underfitting/ 이상치 어느정도 허용
- kernel Trick : 차원을 추가(초평면)하여 분류 (원안에 원이 있을 때처럼 선으로 가를 수가 없을 때)

decision tree

- 질문(node)가 너무 적으면 underfitting이 생길 수 있음

- 질문(node)가 너무 많아지면 과적합(overfitting)생길 수 있음 : 문제 다 읽지도 않고 문제 번호만 보고 답을 맞춰버릴 수도 있는 것임

ensemble

k-mean 

- 분류하고자 하는 클래스의 개수(k)
- 각 데이터들을 가장 가까운 그룹에 할당
- 각 데이터의 그룹과 중심의 거리 차이의 분산을 최소화