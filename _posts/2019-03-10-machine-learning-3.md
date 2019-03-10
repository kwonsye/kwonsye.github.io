---
layout: post
comments: true
title: "모두를 위한 딥러닝 기초 - (3) multi-variable (feature)의 Linear Regression"
categories:
  - Study Note
tags:
  - deep-learning
  - machine-learning
  - tesorflow
  - linear-regression
  - multi-variable
---

 <a href="https://www.inflearn.com/course/%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%EB%94%A5%EB%9F%AC%EB%8B%9D-%EA%B0%95%EC%A2%8C/">**인프런-모두를 위한 딥러닝**</a> 을 보고 공부한 내용을 정리했다.

<br>

## Multi-variable Linear Regression
---
여러 개의 입력 data 값이 있다면 `hypothesis`와 `cost function`은 이렇게 달라진다.

- **Hypothesis**
    - H(x<sub>1</sub>, x<sub>2</sub>,x<sub>3</sub> ) = w<sub>1</sub>x<sub>1</sub> +  w<sub>2</sub>x<sub>2</sub> +  w<sub>3</sub>x<sub>3</sub> + b

- **Cost Fuction**
    - cost(W,b) = $$ \frac{1}{m} \sum_{i=1}^m (H(X_1^i, x_2^i, x_3^i)-y^i)^2 $$


**행렬곱**을 이용하면 multi-variable 입력값을 쉽게 표현할 수 있다.

$$\begin{pmatrix}x_1 & x_2 & x_3 \\\end{pmatrix} \begin{pmatrix}w_1 \\ w_2 \\ w_3 \\\end{pmatrix}$$
