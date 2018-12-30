---
layout  : wiki
title   : QueryDsl
summary : QueryDsl 설정 방법과 tip 들
date    : 2018-12-30 18:41:09 +0900
updated : 2018-12-30 19:50:22 +0900
tags    : jpa, querydsl
toc     : true
public  : true
parent  : JPA
latex   : false
adsense : true
---
* TOC
{:toc}

## Gradle 설정방법

* gradle plugin 추가
 
```
buildscript {
  repositories {
    maven {
      url "https://plugins.gradle.org/m2/" }
  }
  dependencies {
    classpath "gradle.plugin.com.ewerk.gradle.plugins:querydsl-plugin:1.0.10"
  }
}

apply plugin: "com.ewerk.gradle.plugins.querydsl"
```

* querydsl 의 QEntity 생성 폴더 변경
 
```
compile "com.querydsl:querydsl-jpa:$queryDslVersion"
 
 
ext {
    querydslSrcDir = 'src/main/generated'
}
 
 
querydsl {
    library = "com.querydsl:querydsl-apt"
    jpa = true
    querydslSourcesDir = querydslSrcDir
}
 
sourceSets {
    main {
        java {
            srcDirs += file(querydslSrcDir)
        }
    }
}
 
idea {
    module {
        generatedSourceDirs += file(querydslSrcDir)	// just hint
    }
}
```

## trouble shooting 

* compileQueryDsl 단계에서 계속 annotation 들을 찾을수 없다면서 에러가 발생할때,
	* 에러메시지: `error: package org.springframework.boot does not exist`
	* 조치방법: 혹시 **Spring Initializr** 를 통해서 프로젝트를 생성했다면, `compile('org.springframework.boot:spring-boot-starter-data-jpa')`가 compile 이 아니고 `implementation` 으로 되어 있진 않은지 확인해 보길 바란다.
	 
## 참고자료

* [com.ewerk.gradle.plugins.querydsl](https://plugins.gradle.org/plugin/com.ewerk.gradle.plugins.querydsl)
