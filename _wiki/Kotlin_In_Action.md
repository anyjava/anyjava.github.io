---
layout  : wiki
title   : Kotlin In Action
summary : kia study 요약
date    : 2019-10-01 08:50:10 +0900
updated : 2019-10-23 09:01:12 +0900
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


## 8장 고차 함수: 파라미터와 반환 값으로 람다 사용

### 고차함수 (lambda)
* 고차함수: 다른 함수를 인자로 받꺼나 함수를 반환하는 함수다
* function type 
  * example

```kotlin
val sum = { x: Int, y: Int -> x + y }
val action = { println(42) }
```

* null 이 될 수 있는 함수타입도 정의 가능하다.
  * null 이 반환될 수 있는 타입: `var canReturn Null: (Int, Int) -> Int? = {x, y -> null }`
  * null 이 될 수 있는 타입: `var funOrNull: ((Int, Int) -> Int)? = null`

* Java 에서 코틀린 함수 타입 사용
  * 컴파일된 코드안에서 함수 타입은 일반 인터페이스로 바뀐다. FuntionN 형태
  * Funtion0<R>, Funtion1<R1, R> 등 invoke method 에 함수의 본문이 들어감

* 함수 파라미터의 디폴트 함수도 설정 가능


### inline 함수: 람다의 부가 비용 없애기

* keyword: `inline`, `noinline`
* inline: 컴파일 시점에 함수를 풀어서 코드내에 녹여지는 것.
* 코틀린이 보통 람다를 무명 크래스로 컴파일하지만 그렇다고 람다 식을 사용할 때마다 새로운 클래스가 만들어지지는 않는다.
* 람다가 변수를 포획하면 람다가 생성되는 시점마다 새로운 무명 클래스 객체가 생긴다. (주의)

* inline 할 수 있는 경우: 
  * 인라인 함수의 본문에서 람다 식을 바로 호출 하는 경우
  * 람다 식을 인자로 전달받아 바로 호출하는 경우


* Kotlin Collection 표준함수인 filter 는 기본적으로 inline 함수 이다. 하지만 asSequence 를 통한 filter 는 람다를 저장해야하기 때문에 inline 함수가 아니다.
  * 따라서,  크기가 작은 컬렉션의 경우 asSequence 를 사용함면 성능이 더 느릴 수 가 있다.


### 함수를 인라인으로 선언해야 하는 경우
* JVM 은 `JIT Compiler` 가 이미 강력한 인라이닝을 지원하고 있음.

* inline 선언시 이점
  * 1. 함수 호출 비용을 줄일 수 있을 뿐 아니라 람다를 표현하는 클래스와 람다 인스턴스에 해당하는 객체를 만들 필요도 없어진다.
  * 2. JVM은 함수 호출과 람다를 인라이닝해 줄 정도로 똑똑하지 못하다.
  * 3. 사용할 수 없는 몇가지 기능을 사용할 수 있다. (ex. non-local 반환)

* `non-local return`
  * 람다로부터만 반환되는 게 아니라 그 람다를 호출하는 함수가 실행을 끝내고 반환된다.
  * inline 함수인 경우에만 가능
  * 무명함수는 기본적으로 `local return`
  * 레이블을 이용하여 return 하고자하는 function 을 지정할 수 있음,
  * 간단하게는 `return` 은 가장 가까은 `fun` 구문을 빠져다온다고 보면 된다.

```
fun lookForAlice(people: List<Person>) {
	people.forEach {
    if (it.name == "Alice") {
      println("Found!")
      reutrn // non-local return
    }
  }
}
```
 
## 9장. 제네릭스  

### 제네릭 타입 파라미터

* Kotlin 은 처음부터 제네릭을 도입했기 때문에 raw type 을 지원하지 않는다. 제네릭 타입의 타입 인자를 항상 정의 해야 한다.
  * start projection `*` 을 사용하면 할수는 있다.

```kotlin
fun <T> List<T>.slice(indices: IntRange): List<T>
    ___ 파라미터 선언부
            ___ 타입 파라미터가 수신 객체와 반환 타입에 쓰인다.
``` 

* Type Parameter constraint (타입 파라미터 제약)
  * Upper bound (상한): 제네릭 타입을 인스턴스화할 때 사용하는 타입 인자는 반드시 그 상한 타입이거나 그 상한 타입의 하위 타입이어야 한다.

```kotlin
fun <T : Number> List<T>.sum() : T

fun <T: Comparable<T>> max(first: T, second: T): T {
  return if (first > second) first else second
}
```

  * 드물지만 둘 이상의 제약을 사용할 수 도 있음 -> `where T : CharSequence, T : Appendable``

* Null 가능성을 제외한 아무런 제약도 필요 없다면 Any? 대신 Any 를 상한으로 사용하라. Default Upper Bound is Any?


### 실행시 제네릭스의 동작: 소거된 타입 파라미터와 살체화된 타입 파라미터

* reify (실체화): 함수를 inline 으로 만들면 타입 인자가 지워지지 않게 할 수 있다.
* 타입소거가 가능한게, 타입인자를 알고 올바른 타입의 값만 각 리스트에 넣도록 컴파일 타임에서 보장해 준다.

```kotlin
>>> fun <T> isA(value: Any) = value is T
Error: Cannot check for instance of erased type : T
```

* 실체화한 타입파라미터를 사용한다면,

```kotlin
inline fun < reified T> isA(value: Any) = value is T // 컴파일 가능
```
* 따라서 자바에서는 사용이 불가능 하다.


### Variance (변성)
