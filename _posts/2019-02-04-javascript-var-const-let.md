---
layout: post
comments: true
title: "JavaScript const,var,let 비교"
categories:
  - Study Note
tags:
  - javascript
  - cont
  - var
  - let
---

## Intro
---
예전에 vanilla js로 to-do 페이지를 클론 코딩한 적이 있었는데 오랜만에 복습하려고 다시 들어가보니 localStorage todo-list 관리하는 부분에 문제가 있는 걸 발견했다.

<a href="https://github.com/kwonsye/clock_and_todoList_app">자세히 보기</a>

이 오류를 해결하기 위해서 todo Object에 `newID`를 부여하는 부분을 수정하려고 몇 줄 안되는 코드를 넣었는데 로직은 맞는데 그 짧은 코드가 안돌아가서 헤맸다..맞왜틀?

나중에 보니 `var`이나 `let`대신 당연한 것처럼 `const`를 써놔서 안돌아가는 것이었다..

자바스크립트 기본 실력의 밑바닥이 드러났던 경험이었다^^

그래서 `const`, `var`, `let`을 확실하게 정리해보고자 한다.

<br>

## 핵심 요약
---
- `const` 
    - 상수를 선언하는 키워드
    - 블록 스코프 (block scope) 를 가진다.
    - 호이스팅(hoisting) 되지 않는다.
- `var` 
    - 변수를 선언하는 키워드
    - 함수 스코프 (functional scope) 를 가진다.
    - 호이스팅이 된다.
-  `let` 
    - 변수를 선언하는 키워드
    - 블록 스코프를 가진다.
    - 호이스팅되지 않는다.

<br>

## const VS var, let
--- 
`const` 는 상수를 선언하는 키워드이다. 따라서 값의 재할당이 불가능하고, 선언과 동시에 값을 할당해주어야 한다. (=초기화 해 주어야 한다.)

```
const a = 'a'
const a = 'b' //error
a = 'b' //error

const b //error
b = 'b' //error
```
`var` 과 `let`은 변수를 선언하는 키워드이다. 따라서 값의 재할당이 가능하다. 물론 초기화를 안해줘도 된다.
```
var a = 'a'
let b = 'b'

a = 'c' // no error
b = 'd' // no error

var c;
c = 'c' //no error
```

<br>

## const, let VS var
---
`const`과 `let` 은 블록 스코프를 갖기 때문에 블록 `{}` 안에서만 유효한 사용 범위를 가진다.
```
const a = 'a'
let b = 'b'
{
    const a = 'b' //no error
    console.log(a) //'b'

    let b = 'c' //no error 블록 내의 지역스코프를 갖음
    console.log(b) //'c'

    let c = 'd'
}

console.log(a) //'a'
console.log(b) //'b'
console.log(c) //error -> c는 블록 안에서만 살아있다.
```
반면 `var`는 함수 스코프를 갖기 때문에 블록이고 뭐고 다 덮어 씌워진다.
```
var a = 'a'
{
    var a = 'b'
    var b = 'c'
}

console.log(a) //'b' 전역 스코프가 오염된다.
console.log(b) //'c'
```

`let`, `const`는 ECMA6에서 도입되었으며, `var`로 인해 발생하는 혼란스러운 코드작성을 피하기 위해 만들어졌다.

따라서 `var` 보다 `const`와 `let`을 사용하도록 하자

<br>

## var VS let
---

`let`은 재선언하면 에러를 내준다. 반면 `var`은 재선언을 아무리 해도 에러가 나지 않는다. 따라서 예측하기 어려운 코드를 만든다..
```
var a = 'a'
var a = 'b' //no error
console.log(a) //'b'

let b = 'c'
b = 'd' //no error
console.log(b) //'d'

let b = 'd' //error
```

`var`로 선언한 변수는 <b>호이스팅</b> 된다. 

호이스팅이란 변수 또는 함수의 <b>선언</b>이 코드의 가장 상위로 끌어올려지는 것을 의미한다.

따라서 다음과 같은 코드는 에러가 나지 않는다.
```
//var의 변수 호이스팅
console.log(a) //undefined 선언만 끌어올려진다.
var a = 'a'
console.log(a) //'a'

console.log(b) //undefined
{
    var b = 'b'
}
```
- 참고 : 함수 호이스팅
    - 함수가 함수 선언식 (function declaration) 으로 선언되면 스코프의 최상단으로 호이스팅된다.
    ```
    sayHello() //'hello'
    function sayHello () {
        console.log('hello')
    }
    ```
    - 함수가 함수 표현식 (function expression) 으로 선언되면 호이스팅되지 않는다.
    ```
    sayHello() //error
    const sayHello = function () {
        console.log('hello')
    }
    ```

<br>

반면 `let`은 호이스팅되지 않아 조금 더 예측 가능한 코드를 작성할 수 있다.
```
console.log(a) //error
let a = 'a'
console.log(a) //'a'
```
<br>

## 마무리
---
변수를 사용해야 할 경우 `var` 대신 예측 가능한 `let`을 쓰고 상수를 사용해야 할 경우 `const`를 쓰자!

다음에는 js의 클로저에 대해서도 공부하고 포스팅해야겠다.