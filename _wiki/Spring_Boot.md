---
layout  : wiki
title   : Spring Boot
summary : Spring Boot 를 사용하면서 알게 된점
date    : 2019-02-01 20:46:11 +0900
updated : 2019-02-01 20:47:54 +0900
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


### 참고자료

* [https://stackoverflow.com/questions/26698071/how-to-exclude-autoconfiguration-classes-in-spring-boot-junit-tests](https://stackoverflow.com/questions/26698071/how-to-exclude-autoconfiguration-classes-in-spring-boot-junit-tests)
