---
layout  : wiki
title   : Spring Boot 프로젝트 초기 설정시 해야 할것
summary : Spring Boot 은 많은 것을 자동으로 설정해 주지만 세세하게 설정해 주어야 할 사항이 있다.
date    : 2019-11-26 10:57:58 +0900
updated : 2019-11-26 11:06:16 +0900
tags    : spring, springboot
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

## DateTime 관련 설정

* LocalDateTime 을 MySql DB에 저장시 반올림이 발생하는 문제
  * 기본적으로 Java 의 `LocalDateTime` 은 nanosecond 까지 지원합니다.
  * Mysql 은 micro second 까지 지원 [https://dev.mysql.com/doc/refman/8.0/en/datetime.html](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)
  * 해결 방안
    * 1안) parameter 로 들어올때, nanosecond 처리 하지 않기, 하지만 도메인 내부에서 충분히 LocalTime.MAX 로 처리하발 발생가능
    * 2안) JpaConveter 로 처리
  * 참고 [권남님 위키](https://kwonnam.pe.kr/wiki/database/mysql/5.6?s)


