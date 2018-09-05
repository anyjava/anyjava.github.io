---
layout  : wiki
title   : Spring Logging
summary : 
date    : 2018-08-28 01:49:43 +0900
updated : 2018-09-05 09:25:23 +0900
tags    : slf4j
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# Spring Application Logging 전략

* 기본적으로 logback 을 사용하자.
* log를 남기는 전략
  * logback 설정은 xml 로 따로 관리하는게 보기 편한다.(spring boot yml에 같이 있는것 보단 따로 명확하게)
	* consoleAppender 를 사용하자.
		* 왜? servlet 에러(Spring 바깥에서 발생하는)등을 catch 하지 못한다.
		* console log 를 monitoring 하는것이 좋다.

# 참고
* http://kwonnam.pe.kr/wiki/java/logback?s[]=logging 
