---
layout  : post
title   : 효과적인 테스트 커버리지 관리: 비즈니스 로직에 집중하기
summary : 의미 있는 테스트 커버리지를 달성하기 위한 전략과 실천 방안을 알아봅니다.
date    : 2025-04-09 10:30:00 +0900
tags    : [Testing, JUnit, Jacoco, TDD, Java, SpringBoot, CleanCode]
toc     : true
comment : true
public  : true
adsense : true
---

* TOC
{:toc}

# 효과적인 테스트 커버리지 관리: 비즈니스 로직에 집중하기

## 1. 문제 인식

많은 개발 팀들이 테스트 커버리지 80% 이상을 목표로 삼고 있습니다. 하지만 이런 수치적 목표는 몇 가지 문제를 야기할 수 있습니다:

### 1.1. 의미 없는 테스트 코드 증가

```java
@Test
void dto_getter_테스트() {  // 이런 테스트가 필요할까요?
    ProductDto dto = new ProductDto("상품", 1000);
    assertThat(dto.getName()).isEqualTo("상품");
    assertThat(dto.getPrice()).isEqualTo(1000);
}

@Test
void config_빈_등록_테스트() {  // 스프링이 이미 보장하는 것을 우리가 테스트해야 할까요?
    @Configuration
    class TestConfig {
        @Bean
        public RestTemplate restTemplate() {
            return new RestTemplate();
        }
    }
    
    var context = new AnnotationConfigApplicationContext(TestConfig.class);
    assertThat(context.getBean(RestTemplate.class)).isNotNull();
}
```

### 1.2. 테스트 유지보수 비용 증가

- 의미 없는 테스트들로 인해 빌드 시간 증가
- 설정 변경 시 불필요한 테스트 수정 필요
- 테스트 코드 리뷰에 들이는 시간 낭비

### 1.3. 잘못된 품질 지표

```java
@Getter
@Setter
public class SimpleDto {
    private String field1;
    private String field2;
    // ... 50개의 필드
}

// 이런 테스트는 커버리지는 높여주지만 품질 향상에는 기여하지 않습니다
@Test
void 모든_필드_테스트() {
    SimpleDto dto = new SimpleDto();
    dto.setField1("value1");
    assertThat(dto.getField1()).isEqualTo("value1");
    // ... 50개 필드 모두 테스트
}
```

## 2. 해결 방안: 의미 있는 테스트 커버리지 전략

### 2.1. 테스트 제외 대상 명확히 하기

```gradle
jacocoTestReport {
    afterEvaluate {
        classDirectories.setFrom(files(classDirectories.files.collect {
            fileTree(dir: it, exclude: [
                "**/Q*.class",        // Querydsl
                "**/*Test*.*",        // 테스트 클래스
                "**/generated/**",    // OpenAPI 생성 코드
                "**/dto/**",          // DTO 클래스
                "**/config/**",       // 설정 클래스
                "**/Application.class" // 메인 애플리케이션 클래스
            ])
        }))
    }
}
```

### 2.2. 테스트가 필요한 영역 식별하기

#### 2.2.1. 반드시 테스트해야 할 것

1. **도메인 로직이 있는 Entity**
```java
@Entity
public class Order {
    public void apply(Coupon coupon) {
        if (!coupon.isValidFor(this)) {
            throw new InvalidCouponException();
        }
        this.discount = coupon.calculateDiscount(this.amount);
        this.couponUsed = true;
    }
}

@Test
void 주문에_쿠폰_적용시_할인금액이_정확히_계산되어야_한다() {
    // given
    var order = OrderTestFixture.createWithAmount(10000);
    var coupon = CouponTestFixture.createPercentage(10); // 10% 할인

    // when
    order.apply(coupon);

    // then
    assertThat(order.getDiscount()).isEqualTo(1000);
    assertThat(order.isCouponUsed()).isTrue();
}
```

2. **비즈니스 로직이 있는 Service**
```java
@Test
void 재고가_부족한_상품_주문시_실패해야_한다() {
    // given
    var product = ProductTestFixture.createWithStock(0);
    var orderRequest = OrderRequestTestFixture.create(product.getId(), 1);

    // when & then
    assertThatThrownBy(() -> orderService.createOrder(orderRequest))
        .isInstanceOf(OutOfStockException.class);
}
```

#### 2.2.2. 테스트가 불필요한 것

1. **단순 DTO**
```java
@Getter
@AllArgsConstructor
public class ProductResponse {
    private String name;
    private BigDecimal price;
    // 단순 데이터 전달 객체는 테스트가 불필요
}
```

2. **설정 클래스**
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("*");
    }
    // 프레임워크가 보장하는 동작은 테스트 불필요
}
```

## 3. 실천 방안

### 3.1. 테스트 우선순위 정하기

1. 높은 우선순위
   - 핵심 비즈니스 로직
   - 도메인 규칙
   - 중요한 유효성 검사

2. 중간 우선순위
   - 인프라 코드
   - 외부 시스템 연동

3. 낮은 우선순위
   - 설정 클래스
   - 단순 DTO
   - 자동 생성 코드

### 3.2. 테스트 가치 평가 체크리스트

- [ ] 이 테스트는 비즈니스 요구사항을 검증하는가?
- [ ] 이 테스트가 실패하면 실제 문제가 있다는 것을 의미하는가?
- [ ] 이 테스트는 코드의 동작을 명확하게 설명하는가?
- [ ] 이 테스트는 유지보수 비용 대비 충분한 가치가 있는가?

## 4. 결론

테스트 커버리지는 코드 품질을 측정하는 하나의 지표일 뿐입니다. 진정한 목표는 신뢰할 수 있는 소프트웨어를 만드는 것입니다. 따라서:

1. 의미 있는 비즈니스 로직에 집중하여 테스트를 작성하세요
2. 자동 생성된 코드나 단순 구조체는 과감히 제외하세요
3. 테스트의 가치를 항상 고민하고 평가하세요

이를 통해 테스트는 단순한 커버리지 수치가 아닌, 실제 코드 품질 향상에 기여할 수 있습니다.

> "테스트의 목적은 버그를 찾는 것이 아니라, 코드가 의도대로 동작한다는 확신을 주는 것입니다."

## 참고 자료

- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [Jacoco Documentation](https://www.jacoco.org/jacoco/trunk/doc/)
- [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
- [Test Driven Development: By Example](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530)

