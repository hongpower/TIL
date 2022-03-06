# Tensorflow

- 파이참 터미널(cmd)창에서 `conda install tensorflow==1.15 -y`



#### Loss(cost) function

- 예측값(가설)과 실제값(y)의 차이를 확인
- 기울기가 0인 곳을 정답이라고 판별
- 학습을 반복하면서 차이가 최소가 되는 쪽으로 weight과 bias 조정

- 이 때의 예측값과 실제값의 차이는 각 학습(x값과 y이 있는 하나의 케이스)마다 구해지는데 이 오차를 다 합해버려서 평균을 구해서 차이가 최소가 되게 하면 오류가 생김.. (오차가 하나는 +이고 -라면 ?) 그래서 최소제곱합과 같은 개념으로

  - 컴퓨터에게 : 제곱합이 최소가 되는 w와 b값을 찾으라고 명령하는 것이 최종 loss function

  

#### Optimizer

- loss function의 기울기가 최소(0에 수렴할 때까지)가 되는 곳을 찾아줌



#### Activation Function

- 머신러닝에서 중간층이 늘어난 것이 딥러닝이라면
  - 중간층에 기존 머신러닝 방식처럼 그냥 weight만 계산해서 주는거면 머신러닝과 차이점이 없기 때문에
  - 이 때 사용하는 것이 활성화 함수임
- 가중치를 부여해서 나온 값에다가 활성화함수를 거쳐서 새로운 값으로 변경해서 그 값을 다음 노드로 보내는 것이고 반복
- 즉 활성화 함수는 값을 찌부시켜준다..
- 활성화 함수는 여러 종류가 있음
  - 대표적인게 시그모이드 함수
- 활성화 함수가 있어서 선형적이고 단순한 예측 대신에 비선형 예측도 가능해짐
- 만약 중간층말고 최종결과도 시그모이드처럼 0~1사이의 값으로 표현하고 싶다면 출력층에도 시그모이드 적용하면 됨



#### 경사하강법

- w1을 최저값으로 만들 때 사용하는 방식
- 현재 w1 값에서 접선의 기울기를 뺌
- 접선의 기울기가 최소화 될 때 까지 반복



#### Learning rate

- 문제는 경사하강법을 통해 구한 접선의 기울기가 최소인줄 알았는데 더 최소인 부분이 있을 때.
- 기존 방식 : w1 - 기울기
- learning rate반영 방식 : learning rate(w1 - 기울기)



- Tensor : 데이터 저장하는 객체 
- Variable : Weight, Bias
- Operation : H = W * X + b 
  - graph라고 칭함
- Session : 그래프 실행 환경

- :heavy_check_mark: Session을 run하기 전까지 graph는 execute되지 않음!!!!!!! 즉 session run 전에 있는 모든 그래프나 변수 등은 computation을 define할 뿐이지 어떤 value를 홀드하거나 compute하지 않는다!!!!
- :heavy_check_mark: 그래서 print구문 안에 sess.run()을 해야 print가 되는 것임



`import tensorflow as tf` or `import tensorflow.compat.v1 as tf`

- 상수 노드 만들기 : 
  - `node = tf.constant(100)`
  - `node = tf.constant(0.05, dtype=tf.float32)`

- 변수 만들기
  - `W = tf.Variable()`

- 세션 만들기 :
  - `sess = tf.Session()`

- 그래프 실행 :
  - `print(sess.run(node))` : 역방향으로 노드의 연산 수행
  - `print(sess.run([node1, node2]))` : 여러 노드 동시에 실행하고 싶을 때는 __리스트__ 활용



- 난수 만들기 :
  - `tf.random_normal([2,3], mean=1, stddev=4)` : 평균이 1 이고 표준편차가 4인 난수값을 [2,3]shape로 생성. [2,3]은  하나의 리스트 안에 2개의 리스트안에 3개의 요소가 있는 구조
    - tf.random_normal의 shape는 **1차원 integer tensor나 python array**여야함. 그래서 [1] 이렇게 작성. 



- 그래프를 실행하는 시점에 데이터 받아서 노드 만들기 :

