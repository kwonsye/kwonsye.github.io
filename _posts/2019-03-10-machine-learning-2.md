---
layout: post
comments: true
title: "모두를 위한 딥러닝 기초 - (2) Linear Regression의 개념"
categories:
  - Study Note
tags:
  - deep-learning
  - machine-learning
  - tesorflow
  - linear-regression
---

 <a href="https://www.inflearn.com/course/%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%EB%94%A5%EB%9F%AC%EB%8B%9D-%EA%B0%95%EC%A2%8C/">**인프런-모두를 위한 딥러닝**</a> 을 보고 공부한 내용을 정리했다.

<br>

## Linear Regression 모델
---

기본적으로 supervised learning 이다.

- data(X)값과 label(Y)값이 있는 training dataset을 이용해 model을 train한다.
- 이때 (X,Y)를 좌표평면에 찍었을 때 그 data에 맞는 linear한 직선 함수로 모델의 가설(hypothesis)를 세우는 것
- training dataset에 잘 맞는 linear 선을 찾는 것이 모델이 학습을 하는 과정이다.

<br>

linear 가설 함수의 표준형 : **`H(X) = WX + b`**

그럼 우리의 training data에 잘 맞는 `W`와 `b`를 어떻게 찾을 수 있을까?

`H(X) = WX +b`와 우리의 training data 점들과의 거리를 구해서 (= Cost Function ) 그 거리가 **가장 작은 것**이 우리의 data에 맞는 linear 직선일 것이다.

Cost(Loss) Function : cost(W,b) = $$ \frac{1}{m} \sum_{i=1}^m (H(X^i)-y^i)^2 $$

따라서 우리의 목표는 

**`cost(W,b)`**를 최소화하는 `W`와 `b`값을 구하는 것이다. = 이것이 모델을 학습하는 과정인 것이다.

<br>

## Tensorflow로 간단한 Linear Regression 구현
---
```python
import tensorflow as tf

x_train = [1,2,3] # 데이터
y_train = [1,2,3] # 라벨

W = tf.Variable(tf.random_normal([1])) # shape=1
b = tf.Variable(tf.random_normal([1])) # shape=1

hypothesis = W*x_train + b # 우리의 가설
cost = tf.reduce_mean(tf.square(hypothesis - y_train)) # loss function

#cost를 줄이는 방향으로 train
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)

session = tf.Session() # 세션열고
session.run(tf.global_variables_initializer()) # variable 노드 초기화

for step in range(2001):
    session.run(train) # train
    if step%20 == 0: # 20배수 일때만 print
        print("step :", step, "cost :", session.run(cost), "W :", session.run(W), "b :", session.run(b))

#마지막 결과
# step : 2000 cost : 2.9568784e-05 W : [0.9936845] b : [0.01435669]
```
<br>

cost 최소화 알고리즘으로 `Gradient Descent Algorithm` 을 사용했다.

- Gradient Descent 알고리즘
  - cost function이 convex function의 모양일 경우에만 사용 가능
  - cost function의 **최솟값 =** 경사를 따라 내려가다가 **경사의 기울기가 0인 곳 = 미분 값이 0인 지점**을 찾아가는 알고리즘


<br>

## 런타임에 train-dataset 넘기고 생성된 모델로 test해보기
---

런타임에 넘겨야하므로 **placeholder**사용
```python
import tensorflow as tf

x_train = tf.placeholder(tf.float32, shape=[None]) # shape: rank=1,
y_train = tf.placeholder(tf.float32, shape=[None]) # label

W = tf.Variable(tf.random_normal([1]))
b = tf.Variable(tf.random_normal([1]))

hypothesis = x_train*W + b # model
cost = tf.reduce_mean(tf.square(hypothesis - y_train))

optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.01)
train = optimizer.minimize(cost)

session = tf.Session()
session.run(tf.global_variables_initializer())

#train
for step in range(2001):
    _, W_value, b_value, cost_value = session.run([train, W, b, cost] , feed_dict={x_train : [1,2,3,4,5], y_train :[2.1,3.1,4.1,5.1,6.1]})
    if step % 20 == 0 :
        print("step :", step, "cost :", cost_value, "W :", W_value, "b :", b_value)

#마지막 결과
#step : 2000 cost : 6.431228e-07 W : [1.0005189] b : [1.0981266]

#생성된 모델로 test
print("test model result: ",session.run(hypothesis, feed_dict={x_train : [9,10]}))
#test model result:  [10.102797 11.103315]
```

모델 `hypothesis`가 잘 학습되었고 그 모델에 test_X를 넣으면 제대로 된 test_label 값을 던져주는 것을 확인 할 수 있다. 

<br>

<br>

