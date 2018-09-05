---
layout  : wiki
title   : JPA
summary : JPA 활용에 대한 모든 것 
date    : 2018-08-28 09:32:31 +0900
updated : 2018-09-05 08:33:18 +0900
tags    : jpa
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# JPA (Java Persistence API)

## 복합키 매핑 전략

* 1. @Id annotaion을 여러개로 구성하는 전략
	* 선택해야하는 상황: 단순한 seq 의 조합의 복합키가 아니라 복합키의 각자의 값에 유의미한 데이터가 존재하는 경우, Id 값을 class 로 추출하는것 보단 @Id annotaion 을 이용하여 Entity class 에 노출시켜 주는것이 좋다.
* 2. @EmbeddedId 를 통해서 복합키를 위해 class 를 이용하는 방법
	* 선택해야하는 상황 : 단순 seq 의 조합으로 복합키가 이루어 졌을 경우, depth 구조가 복합하지 않은 경우

