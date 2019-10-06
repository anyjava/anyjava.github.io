---
layout  : wiki
title   : Kotlin In Action
summary : kia study 요약
date    : 2019-10-01 08:50:10 +0900
updated : 2019-10-07 03:00:51 +0900
tags    : kotlin,kia,kotlin in action
toc     : true
public  : true
parent  : Kotlin
latex   : false
adsense : true
---
* TOC
{:toc}

# Kotlin In Action 스터디 정리

* 요약

```
 * 매주 월요일 점심 시간
 * 참석 4명
 * 책일 읽어오고 읽어온 내용에 대해서 공유하기
```

## 지은이의 말 & 옮긴이의 말

* 자바를 대신하기 위한 요구사항 - C#으로 ㄷ개발하는 닷넷팀 개발자들이 부러웠다. Java 진영에서는 찾을 수 없었다. 그래서 만듦
  * 정적 타입의 지정
  * Java 코드와 완전히 호환되는 언어가 필요함
  * 그 언어르 루이한 도구 개발이 쉬워야 했다.
  * 배우기 쉽고 뜻을 파악하기 쉬운 언어가 필요
* 코틀린 언얼르 설계하는 과정에 관여하고 코틀린 언어의 특성이 왜 현재의 모습이 되었는지에 대해 자신있게 설명 할 수 잇는 사람들이 쓴 책이다.

* 코틀린이 제공하는 여러 기능으로 살펴보면 정말 자바를 개선하기 위해 많은 부분에서 고민을 했다는 느낌이 많이 든다.
* 대규모 개발과 유지 보수성, 기존 자바 시스템과의 ㅇ호환성을 등을 고려 해 본다면 가장 강력한 자바 대체재라고 볼 수 있다.


## 1장. 코틀린이 무엇이며 왜 필요 한가?

* 새로운 예약어: `it`, 람다의 파라미터가 1개 일때 it 으로 축약 가능하다.

```
data class Person(val name: String, val age: Int? = null)

fun main(args: Array<String>) {
    val persons = listOf(Person("영희"), Person("철수", age = 29))
    val oldest = persons.maxBy{ it.age ?: 0}
    println("oldest: $oldest")
    
}
```

## 7장 연산자 오버로딩과 기타 관례

### 산술연산자 오버로딩

* 기존 자바 클래스에 대해 확장 함수를 구현하면서 관례에 따라 이름을 붙이면 기존 자바 코드를 바구지 않아도 새로운 기능을 쉽게 부여할 수 있다.k
* `+` 연산자 오버로딩의 예
```
data class Point(val x: Int, val y: Int) {
	operator fun plus(other: Point): Point {
		return Point(x + other.x, y + other.y)
	}
}
```
* `operator` 키워드
* 코틀린은 직접 연산자를 만들수 없고, 언어에서 미리 정해둔 연산자만 오버로딩 할 수 있으며, 관레에 따르기 위해 클래스에서 정의해야 하는 이름이 연산자별로 정해져 있다.
  * 비트 연산자에도 있다. 310page 참고

| 식    | 함수 이름 |
| ____  | _________ |
| a * b | times     |
| a / b | div       |
| a % b | mod       |
| a + b | plus      |
| a - b | minus     |
* 우선순위는 언제나 표준 숫자 타입에 대한 연산자 우선순위와 같다.
* 주의! 코틀린 연산자가 자동으로 교환 법칙을 지원하지는 않음에 유의하라.

### 복합대입연산자 오버로딩
* Compound assignment: ex) +=
  * `plusAssign` `minusAssign`
* `plus` 와 `plusAssign` 을 동시에 정의하지 말라.
  * += 는 plus 와 plusAssign 양쪽으로 컴파일 될 수 있다.
* 코틀린 표준 라이브러리는 컬렉션에 대해 두가지 접근 방법을 함께 제공한다.
  * `+/-` 항상 새로운 컬렉션을 반환
	* `+=` `-=` 연산자는 항상 변경 가능한 컬렉션에 작용해 메모리에 있는 객체 상태를 변화시킨다. mutable collection 에 대해서는 복사본을 생성해서 전달한다.

### 단항 연산자 오버로딩

* unary operator overloading
 
| 식       | 함수 이름  |
| ____     | _________  |
| +a       | unaryPlus  |
| -a       | unaryMinus |
| !a       | not        |
| ++a, a++ | inc        |
| --a, a-- | dec        |

### 비교연산자 오버로딩

* 자바의 equals 나 compareTo 를 사용한 코드보다 더 간결하며 이해하기 쉽다.
* identity equals: ===
  * 오버로딩 할 수 없다.
* Any에서 상속박은 equals 가 확장 함수보다 우선순위가 높기 때문에 equals를 확장 함수로 정희 할수는 없다는 사실에 유의하라.


### 컬렉션과 범위에 대해 쓸 수 있는 관례

* index operator
* `[]`: keyword -> get / set
* `in`: membership test 라 한다. / keyword -> `contains`
* `..`: rangeTo
  * 범위연산자는 우선순위가 낮아서 범위의 메소드를 호출하려면 범위를 괄호로 둘러싸야 한다.
	* `(0..n).forEach { print(it) }`
* `for (x in list)`: operator iterator 내부에선 Iterator interface 를 구현

### 구조 분해 선언과 component 함수

* destructuring decalartion
* 구조 분해를  사용하면 복합적인 값을 분해해서 여러 다른 변수를 한거번에 초기화할 수 있다.
 
```
val p = Point(10, 20)
val (x, y) = p
```
* operator componentN 이라는 함수가 호출됨
  * 위 예제에서 x 에는 component1, y 에는 component2
* data class 의 주 생성자에 들어있는 프로퍼티에 대해서는 컴파일러가 자동으로 componentN 함수를 만들어 준다.  


### 위임 프로퍼티 
