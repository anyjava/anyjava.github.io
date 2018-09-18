---
layout  : wiki
title   : BeanScope
summary : Bean scope 에 대한 설명
date    : 2018-09-18 20:05:34 +0900
updated : 2018-09-18 20:11:14 +0900
tags    : spring, bean_scope
toc     : true
public  : true
parent  : Spring
latex   : false
---
* TOC
{:toc}

# Bean Scope

* Spring Bean 의 생성 주기를 설정할 수 있다.
	* org.springframework.beans.factory.config.ConfigurableBeanFactory.SCOPE_SINGLETON
	* org.springframework.beans.factory.config.ConfigurableBeanFactory.SCOPE_PROTOTYPE
	* SessionScope
	* RequestScope
	* JobScope
	* StepScope

## 주의사항

* org.springframework.beans.factory.config.ConfigurableBeanFactory.SCOPE_PROTOTYPE
	* 실제 `@Autowired` 해서 주입된 bean 을 getter 로 조회 한다면 동일한 객체에 접근이 된다.
	* 따라서 아래와 같은 방법으로 사용하자.
		* Provider<Object> 로 주입 받아아 한다.(javax)
		* ObjectFactory<Object> 로 주입 받는다. (spring)
