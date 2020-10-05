---
layout  : wiki
title   : 트랜잭션
summary : 트랜잭션
date    : 2020-10-06 02:04:00 +0900
updated : 2020-10-06 02:22:36 +0900
tags    : db, transaction
toc     : true
public  : true
parent  : DBMS
latex   : false
adsense : true
---
* TOC
{:toc}

## 주요개념


> 아래 내용들은 '데이터 중심 애플리케이션 설계 07장 트랜잭션'을 참고하였습니다.


* [Dirty Reads](https://en.wikipedia.org/wiki/Isolation_(database_systems)#Dirty_reads)
  * 한 클라이언트가 다른 클라이언트가 썼지만 아직 커밋되지 않은 데이터를 읽는다. 커밋 후 읽기 또는 그보다 강한 격리 수준은 더티 읽기를 방지한다. 

* [Dirty Write](https://en.wikipedia.org/wiki/Write%E2%80%93read_conflict)
  * 한 클라이언트가 다른 클라이언트가 썻지만 아직 커밋되지 않은 데이터를 덮어쓴다. 거의 모든 트랜잭션 구현은 더티 쓰기를 방지한다.

* 읽기 스큐 (비반복 읽기) 
  * 클라이언트는 다른 시점에 데이터베이스의 다른 부분을 본다. 이 문제를 막기 위한 해결책으로 트랜잭션이 어느 시점의 일관된 스냅숏으로부터 읽는 스냅숏 격리를 가장 흔히 사용한다. (REPEATABLE READ) 스냅숏 격리는 보통 다중 버전 동시성 제어(MVCC) 를 써서 구현 한다.

* 갱신 손실
  * 두 클라이언트가 동시에 read-modify-write 주기를 실행한다. 서로 다른 트랜잭션이 수정내용을 후행하는 버젼이 덮으쓰는 경우. 스냅숏 격리 구현 중 어떤 것은 이런 이상 현상을 자동으로 막아주지만 그렇지 않은 것은 수동 잠금(SELECT FOR UPDATE)이 필요하다. 

* 쓰기 스큐
  * 트랜잭션이 무엇인가를 읽고 읽은 값을 기반으로 어떤 결정을 하고 그 결정을 데이터베이스에 쓴다. 그러나 쓰기를 실행하는 시점에는 결의 전제가 더 이상 참이 아니다. 직렬성 격리만 이런 이상 현상을 막을 수 있다.
  * 보통 INSERT 할때 발생하는데, Table 의 UNIQUE INDEX 을 해결하거나 id 기반의 직렬성 격리, 혹의 범위기반의 서술 잠금(predicate lock)이 필요함.


* [팬텀 읽기(Phantom Reads)](https://en.wikipedia.org/wiki/Isolation_(database_systems)#Phantom_reads)
  * 트랜잭션이 어떤 검색 조건에 부합하는 객체를 읽는다. 다른 클라이언트가 그 검색 결과에 영향을 주느 ㄴ쓰기를 실행한다. 스냅쇼 ㅅ격리는 간단한 팬텀읽기는 막아주지만 쓰기 스큐 맥락에서 발생하는 팬텀은 색인 범위 잠금처럼 특별한 처리가 필요하다.
