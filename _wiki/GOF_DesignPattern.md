---
layout  : wiki
title   : GoF DesignPattern
summary : GoF 디자인 패턴 
date    : 2019-04-24 08:52:39 +0900
updated : 2021-12-29 18:33:23 +0900
tags    : book, designpattern
toc     : true
public  : true
parent  : Books
latex   : false
adsense : true
---
* TOC
{:toc}

## Chapter 1. 서론


### Composition vs aggregation vs delegation

* Composition
  * 두 연관관계의 객체가 서로 별도로 존재할 수 없다.
  * House 가 만들어질때 Table 도 같이 만들어 져야함.

* aggregation
  * 여러개의 집합관계의 느낌, 단독으로 존재도 가능하고 모아주는 느낌

* delegation
  * 하나의 객체의 역할을 다른 객체에게 위임하고, 위임받은 객체는 독립적이여야 하며 런타임에 어떤객체가 될지 모른다.

* 참고: 
  * [stackoverflow](https://stackoverflow.com/a/1384476/5270692)  
  * [Delegation, Composition and Aggregation 이해하기](https://ryukato.github.io/oop/2012/09/19/delegation-composition-aggregation.html) 


## Chapter 3. 생성패턴

### Sigleton Pattern

* 오직 한개만 제공하는 클래스

```
public class Foo {
	private static Foo instance; 

	private Foo() {}

	public static Foo getInstance() {
		if (instance != null) {
			return new Foo();
		}

		return instance;
	}
}
```

* multi thread 환경에서 Trhead safe 하지 않다.!
	* sychronized keyword 사용하기 -> 성능의 손해가 있다.
	* 이른 초기화: static block 에서 미리 만들어 준다.
	* double checked locking: 변수 선언시 volatile keyword 필요

```
private static volatile Foo instance;

public static Foo getInstance() {
	if (instance != null) {
		synchroized (Foo.class) {
			return new Foo();
		}
	}

	return instance;
} 
```

	* static inner class 사용하기

```
public class Foo {
	private static Foo instance; 

	private Foo() {}

	private static class FooHolder {
		private static final Foo INSTANCE = new Foo();
	}

	public static Foo getInstance() {
		return FooHolder.INSTANCE;
	}
}
```


### 추상 팩토리 패턴 (Abstract Factory Pattern)

* `Abstract Factory`, `AbstractProduct` 두개로 추상화된 인터페이스를 의존하게 된다. 중요한 포인트는 생성되는 객체의 집합을 하나의 팩토리에서 정의를 하는게 중요하다.
  * 집한군을 생성하는게 아니라면 Factory Method Pattern 을 이용하는게 좋을듯 하다.
  * 단점은, 만들어야할 제품군이 추가되는 요구사항에 유연하지 못하다. -> 동적타입언어를 이용한다면 이부분도 유연하게 구현은 가능할듯

* [싱글턴패턴관련](https://lee1535.tistory.com/75?category=819409)

### Prototype Pattern

* 복제 기능을 갖추고 있는 기존 인스턴스를 프로토타입으로 사용해 새 인스턴스를 만들 수 있다.

* Java 에서 Clone
  * Clonable interface 를 구현하면 된다.
  * Java 에서 제공해주는 것은 기본적으로 얕은 복사

* 장점
  * 복잡하게 객체를 만드는 과정을 숨길 수 있다.
  * 비용이 효율적일 수 있다.
  * 추상적인 타입을 리턴할 수 있다.
* 단점
  * 구현 과정이 복잡해질 수 있다. 순환 참조가 있는경우



