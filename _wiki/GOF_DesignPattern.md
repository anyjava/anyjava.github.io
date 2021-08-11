---
layout  : wiki
title   : GoF DesignPattern
summary : GoF 디자인 패턴 
date    : 2019-04-24 08:52:39 +0900
updated : 2021-08-11 20:50:08 +0900
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