```python
node1 = tf.placeholder(dtype=tf.float32)
node2 = tf.placeholder(dtype=tf.float32)

node3 = node2 - node1

sess = tf.Session()

a = [1, 2, 3, 4]
b = [5, 6, 7, 8]

print(sess.run(node3, feed_dict={node1 : a, node2 : b}))
```

- `feed_dict` : placeholder를 사용시 원하는 값을 줄 때 sess.run()안에 작성해서 사용



- 절차:

  - 데이터 준비
  - 가설 설정
  - 준비
  - 학습
  - 예측 및 평가


## Linear Regression

### Linear Regression

```python
import tensorflow as tf

## 1. 데이터 준비
X = tf.placeholder(tf.float32)
y = tf.placeholder(tf.float32)


## 2. 가설 설정
# 첫 W와 b는 랜덤하게 결정 
W = tf.Variable(tf.random_normal([1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')

H = W * X + b


## 3. 준비
# loss function
loss = tf.reduce_mean(tf.square(H - y))

# optimizer
optimizer = tf.train.GradientDescentOptimizer(0.01)
# loss를 최소화 시키는 그래프 :
train = optimizer.minimize(loss)

# session
sess = tf.Session()

# global_variables_initializer
sess.run(tf.global_variables_initializer())


## 4. 학습
# epochs : 학습 횟수
epochs = 5000
for step in range(epochs):
    tmp, loss_val, W_val, b_val = sess.run([train, loss, W, b], feed_dict={X: [1, 2, 3, 4, 5], y: [3, 5, 7, 9, 11]})

## 5. 예측 및 평가
print(sess.run(H, feed_dict={X : [10, 11, 12, 13, 14]}))
```

- Mean Square Error (오차 제곱법) : 예측값과 실제값과의 차이를 제곱해서 평균
  - `tf.reduce_mean(tf.square(H-y))`
- learning rate: 얼마큼씩 경사를 하강할 것인지
- loss 최소화 시키는 그래프 :
  - `optimizer.minimize(loss)`

- global_variables_initializer : 그래프 내의 모든 변수 초기화
- `sess.run`의 return value는 내가 입력한 값들 그대로

### Multi Linear Regression

```python
import tensorflow as tf

# 1. 데이터 준비
# X_data : 3번의 쪽지시험 결과 / y_data : 실제 평가 결과
X_data = [
    [73, 80, 75],
    [93, 88, 93],
    [89, 91, 90],
    [96, 89, 100],
    [73, 66, 70]
]
y_data = [
    [80],
    [91],
    [88],
    [94],
    [61]
]

# shape=[None, 3] : 내부 리스트의 개수는 몇개든 상관없지만 내부 리스트의 요소의 수는 3개
X = tf.placeholder(shape=[None, 3], dtype=tf.float32)
y = tf.placeholder(shape=[None, 1], dtype=tf.float32)

# 2. 가설 설정
# random_normal([]) : 안에 들어가는 값. W는 X와 곱해지기 때문에 행렬연산할 때 X가 [,3] 라면 w는 [3, 1] 이렇게 해야함.
# 생성된 W와 b는 X_data의 하나의 내부 리스트, y_data의 한 내부 리스트만 다루기 때문에 1인 것임.
W = tf.Variable(tf.random_normal([3, 1]), name='weight')
b = tf.Variable(tf.random_normal([1]), name='bias')

# matmul(X, W) : X랑 W 행렬 곱
# 순서 중요!
H = tf.matmul(X, W) + b

# 3. 준비
# loss function
loss = tf.reduce_mean(tf.square(H-y))

# optimizer
learning_rate = 0.00004
optimizer = tf.train.GradientDescentOptimizer(learning_rate)
train = optimizer.minimize(loss)

# session
sess = tf.Session()

# 변수 초기화
sess.run(tf.global_variables_initializer())

# 4. 학습
epochs = 10000
for step in range(epochs):
    _, loss_val, W_val, b_val = sess.run([train, loss, W, b], feed_dict={X: X_data, y: y_data})
    if step % 100 == 0:
        print(f'W: {W_val} \t b: {b_val} \t loss: {loss_val}')

# 5. 예측
print(sess.run(H, feed_dict={X: [[100, 80, 87]]}))
```

- :heavy_check_mark: matmul의 순서 매우 중요!!!!!

