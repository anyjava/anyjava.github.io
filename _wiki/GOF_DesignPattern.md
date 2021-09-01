---
layout  : wiki
title   : GoF DesignPattern
summary : GoF 디자인 패턴 
date    : 2019-04-24 08:52:39 +0900
updated : 2021-09-01 20:18:01 +0900
tags    : book, designpattern
toc     : true
public  : true
parent  : Books
latex   : false
adsense : true
---
* TOC
{:toc}

## Chapter 1. 서론


### Composition vs aggregation vs delegation

* Composition
  * 두 연관관계의 객체가 서로 별도로 존재할 수 없다.
  * House 가 만들어질때 Table 도 같이 만들어 져야함.

* aggregation
  * 여러개의 집합관계의 느낌, 단독으로 존재도 가능하고 모아주는 느낌

* delegation
  * 하나의 객체의 역할을 다른 객체에게 위임하고, 위임받은 객체는 독립적이여야 하며 런타임에 어떤객체가 될지 모른다.

* 참고: 
  * [stackoverflow](https://stackoverflow.com/a/1384476/5270692)  
  * [Delegation, Composition and Aggregation 이해하기](https://ryukato.github.io/oop/2012/09/19/delegation-composition-aggregation.html) 


## Chapter 3. 생성패턴

### 추상 팩토리 패턴 (Abstract Factory Pattern)

* `Abstract Factory`, `AbstractProduct` 두개로 추상화된 인터페이스를 의존하게 된다. 중요한 포인트는 생성되는 객체의 집합을 하나의 팩토리에서 정의를 하는게 중요하다.
  * 집한군을 생성하는게 아니라면 Factory Method Pattern 을 이용하는게 좋을듯 하다.
  * 단점은, 만들어야할 제품군이 추가되는 요구사항에 유연하지 못하다. -> 동적타입언어를 이용한다면 이부분도 유연하게 구현은 가능할듯

* [싱글턴패턴관련](https://lee1535.tistory.com/75?category=819409)
