---
layout  : wiki
title   : PostConstruct Annotation
summary : PostConstruct annotion 의 사용법 용례
date    : 2018-09-18 20:01:31 +0900
updated : 2018-09-18 20:13:23 +0900
tags    : java, annoatation
toc     : true
public  : true
parent  : Java
latex   : false
---
* TOC
{:toc}

# PostConstruct annotation

## 설명
* 객체의 생성 직후, 수행되는 method 이다.
* Java 의 표준 annotation 이며, Spring 에서도 Bean 생성 후, 연관관계 주입 후에 수행된다.

## 사용 사례

* 어디서 많이 사용할까?
	* 한번에 대량의 데이터를 미리 읽어 둘때, ex) 지역코드 정보, 공통 코드 정보
	* 사용자의 명령이나 행동 패턴에 따른 Command Object 를 Map에 저장해 두고 싶을때,
		* 사용자의 명령 commaon String 이나 enum type 을 Map 의 key 로 지정하고 value 에는 Command 객체를 미리 매핑해두고 로직에서 사용한다.
		* 이렇게 사용하면 if 문을 획기적으로 줄일 수 있다.
		* TODO: examples...
