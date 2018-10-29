---
layout  : wiki
title   : XSS (cross-site scripting)
summary : 크로스 사이트 스크립팅
date    : 2018-10-29 12:55:36 +0900
updated : 2018-10-29 13:02:42 +0900
tags    : secure, xss
toc     : true
public  : true
parent  : SecureCoding
latex   : false
---
* TOC
{:toc}

# XSS (cross-site scripting)

* 공격방법
 
	* 파라미터로 javascript 를 넘겨서 CORS 를 피해서 공격을 시도 한다.
	* 공격하는 JavaScript 를 이용하여 얼마든지 해당 사이트에서의 Post 요청을 가로챌 수 있다.

* 대응방법
 
	* 파라미터를 view 에 노출할 경우에 HTML 필터링이 필수가 된다.
	* 대부분의 최신의 view template engine 은 필터링이 기본 기능이다. 하지만,  Jsp 의 경우 개발자가 직접 replace 해줘야 한다. 

# 참고자료

* 위키: https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85
