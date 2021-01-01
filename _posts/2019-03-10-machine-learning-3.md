---
layout: post
comments: true
title: "모두를 위한 딥러닝 기초 - (3) multi-variable (feature)의 Linear Regression"
categories:
  - Develop
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
    - cost(W,b) = $$ \frac{1}{m} \sum_{i=1}^m (H(x_1^i, x_2^i, x_3^i)-y^i)^2 $$


**행렬곱**을 이용하면 multi-variable 입력 값을 쉽게 표현할 수 있다.

위의 예제를 행렬곱으로 나타내면 아래와 같다.

$$\begin{pmatrix}x_1 & x_2 & x_3 \\\end{pmatrix} \begin{pmatrix}w_1 \\ w_2 \\ w_3 \\\end{pmatrix} = \begin{pmatrix}x_1 w_1 & x_2 w_2 & x_3 w_3 \\\end{pmatrix} $$


여러 개의 dataset이 주어지는 경우 아래 사진과 같이 행렬곱으로 쉽게 표현할 수 있다.

<img src="/assets/images/190310/matrix.JPG" title="출처 : 인프런_모두를 위한 딥러닝">

따라서 `hypothesis`는 두 행렬의 곱인 **H(X) = XW**로 표현 할 수 있다! 

`X`는 입력 dataset 행렬, `W`는 우리가 찾아야하는 weight 값 행렬이다. 

<br>

## multi-variable linear regression을 TensorFlow에서 구현하기
---

<script src="https://gist.github.com/kwonsye/47aa6ce5b87341dad3442b0bc561f322.js"></script>

<br>

<br>

## tensorflow로 .csv 파일에서 데이터 읽어오기
---
`.csv` 파일이란 데이터 값들이 `,`로 이어져 있는 텍스트 파일이다.

메모장으로 `input_data.csv`를 만든 후 아래와 같이 입력하고 저장한다. -> 엑셀파일로 저장됨.
```
#EXAM1,EXAM2,EXAM3,FINAL
73,80,75,152
93,88,93,185
89,91,90,180
96,98,100,196
73,66,70,142
53,46,55,101
```

그리고 앞의 코드에서 `x_data`, `y_data` 로 넣어줬던 값 대신 아래와 같이 쓰면 된다!

```python
# csv 파일 읽어오기 -> 파라미터로 파일경로를 넘긴다.
csv_data = np.loadtxt('C:/Users/kwons/Desktop/input_data.csv', delimiter=',' , dtype= np.float)

x_data = csv_data[:, 0:-1] # [행:열] 모든 행의 마지막 열을 제외한 모든 열을 가져옴
y_data = csv_data[:, [-1]] # 모든 행의 마지막 열==label을 가져옴

print(x_data.shape, x_data) # 잘 들어갔는지 확인!
print(y_data.shape, y_data)

... # 앞의 코드와 동일


```
<br>

## .csv 파일이 여러 개라면? -> queue와 batch 사용하기
---

<script src="https://gist.github.com/kwonsye/d73479ff3361c12e338dc78b112a693d.js"></script>

<br>

<br>