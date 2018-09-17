---
layout  : wiki
title   : Enum
summary : Enum 사용법 및 팁
date    : 2018-09-18 01:38:37 +0900
updated : 2018-09-18 01:50:39 +0900
tags    : enum, java
toc     : true
public  : true
parent  : Java
latex   : false
---
* TOC
{:toc}

# Enum

## Enum 의 static field 만들기

* 가끔 enum 중에 특정 EnumSet 을 가져와야 할 필요가 있다. 예를 들어서 주문취소 가능한 상태를 리턴한다고 가정하고 아래 코드를 보자
	* final 은 Immutable Set 이여야 함. 
	* static block 에서 초기화를 해주자.
 
```java
pulbic enum OrderStatus {
	PAID("결제완료",  true),
	COMFIRM("주문확인", true),
	DELIVERY("배송중", false),
	DONE("배송완료", false);
	
	private String desc;
	private boolean ableToCancel;
	
	public static final Set<OrderStatus> ABLE_TO_CANCEL_STATUSES;
	
	static {
		Set<OrderStatus> statuses = EnumSet.nonOf(OrderStatus.class);
		for (OrderStatus status : OrderStatus.values()) {
			if (status.ableToCancel) {
				statuses.add(status);
			}
		}
		
		ABLE_TO_CANCEL_STATUSES = Collections.UnmodifiableSet(statuses);
	}
	
	public OrderStatus(String desc, boolean ableToCancel) {
		this.desc = desc;
		this.ableToCancel = ableToCancel;
	}
}

```

## 참고 링크

* [enum 활용사례 3가지 - jojoldu](https://jojoldu.tistory.com/137)
