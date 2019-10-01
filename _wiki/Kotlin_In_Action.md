---
layout  : wiki
title   : Kotlin In Action
summary : kia study 요약
date    : 2019-10-01 08:50:10 +0900
updated : 2019-10-01 09:14:59 +0900
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