```python
import tensorflow as tf

a = tf.constant([[1., 3.]])
b = tf.constant([[4.],[2.]])

a_b = tf.matmul(a, b)

b_a = tf.matmul(b, a)

sess = tf.Session()

print(sess.run(a_b))
# --> [[10.]]
print(sess.run(b_a))
# --> [[ 4. 12.]
#      [ 2.  6.]]
```



## Logistic Regression

### Binary Classification

- Sigmoid 활성화 함수 활용
  - 이진 분류만 가능

```python
import tensorflow as tf

## 1 데이터 준비
X_data = [
    [1, 0],
    [2, 0],
    [5, 1],
    [2, 3],
    [3, 3],
    [8, 1],
    [10, 0]
]
y_data = [
    [0],
    [1],
    [0],
    [0],
    [1],
    [1],
    [1]
]

X = tf.placeholder(shape=[None, 2], dtype=tf.float32)
y = tf.placeholder(shape=[None, 1], dtype=tf.float32)

## 2 가설 설정
W = tf.Variable(tf.random_normal([2, 1]), name='weight')
b = tf.Variable(tf.random_normal([1,]), name='bias')

# sigmoid() : 0과 1사이의 실수를 가지고 판단.(H> 0.5 => True)
# 함수 먼저 생성 후 :
logit = tf.matmul(X, W) + b
# 활성화 함수 적용 :
H = tf.sigmoid(logit)

## 3 준비
# tf의 neuron network가 가진 sigmoid_cross_entropy_with_logits() : loss 값을 미분했을 때 0이 되는 지점이 1개가 아닐 때 사용! (즉 그래프 그렸을 때 기울기가 0이 되는 움푹파인 공간이 여러개일 때)
loss = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=y))

# optimizer
learning_rate = 0.1
optimizer = tf.train.GradientDescentOptimizer(learning_rate)
train = optimizer.minimize(loss)

# session
sess = tf.Session()
sess.run(tf.global_variables_initializer())

## 4 학습
epochs = 10000
for step in range(epochs):
    _, loss_val, W_val, b_val = sess.run([train, loss, W, b], feed_dict={X: X_data, y: y_data})
    if step % 500 == 0:
        print(f'W: {W_val} \t b: {b_val} \t loss: {loss_val}')

## 5 예측
print(sess.run(H, feed_dict={X: [[4, 2], [2, 4]]}))
# 첫번 째 애는 0.5보다커서 pass, 두번 째 애는 0.5보다 작아서 fail
```

- `sigmoid_cross_entropy_with_logits()`: 
- labels : one-hot encoded되어야함. 
  - :white_check_mark: one-hot encoding : 0과 1로 구성되어있는 데이터로 컴퓨터가 읽을 때 1에 해당되는 것만 읽어서 데이터 구분
- logits : sigmoid/softmax 등의 활성화함수 지나기 전 단계, 즉 model의 raw output을 칭함



### Multi Classification

- softmax 활성화 함수 사용
  - 모든 확률의 합은 1에 수렴
  - sigmoid와 다르게 여러개의 분류 가능

```python
import tensorflow as tf
# binary는 0과 1중 둘중 하나였고 이번엔 여러개 분류

# 1.
# 4번의 쪽지시험 -> 결과 : 상/중/하
X_data = [
    [10, 7, 8, 3],
    [8, 8, 9, 4],
    [7, 8, 2, 3],
    [6, 3, 9, 3],
    [7, 6, 7, 5],
    [3, 5, 6, 2],
    [2, 4, 3, 1]
]
y_data = [
    [1, 0, 0],
    [1, 0, 0],
    [0, 1, 0],
    [0, 1, 0],
    [0, 1, 0],
    [0, 0, 1],
    [0, 0, 1]
]

X = tf.placeholder(shape=[None, 4], dtype=tf.float32)
y = tf.placeholder(shape=[None, 3], dtype=tf.float32)

# 2.
# W의 shape의 두번 째 인자가 3인 이유 : (상, 중, 하) 3개로 sigmoid변환된 숫자를 출력할 거라서
# 즉 상중하중 해당 숫자에 0아니면 1 이 나올거니까!!
W = tf.Variable(tf.random_normal([4, 3]), name='weight')
b = tf.Variable(tf.random_normal([3]), name='bias')

logit = tf.matmul(X, W) + b
H = tf.nn.softmax(logit)

# 3.
# loss function
loss = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=logit, labels=y))

# optimizer
train = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(loss)

# session
sess = tf.Session()
sess.run(tf.global_variables_initializer())

# 4.
for step in range(3000):
    # loss_val == cost_val
    _, cost_val = sess.run([train, loss], feed_dict={X: X_data, y: y_data})
    if step % 300 == 0:
        print(f'cost:{cost_val}')

# 5. 결과값 보면 0.9%의 확률로 하라는 것을 보여줌
print(sess.run(H, feed_dict={X: [[4, 9, 8, 5]]}))
```

