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

**업데이트** 19.03.06

## Intro
---
자주 나오지만 계속 헷갈리는 MVC 패턴을 정리해보려고 한다!

누가 물어봤을 때 쉽고 제대로 설명해줄 수 있었으면 좋겠다..

<br>

## MVC 의 개념
---
MVC는 Model, View, Controller 의 약자로, <a href="https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC">위키피디아</a>를 보면 아래와 같이 설명되어 있다.
```
MVC에서 모델은 애플리케이션의 정보(데이터)를 나타내며,

뷰는 사용자 인터페이스 요소를 나타내고, 

컨트롤러는 데이터와 비즈니스 로직 사이의 상호동작을 관리한다. 
```
<br>
이 설명만 보고는 와닿지 않지만 여기까지만 정리해보자.

MVC 패턴은 하나의 애플리케이션을 구성할 때, 그 구성요소를 모델, 뷰, 컨트롤러의 <b>세 가지 역할로 분리</b>해서 각자의 역할에만 집중하게 하자는 개념이다.

<br>
먼저 전체적인 흐름을 정리한 다음에 Mode, View, Controller 각각을 설명할 것이다.

<img src="/assets/images/190305/mvc_2.JPG">

**1.** user가 UI를 맡고 있는 View에서 화면에 표시된 내용을 변경하게 되면(ex Submit 버튼을 누른다.), Controller가 사용자의 행동을 받아서 사용자의 요청 명령에 응답한다.
<br>

**2.** Controller는 변경이 발생한 View에 해당하는 Model에게 어디가 바뀌었으니 데이터를 변경하라고 통지한다.

**3.** Model의 데이터가 변경됐다면, Model은 데이터가 변경되었음을 Controller에게 알린다.

**4.** Model의 변경통지를 받은 Controller는 최종 UI로 출력될 View를 결정하고, View한테 화면을 다시 그리라고 알려준다.

**5.** Controller에게 통지받은 View는 화면을 다시 그리고 user는 새롭게 그려진 View를 보게 된다.

<br>

## Model
---
- 데이터를 가지고 있는 객체이며 모든 데이터와 상태에 대한 정보와 데이터 처리 관련 로직을 가지고 있다.
- Controller에서 Model의 상태를 조작하거나 가져오기 위한 인터페이스를 제공한다.

기본적으로 View나 Controller에 대해서 관심이 없다.

## View
---
- 모델에 포함된 데이터의 시각화를 담당하는 사용자 인터페이스이다.

데이터를 그릴 뿐, 모델이 가지고 있는 정보를 따로 저장해서는 안된다.

데이터를 그릴 뿐, 인터페이스의 행동에 대한 결정은 모두 컨트롤러에게 맡긴다.


## Controller
---
- 애플리케이션의 메인 로직=비지니스 로직이 구현되어 있다.
- 사용자가 View에서 어떤 일을 했는지 해석하고, 그에 따라 Model에게 어떤 데이터를 변경하라고 알려준다.
- Model을 통해서 필요한 데이터를 가져와 응답(Response)을 구성한다.

데이터와 사용자인터페이스 요소들을 잇는 다리역할을 한다.

사용자의 인터렉션을 처리하고, 모델을 조작하며, 최종 UI로 출력될 뷰를 결정한다.

<br>

### MVC 개념을 이해하기 위한 간단한 코드를 작성해보자.
---
열 마디 말보다는 한 줄의 코드가 더 이해가 빠를 수도 있다! MVC 개념 이해를 위한 간단한 코드를 작성해보았다.

<script src="https://gist.github.com/kwonsye/1d35c9772508a1a96eb9337f8ef8969e.js"></script>

<br>

`Main.java` 에서 `user`가 `show..()`를 처리한 요청 결과는 아래 사진과 같다.

<br>

<img src="/assets/images/190305/mvc.JPG">

<br>

### 실제로 MVC 패턴을 적용해서 간단한 앱을 만들어보자
---
(추가 예정)

### MVC 패턴의 장점
---

사용자가 보는 페이지, 데이터처리, 그리고 이 2가지를 중간에서 제어하는 컨트롤.

이 3가지로 구성되는 하나의 애플리케이션을 만들면 서로 <b>분리</b>되어 각자 맡은 역할에만 집중을 할 수 있게 된다.

각 요소 간에 **연결을 보다 느슨하게 구성**할 수가 있게 되기 때문에 복잡한 애플리케이션을 관리하기에 용이하다.

따라서 다른 디자인 패턴처럼
- **유지보수성**
- **애플리케이션의 확장성**
- **유연성**

이 증가한다!


### MVC 패턴의 단점
---
- **Massive ViewController (대규모 MVC 어플리케이션)**
    - Controller가 무지하게 커지고 복잡해질 수 있다.
    - 화면에 복잡한 화면과 데이터의 구성 필요한 구성이라면, Controller에 다수의 Model과 View가 복잡하게 연결되어 있는 상황이 생길 수 있다.


이러한 mvc의 문제점을 보완한 여러 다양한!! 패턴 (MVP, MVVM, Flux, Redux ...)이 생겼다.

<br>

## Reference
---
<a href="https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC">위키피디아</a>

<a href="https://bsnippet.tistory.com/13">https://bsnippet.tistory.com/13</a>

<a href="https://m.blog.naver.com/jhc9639/220967034588">https://m.blog.naver.com/jhc9639/220967034588</a>


<a href="https://plposer.tistory.com/33">https://plposer.tistory.com/33</a>

<a href="https://www.tutorialspoint.com/mvc_framework/mvc_framework_introduction.htm">https://www.tutorialspoint.com/mvc_framework/mvc_framework_introduction.htm</a>

<a href="https://medium.com/@jang.wangsu/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-mvc-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-1d74fac6e256">https://medium.com/@jang.wangsu/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-mvc-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-1d74fac6e256</a>

<br>

## 더 공부할 부분
---
MVP, MVVM

<br>

<br>