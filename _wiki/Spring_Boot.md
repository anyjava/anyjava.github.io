---
layout  : wiki
title   : Spring Boot
summary : Spring Boot 를 사용하면서 알게 된점
date    : 2019-02-01 20:46:11 +0900
updated : 2021-02-14 00:08:53 +0900
tags    : spring, springboot, java
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

## AutoConfig disable 하는 방법 

```
@TestPropertySource(properties=
{"spring.autoconfigure.exclude=comma.seperated.ClassNames,com.example.FooAutoConfiguration"})
@SpringBootTest
public class MySpringTest {...}
```
or
```
@SpringBootTest(classes = {Application.class}
              , webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT
              , properties="spring.autoconfigure.exclude=com.xx.xx.AutoConfiguration"
               )
```

or

```
---
spring:
  autoconfigure.exclude: org.springframework.boot.autoconfigure.session.SessionAutoConfiguration
```

## 멀티모듈 프로퍼티 관리 
* [Spring Boot 2.4 multi module properties & profile-group](https://github.com/kwon37xi/research-spring-boot-2.4/tree/master/profile-group?fbclid=IwAR2ENnGobyZPTZqk1n5osnwlsk35J0iN7nMMKFHHCHXr1mXiEHLKbS0mvws) 

### 참고자료

* [https://stackoverflow.com/questions/26698071/how-to-exclude-autoconfiguration-classes-in-spring-boot-junit-tests](https://stackoverflow.com/questions/26698071/how-to-exclude-autoconfiguration-classes-in-spring-boot-junit-tests)
