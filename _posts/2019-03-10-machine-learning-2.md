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

`H(X) = WX +b`와 우리의 training data 점들과의 거리를 구해서 (= Cost Function ) 그 거리가 가장 작은 게 우리의 data에 맞는 linear 직선일 것이다.

Cost(Loss) Function : $$\frac{1}{m}\sum_{i=1}^m (H(X^i)-y^i)^2$$


