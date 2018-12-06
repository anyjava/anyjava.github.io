---
layout  : wiki
title   : MS-SQL
summary : MS-SQL 관련 용어들
date    : 2018-12-06 18:57:05 +0900
updated : 2018-12-06 19:01:50 +0900
tags    : db, ms-sql, dbms
toc     : true
public  : true
parent  : DBMS
latex   : false
adsense : true
---
* TOC
{:toc}

## MS-SQL 의 특징들

### VARCHAR vs NVARCHAR

* VARCHAR : 문자를 무조건 바이트로 저장한다, DBMS 의 Character Set 설정에 따라서 바이트를 문자로 변환하여 사용한다.
* NVARCHAR : 문자를 유니코드로 저장한다. 즉, 모든 언어를 저장할 수 있다.
* 주의해야 할점
	* 문자열 저장 길이를 계산할 때, 혼란이 올 수 있다. ex) varchar(2) 한글 1글자, nvarchar(2) 는 한글 2글자 
	* 따라서, 최근에는 NVARCHAR 를 사용하는 것을 추천한다.
