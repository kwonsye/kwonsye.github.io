---
layout: post
comments: true
title: "MVC 패턴 이해하기"
categories:
  - Study Note
tags:
  - MVC
  - MVT
  - design-pattern
---
## Intro
---
자주 나오지만 계속 헷갈리는 MVC 패턴을 정리해보려고 한다!

누가 물어봤을 때 쉽고 제대로 설명해줄 수 있었으면 좋겠다..

<br>

## MVC 의 개념
---
MVC는 Model, View, Controller 의 약자로, <a href="https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC">위키피디아</a>를 보면 아래와 같이 설명되어 있다.
```
MVC에서 모델은 애플리케이션의 정보(데이터)를 나타내며, 뷰는 사용자 인터페이스 요소를 나타내고, 컨트롤러는 데이터와 비즈니스 로직 사이의 상호동작을 관리한다. 
```
<br>
이 설명만 보고는 와닿지 않지만 여기까지만 정리해보자.

MVC 패턴은 하나의 애플리케이션을 구성할 때, 그 구성요소를 모델, 뷰, 컨트롤러의 <b>세 가지 역할로 분리</b>해서 각자의 역할에만 집중하게 하자는 개념이다.

<br>
먼저 전체적인 흐름을 정리한 다음에 Mode, View, Controller 각각을 뜯어볼 것이다.

<img>

**1.** user가 UI를 맡고 있는 View에서 화면에 표시된 내용을 변경하게 되면(ex Submit 버튼을 누른다.), Controller가 사용자의 행동을 받아서 사용자의 요청 명령에 응답한다.
<br>

**2.** Controller는 변경이 발생한 View에 해당하는 Model에게 어디가 바뀌었으니 데이터를 변경하라고 연락한다.

**3.** Model의 데이터가 변경됐다면, Model은 관련된 View에게 데이터가 변경되었음을 알린다.

**4.** Model의 변경통지를 받은 View는 변경된 데이터를 Model에게 직접 요청하고 그렇게 가져온 데이터로 화면을 다시 그린다.

**5.** user는 새롭게 그려진 View를 보게 된다.

<br>

## Model
---

## View
---

## Controller
---


### 실제로 MVC 패턴을 적용해서 간단한 앱을 만들어보자
---


## Reference
---
<a href="https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC">위키피디아</a>

<a href="https://bsnippet.tistory.com/13">https://bsnippet.tistory.com/13</a>

<a href="https://m.blog.naver.com/jhc9639/220967034588">https://m.blog.naver.com/jhc9639/220967034588</a>


<a href="https://plposer.tistory.com/33">https://plposer.tistory.com/33</a>

<br>

<br>