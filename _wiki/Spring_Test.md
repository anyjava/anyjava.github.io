---
layout  : wiki
title   : Spring Test
summary : Spring test 를 위한 정리
date    : 2020-02-24 14:13:51 +0900
updated : 2020-02-24 14:56:06 +0900
tags    : spring,test,springboot,junit
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

## 이슈

* `@MockBean`, `@SpyBean`을 사용하면 TestApplicationContext 를 재사용하지 못하기 때문에 테스트 수행속도를 느리게 할 수 있다.
  * 참고 링크
  * [https://www.baeldung.com/spring-tests](https://www.baeldung.com/spring-tests)
  * [https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-testing-spring-boot-applications-mocking-beans](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-testing-spring-boot-applications-mocking-beans) 

