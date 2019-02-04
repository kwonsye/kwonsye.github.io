---
layout: post
comments: true
title: "MarkDown 이용해서 깃블로그 포스팅하기"
categories:
  - Study Note
exerpt: "MarkDown 문법 정리"
tags:
  - markdown
  - git blog

---

## 헤더 사용하기

* # Header 1
* ## Header 2
* ### Header 3
* #### Header 4
* ##### Header 5
* ###### Header 6

## 코드 삽입하기

코드를 한 줄 삽입합니다. `print("Hello World")`

코드를 여러 줄 삽입합니다.
```python
  var str="Hello World"
  print(str)
  #Hello World 출력
```

## Ordered List & Unordered List

* unordered list
* unordered list
  1. ordered list
  2. ordered list
  3. ordered list
    * unordered list
    * unordered list
  4. ordered list
* unordered list

## 링크 걸기

[**링크입니다.**](https://github.com/kwonsye/kwonsye.github.io)

## 인용문

> 좋은 프로그래머란, 일방통행 도로에서도 양쪽을 모두 보고 건너는 사람이다.
>
> <cite>더그 린더</cite>

## YouTube 삽입하기

<div class="embeded reponsive embeded-reponsive-16by9">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/DJsxPnm388g" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## 테이블 넣기

 | column | column |
 |:-------|-------:|
 |  cell1 | cell2  |
 |  cell3 |  cell4 |

## Definition List

<dl>
  <dt>definition title</dt>
  <dd>definition content</dd>

  <dt>definition title2</dt>
  <dd>definition content2</dd>
</dl>

## 그 외

#### Address element

<address>
  Seoul Korea
</address>

#### Abbreviation element

This is Abbreviation.

*[Abbreviation]: abbreviation element

#### strike 줄 긋기

`<strike></strike>`로 <strike>strike줄</strike>을 그을 수 있습니다.

#### italic체

`__ __` 을 이용해서 _이탤릭체_ 를 이용할 수 있습니다.

#### 밑줄 긋기

`<ins></ins>`을 이용해서 <ins>밑줄</ins>을 그을 수 있습니다.

#### large block of code

<pre>
  public static void main(String[] args){
    System.out.println("Hello World");
  }
</pre>

#### 아랫첨자 & 윗첨자

H<sub>2</sub>O

SuperScript<sup>sup</sup>

#### 각주달기

각주를 달 수 있습니다. 내 깃허브로 가기[^1]

[^1]: <https://github.com/kwonsye/kwonsye.github.io>


## 이미지 넣기

![이미지](https://news.mrwebmaster.it/img/copertine/upl/github.jpg "alt text 입니다.")


