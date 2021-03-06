---
layout  : wiki
title   : Gradle
summary : Gradle 에 필요한 것들을 정리
date    : 2019-08-20 09:52:22 +0900
updated : 2021-03-09 09:56:51 +0900
tags    : gradle
toc     : true
public  : true
parent  : index
latex   : false
adsense : true
---
* TOC
{:toc}

# Gradle

### Gradle Docker Build Plugin

* [Docker Build Spring Boot](https://spring.io/guides/gs/spring-boot-docker/)
  * [timestamp 문제](https://github.com/GoogleContainerTools/jib/issues/413)

### Gradle Project Report Plugin

프로젝트의 상태를 리포팅 해준다.

* [project report plugin](https://docs.gradle.org/current/userguide/project_report_plugin.html) 

### Gradle Lombok / JPA AnnotaionProcessor 가 충돌날때
```
compileQuerydsl {
    options.annotationProcessorPath = configurations.querydsl
}

```


### 참고링크

* [Multi-project](https://docs.gradle.org/current/userguide/multi_project_builds.html#multi_project_builds)
* [api vs implementation](https://tjandroid.blogspot.com/2018/11/api-implementation.html)
* [Gradle5 -> 6 마이그레이션](https://jojoldu.tistory.com/538)

