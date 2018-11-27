---
layout  : wiki
title   : WebFlux
summary : WebFlux 에 대한 내용 정리
date    : 2018-11-27 09:11:40 +0900
updated : 2018-11-27 09:17:28 +0900
tags    : spring, webflux
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# Spring WebFlux 란?
 
* Spring 5.0 부터 지원되는 기능이다.
* 넌블럭킹 방식의 웹서비스를 개발할 수 있다.
* 싱글스레드로 동작을 한다.

## 어떨 때 사용해야 하나? 

* 개념적으로는 node.js 와 유사하다고 볼수 있다. 하지만, 작성일 기준으로 Jdbc Driver 들이 넌블럭킹을 지원하지 않는다.
* 따라서 다음과 같은 상황에서 사용할 수 있다.
 * 타 시스템의 API 조합으로만 비지니스를 구현할 경우
 * 혹인 넌블럭킹 드라이버를 지원하는 데이터스토어를 사용할 경우 (ex. ElasticSearch 등)

## 주의할 점

* WebFlux 를 사용하면, node.js 와 같이 Single Thread 로 동작 하기 때문에 CPU 연산이 많은 작업에는 사용하지 않길 권한다.
* 왜냐하면, CPU 연산이 높게되면 Main 프로세스가 block 되기 때문에 이후 요청들이 모두 block 될 수 있다.


