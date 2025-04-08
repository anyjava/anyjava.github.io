---
layout  : post
title   : Spring 의존성 없이 도메인 로직 테스트하기 - Money Value Object로 알아보는 효과적인 단위 테스트
summary : Money Value Object 예제를 통해 Spring 의존성 없이 도메인 로직을 효과적으로 테스트하는 방법을 알아봅니다.
date    : 2025-04-03 10:30:00 +0900
tags    : [SpringBoot, DDD, ValueObject, UnitTest, Java, CleanCode, TestCode]
toc     : true
comment : true
public  : true
adsense : true
---

* TOC
{:toc}

# 1. 들어가며

Spring 기반의 애플리케이션을 개발할 때, 테스트 코드 작성은 필수적입니다. 하지만 모든 테스트에 Spring Context가 필요한 것은 아닙니다. 특히 도메인 로직을 테스트할 때는 Spring 의존성 없이도 효과적인 테스트가 가능합니다. 이번 글에서는 Money Value Object를 예제로, Spring 의존성 없이 도메인 로직을 테스트하는 방법과 그 장점을 알아보겠습니다.

# 2. Money Value Object 구현

## 2.1. 기본 구조

Money Value Object는 금액과 통화 정보를 캡슐화한 불변 객체입니다. JPA에서는 `@Embeddable`을 사용하여 엔티티에 내장될 수 있습니다.

```java
@Embeddable
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Money {
    @Column(name = "price", nullable = false)
    private BigDecimal price;

    @Column(name = "currency", nullable = false)
    private String currency;

    private Money(BigDecimal price, Currency currency) {
        validatePrice(price);
        this.price = price;
        this.currency = currency.getCurrencyCode();
    }
}
```

## 2.2. 팩토리 메서드

```java
public static Money wons(BigDecimal price) {
    return new Money(price, Currency.getInstance("KRW"));
}

public static Money dollars(BigDecimal price) {
    return new Money(price, Currency.getInstance("USD"));
}

public static Money of(BigDecimal price, Currency currency) {
    return new Money(price, currency);
}
```

## 2.3. 비즈니스 로직

```java
public Money add(Money money) {
    validateCurrency(money);
    return new Money(this.price.add(money.price), 
                    Currency.getInstance(this.currency));
}

public Money multiply(int multiplier) {
    return new Money(this.price.multiply(BigDecimal.valueOf(multiplier)), 
                    Currency.getInstance(this.currency));
}

private void validatePrice(BigDecimal price) {
    if (price == null) {
        throw new IllegalArgumentException("가격은 null일 수 없습니다.");
    }
    if (price.compareTo(BigDecimal.ZERO) < 0) {
        throw new IllegalArgumentException("가격은 0보다 작을 수 없습니다.");
    }
}
```

# 3. Spring 의존성 없는 단위 테스트의 장점

## 3.1. 빠른 테스트 실행
- Spring Context 로딩이 필요 없어 테스트 실행 속도가 매우 빠릅니다.
- 개발 중 빈번한 테스트 실행이 가능합니다.
- TDD(테스트 주도 개발) 실천이 용이합니다.

## 3.2. 간단한 테스트 설정
- Spring 관련 설정이나 의존성 주입이 필요 없습니다.
- 순수한 JUnit과 AssertJ만으로 테스트가 가능합니다.
- 테스트 코드 작성의 진입 장벽이 낮아집니다.

## 3.3. 도메인 로직 집중
- 외부 의존성 없이 순수한 비즈니스 로직에만 집중할 수 있습니다.
- 테스트 실패 시 원인 파악이 용이합니다.
- 도메인 로직의 견고성을 높일 수 있습니다.

# 4. Money Value Object 테스트 예제

## 4.1. 기본 생성 테스트

```java
@Test
@DisplayName("원화 Money 객체 생성")
void createKoreanWon() {
    // given
    BigDecimal amount = new BigDecimal("1000");

    // when
    Money money = Money.wons(amount);

    // then
    assertThat(money.getPrice()).isEqualTo(amount);
    assertThat(money.getCurrency()).isEqualTo("KRW");
}
```

## 4.2. 연산 테스트

```java
@Test
@DisplayName("같은 통화의 Money 객체 덧셈")
void addMoneyWithSameCurrency() {
    // given
    Money money1 = Money.wons(new BigDecimal("1000"));
    Money money2 = Money.wons(new BigDecimal("2000"));

    // when
    Money result = money1.add(money2);

    // then
    assertThat(result.getPrice()).isEqualTo(new BigDecimal("3000"));
    assertThat(result.getCurrency()).isEqualTo("KRW");
}
```

## 4.3. 예외 케이스 테스트

```java
@ParameterizedTest
@ValueSource(strings = {"-1000", "-0.01"})
@DisplayName("음수 금액으로 Money 객체 생성 시 예외 발생")
void createMoneyWithNegativeAmount(String amount) {
    assertThatThrownBy(() -> Money.wons(new BigDecimal(amount)))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessage("가격은 0보다 작을 수 없습니다.");
}
```

# 5. 실제 엔티티에서의 활용

Money Value Object는 실제 엔티티에서도 쉽게 사용할 수 있습니다:

```java
@Entity
@Table(name = "search_products")
public class SearchProduct {
    @Embedded
    private final Money money;
    
    // 다른 필드들...
}

@Test
@DisplayName("SearchProduct 생성 시 Money 값 객체가 정상적으로 설정되어야 한다")
void createSearchProductWithMoney() {
    // given
    Money money = Money.wons(new BigDecimal("10000"));
    
    // when
    SearchProduct searchProduct = new SearchProduct(
        productId, name, imageUrl, money, category, brand, searchRequest
    );

    // then
    assertThat(searchProduct.getMoney())
        .isNotNull()
        .satisfies(m -> {
            assertThat(m.getPrice()).isEqualTo(new BigDecimal("10000"));
            assertThat(m.getCurrency()).isEqualTo("KRW");
        });
}
```

# 6. 결론

Money Value Object와 같은 도메인 객체를 설계할 때, Spring 의존성을 배제하고 순수한 도메인 로직만으로 구현하면 다음과 같은 이점이 있습니다:

1. **빠른 피드백**: Spring Context 로딩 없이 즉각적인 테스트 실행이 가능합니다.
2. **높은 테스트 커버리지**: 테스트 작성과 실행이 쉬워 더 많은 테스트 케이스 작성이 가능합니다.
3. **도메인 로직 견고성**: 비즈니스 로직에 집중된 테스트로 도메인의 견고성이 향상됩니다.
4. **재사용성**: Spring 의존성이 없어 다른 프로젝트에서도 쉽게 재사용할 수 있습니다.

이러한 접근 방식은 도메인 주도 설계(DDD)의 원칙과도 잘 부합하며, 더 견고하고 유지보수하기 쉬운 코드를 작성하는 데 도움이 됩니다. 특히 비즈니스 로직이 복잡한 엔터프라이즈 애플리케이션에서 이러한 테스트 방식은 더욱 큰 가치를 발휘할 수 있습니다.

# 참고 자료
- [Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
- [Effective Unit Testing](https://www.manning.com/books/effective-unit-testing)
- [Spring Framework Documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/)

