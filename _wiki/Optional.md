---
layout  : wiki
title   : Optional
summary : Optional 사용법 및 팁
date    : 2018-09-18 19:58:14 +0900
updated : 2018-09-18 20:00:21 +0900
tags    : Java, optional
toc     : true
public  : true
parent  : Java
latex   : false
---
* TOC
{:toc}

# Optional

## 사용상 주의 사항

* Optional 은 객체의 Field 로 사용하는걸 권장하지 않는다.
	* Optional 은 member field 초기화 될때 default 값이 null 이다. => 으잉??? 뭐야 Optional 이 Null 인 케이스가 있다.
	* Optional 은 Serializable 하지 않다. 즉, DTO 로 만들어서 Json 으로 변환을 하지 못함.
