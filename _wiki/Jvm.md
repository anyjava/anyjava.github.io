---
layout  : wiki
title   : JVM 성능 튜닝
summary : JVM 관련하여 GC 종료, thread dump 보는 방법등을 정리
date    : 2018-12-03 19:31:45 +0900
updated : 2018-12-03 19:40:16 +0900
tags    : devops, jvm, gc, thread
toc     : true
public  : true
parent  : DevOps
latex   : false
---
* TOC
{:toc}

# JVM (Java Virtual Machine) 성능 튜닝

## GC (Gabage Colector) 의 종류와 특징

대부분의 내용은 아래 참고 링크에서 발췌하였음,

* Serial GC
* Parallel GC
* CMS GC
	* 일반적으로 많이 사용함.
* G1 GC
	* 4G 이상의 힙 영역이 매우 큰 머신에서 설정


## 운영시 JVM 상태 모니터링하는 명령어



## 참고 링크

* [[JAVA] 가비지 컬렉터의 배경과 종류](https://okky.kr/article/379036)
