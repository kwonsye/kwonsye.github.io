---
layout: post
comments: true
title: "모두를 위한 딥러닝 기초 - (1) 머신러닝의 개념과 용어"
categories:
  - Study Note
tags:
  - deep-learning
  - machine-learning
  - tesorflow
  - shape
---

## Intro
---

음성 관련 머신러닝 졸업프로젝트를 진행하면서 꾸역꾸역 얻어걸리는 방식으로 코드를 돌려보다가 딥러닝 개념에 대한 구체적인 이해 없이는 도저히 진행할 수 없는 상황에 이르렀다..

흔들리는 기반 위에서 모래성을 쌓다가 어느 한 군데가 막히면 와르르 무너질 것 같은 기분...?ㅋㅋ

다시 기초로 돌아와서 빠르게 딥러닝의 기본을 공부하기로 했다.

<a href="https://www.inflearn.com/course/%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%EB%94%A5%EB%9F%AC%EB%8B%9D-%EA%B0%95%EC%A2%8C/">**인프런-모두를 위한 딥러닝**</a> 강좌를 공부하기로 했고, 공부하는 김에 직접 돌려본 코드로 정리도 해보자!

## 머신러닝의 용어와 개념 설명
---

```
"머신 러닝이란 (모든 가능한 상황에 대한) 구체적인 프로그래밍 없이 컴퓨터에게 배울 수 있는 능력을 준 것이다." - Arthur Samuel(1959)
```
<br>

### **학습 방법에 따른 분류**

**1.** Supervised Learning

- data(X)에 대한 정답값(Y)이 **labeled** 된 training data set으로 학습한다.
    - Image Labeling : 개/고양이 사진 구분하기
    - 스팸메일 필터링
    - 알파고
- **label/예측값**에 따른 Model 종류
    - Regression : label/prediction 이 어떤 범위 내의 모든 값이 될 수 있다.
    - binary classification : label/prediction이 2개
    - multi-label classification : label/prediction이 여러개 

**2.** Unsupervised Learning
- labeled 되지 않은 training data set으로 학습한다.
- GAN 등등 (이걸 이용해서 프로젝트를 진행하려다가 너무 어려워서 포기했었다..)

<br>

<br>

## Tensorflow의 설치
---
- Tesorflow : **dataflow graph**를 사용해서 숫자 계산을 하는 구글에서 만든 머신러닝 라이브러리

dataflow graph가 뭐지?

- **Dataflow graph**
    - Node : 수학적 operation
    - Edge : multi-dimensional data array (=**tensors**) 들이 노드와 노드 사이를 돌아다닌다.

- tensorflow 설치 방법
```
pip install --upgrade tensorflow
```

<br>

## Tensorflow의 기본적인 operation
---
- tensorflow mechanics 순서

1. 노드와 엣지로 그래프를 build한다.
2. `session.run(op)` 를 통해 그래프에 data(tensor)가 흐르게 해준다.
3. 그 결과로 그래프의 변수들을 업데이트하거나 값을 반환한다.

<br>

```python
import tensorflow as tf

node1 = tf.constant(3.0)
node2 = tf.constant(4.0)

session = tf.Session() #세션열고

node_add = tf.add(node1, node2)
node_sub = node2 - node1 # 이렇게도 노드 생성 가능

print(session.run([node1, node2])) #[3.0, 4.0] 
#session.run()에는 리스트 등 하나의 객체만 들어와야함
print(session.run(node_add)) #7.0
print(session.run(node_sub)) #1.0

```

<br>

- 런타임에 텐서 값들을 던져주고 싶을 때 = **tf.placeholder()**

```python
import tensorflow as tf

node1 = tf.placeholder(tf.float32)
node2 = tf.placeholder(tf.float32)

node_add = node1 + node2

session = tf.Session() #세션열고
print(session.run(node_add , feed_dict={node1:[2, 3.2], node2:[4.2, 9]})) # [ 6.2 12.2]

```

<br>

## tensors의 shape 이해하기
---

텐서플로를 하다보면 shape이 맞지 않아서 계속 오류가 난다..

rank와 shape을 어떻게 계산하는지 알아보자!

- **rank = 차원 계산하는 방법**
    - 처음부터 원소data 가 나올 때까지 `[`가 몇 개 나오는지 센다.

```python
[1,2,3,3,2] # rank 1
[[4,6],[2,1]] # rank 2
[[[[[1,2,3]]]]] # rank 5
```

<br>

- **shape 계산하는 방법**
    - shape은 rank 만큼의 원소개수를 가지고 있다.
    - shape 쓰는 순서는 오른쪽부터 왼쪽으로
    - 원소data가 나오는 안 쪽까지 쭉 들어간 후 첫 `]` 가 나오기 전까지의 원소 개수를 세서 맨 오른쪽에 써준다.
    - 그 한 묶음이 가장 가까운 외부 `[]` 안에 몇 개 들어가는지 세서 왼쪽에 써준다.
    - 또 그 한 묶음이 가장 가까운 외부 `[]` 안에 몇 개 들어가는지 세서 왼쪽에 써준다.

```python
[1,2,7] # shape [3]
[[1,2,3],[5,6,7]] # shape [2,3]
[[[1,3,4]],[[8,6,9]]] # shape [2,1,3]
```
<br>

<br>