================여기까지 머신러닝 ===================



## Deep Learning

- 기존의 머신러닝으로는 XOR 문제 해결 x
- 은닉층을 생성함으로써 XOR 문제 해결 가능해짐
- W*X 의 개수가 여러개가 됨

```python
import tensorflow as tf

# 1.
X_data = [
    [0, 0],
    [0, 1],
    [1, 0],
    [1, 1]
]
y_data = [
    [0],
    [1],
    [1],
    [0]
]
X = tf.placeholder(shape=[None, 2], dtype=tf.float32)
y = tf.placeholder(shape=[None, 1], dtype=tf.float32)

# 2. 가설 설정 : 
W1 = tf.Variable(tf.random_normal([2, 10]), name='weight1')
b1 = tf.Variable(tf.random_normal([10]), name='bias1')
layer1 = tf.sigmoid(tf.matmul(X, W1) + b1)

# 히든층 : 10개 들어오고 20개 내보냄
W2 = tf.Variable(tf.random_normal([10, 20]), name='weight2')
b2 = tf.Variable(tf.random_normal([20]), name='bias2')
layer2 = tf.sigmoid(tf.matmul(layer1, W2) + b2)

W3 = tf.Variable(tf.random_normal([20, 10]), name='weight3')
b3 = tf.Variable(tf.random_normal([10]), name='bias3')
layer3 = tf.sigmoid(tf.matmul(layer2, W3)+ b3)

# 출력층 : 2개(X) 받아서 1(y)개 나올거라고 y값이 적혀 있잖아
W4 = tf.Variable(tf.random_normal([10, 1]), name='weight4')
b4 = tf.Variable(tf.random_normal([1]), name='bias4')

logit = tf.matmul(layer3, W4) + b4
H = tf.sigmoid(logit)

# 3.
# loss
loss = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=logit, labels=y))

# optimizer
train = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(loss)

# session
sess = tf.Session()
sess.run(tf.global_variables_initializer())

# 4.
for step in range(10000):
    _, loss_val = sess.run([train, loss], feed_dict={X: X_data, y: y_data})
    if step % 1000 == 0:
        print(f'loss: {loss_val}')

# 5.
print(sess.run(H, feed_dict={X: X_data}))

predict = tf.cast(H > 0.5, dtype=tf.float32)
correct = tf.equal(predict, y)
# bool 값을 float32로 변경해서 평균을 냄 (즉 1/0/0/1.. 이런식으로 된 값들의 평균)
accuracy = tf.reduce_mean(tf.cast(correct, dtype=tf.float32))

# sess.run()을 해야 비로서 accuracy가 실행되고 -> correct가 실행되고 -> predict가 실행
print(sess.run(accuracy, feed_dict={X: X_data, y: y_data}))
```

- `tf.cast(x, dtype=)` : 지정한 dtype으로 x를 cast해줌
  - x에는 조건도 삽입 가능 :
    - tf.cast(x > 0.5, dtype=tf.float32) : x가 0.5보다 크면 1(True)반환, 작으면 0(False) 반환
- `tf.equal(x, y)` : x와 y를 비교해서 bool(True/False)로 리턴. 둘은 같은 type이여함. 

:heavy_check_mark: 은닉층의 b의 random_normal shape의 요소가 하나인 이유는 : b는 한 layer의 노드마다 값이 다를 수 있어도 노드 내의 모든 X에 값이 동일하기 때문에.
