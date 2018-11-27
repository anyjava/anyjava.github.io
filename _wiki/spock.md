---
layout  : wiki
title   : spock
summary : spock 으로 테스트 작성 하기
date    : 2018-11-28 08:27:16 +0900
updated : 2018-11-28 08:55:52 +0900
tags    : spock, test, tdd
toc     : true
public  : true
parent  : Test_Framework
latex   : false
---
* TOC
{:toc}

# 왜 Spock 을 선택할까?

* 테스트를 작성할때, 코드를 검증한다는 것 보다 프로그램 명세를 작성하는데 더 의의를 둔다. 프로그래머가 왜 그렇게 코딩 했는지를 코드로 나타낼 수 있기때문이다.
* JUnit5 가 나오면서 많이 개선 되었지만, 이부분은 앞으로 좀더 비교해 봐야할 듯.

# Spock 설정 with gradle

```
	apply plugin: 'groovy'
		
	dependencies {
		testCompile('org.spockframework:spock-core:1.2-groovy-2.4')
		testCompile('org.spockframework:spock-spring:1.2-groovy-2.4')
		testCompile group: 'cglib', name: 'cglib-nodep', version: '3.2.4' // Class Mocking 할 때 필요.
	}
```

# 테스트 방법

## 기본적인 테스트

```
def "method name - 기능설명"() {
    :given "생략가능"
    // 테스트용 데이터 초기화
 
    :when
    // 테스트 대상 코드를 실행하고 실행 결과를 변수 등에 저장
 
    :then
    // 테스트 결과에 대한 검증.
    // 각 줄은 boolean 결과를 내는 Statement 로 작성한다.
    // 만약 한 줄의 코드가 boolean statement가 아니고 복잡한 구문일 경우에는 
    // 그 안의 assert boolean 구문에 'assert somebooleanexpression' 형태로 assert를 붙여야한다.
}
 
// Data Driven 테스트
def "Feature method name #a - #b = #c"(변수 a, 변수 b, 변수 c) {
    expect:
    // 각 데이터 변수로 테스트 수행
 
    where: "각 데이터 변수 설정"
    a | b || c
    1 | 2 || 3
    4 | 5 || 6
}

```

## 파라미터 검증 방법

```
def "파리미터 검증"() {
	when:
	service.doAction();
	
	then:
	1 * fooRepository.save(_ as Foo) >> { argument ->
		assert argument[0].member == EXPECT_VALUE
	}
}
```

# 참고자료

* [Spock으로 테스트하기](https://d2.naver.com/helloworld/568425)
* [권남님 위키](http://kwonnam.pe.kr/wiki/java/spock?s[]=spock)
