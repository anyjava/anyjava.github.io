---
layout  : wiki
title   : JUnit5
summary : JUnit5
date    : 2019-10-23 09:09:38 +0900
updated : 2019-11-16 13:19:00 +0900
tags    : test,junit
toc     : true
public  : true
parent  : Test_Framwork
latex   : false
adsense : true
---
* TOC
{:toc}

## 간단한 특징

* 백기선님 Java 코드를 테스트하는 다양한 방법
  * JUnit5 특징
		* `@DisplayName` 으로 테스트에 대한 설명을 적을 수 있다.
    * `AssertAll` 을 이용해서 multi assert 가능하다. (기존에는 하나가 실패하면 다음 assert 를 실행하지 않음)
    * `assertThrows` 를 이용해서 exception 을 기대할 수 있고, 객체를 받아서 상태를 검증할 수도 있다.
    * 반복테스트 가능
    * [ParameterizedTest 가능](https://www.baeldung.com/parameterized-tests-junit-5)
    * DI 지원
    * 각각의 시점에 다라 extention 방법이 다르다.
    * `@RunWith`-> `@ExtendWith`
      * meta annotation 으로 사용가능하고 `@SpringBooTest` annotaion 에 포함 되어 있다.
    * `@TestContainers` 를 지원함
  * [Jib](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin): Containerize your Gradle Java Project
  * DockerCompose 를 이용해서 IntegrationTest 환경을 구성해 보자. -> Mock 지양
  * [Docker Compose JUnit Rule](https://github.com/palantir/docker-compose-rule): 을 이용하여 Junit 실행시점에 통합환경을 빌드함.
    * 단점: 느리다. JUnit5 에서 tag 기능을 이용해서 테스트 그룹을 나눠서 CI 서버에서만 동작하게 할 수 도 있다.
  * [Chaos Monkey For Spring Boot](https://github.com/codecentric/chaos-monkey-spring-boot)

## 참고자료

* [JUnit 5 Conditional Test Examples - 환경condition 조작](https://www.mkyong.com/junit5/junit-5-conditional-test-examples/)
* [Junit5 with Spring boot](https://cheese10yun.github.io/junit5-in-spring/)

