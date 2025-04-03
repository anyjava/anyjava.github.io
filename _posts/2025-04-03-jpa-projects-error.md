---
layout  : post
title   : Spring Data JPA Projections의 생성자 타입 매칭 오류 해결하기
summary : JPA Projections 사용 시 발생하는 "Missing constructor" 에러의 원인과 해결 방법을 알아봅니다.
date    : 2024-03-21 15:30:00 +0900
tags    : [SpringBoot, JPA, Hibernate, Database, ErrorHandling, Java]
toc     : true
comment : true
public  : true
---

* TOC
{:toc}

# Spring Data JPA Projections의 생성자 타입 매칭 오류 해결하기

## 자주 발생하는 에러 메시지

```
org.hibernate.query.SemanticException: Missing constructor for type 'com.example.SimilarBrandDto' [
    SELECT new com.example.SimilarBrandDto(
        b.id,
        b.name,
        SIMILARITY(b.name, :brandName)
    )
    FROM Brand b
]
```

이 에러는 크게 두 가지 상황에서 발생합니다:
1. 생성자가 없는 경우
2. **생성자의 파라미터 타입이 쿼리 결과와 일치하지 않는 경우**

## 1. 타입 불일치로 인한 Constructor 에러

### 1.1. 흔한 실수 사례
```java
// ❌ 실패하는 케이스
public record SimilarityDto(
    UUID id,
    String name,
    Double similarity  // PostgreSQL의 similarity()는 numeric 타입 반환
) {}
```

```java
// 이 쿼리는 다음 에러를 발생시킵니다
@Query("""
    SELECT new com.example.SimilarityDto(
        b.id, 
        b.name, 
        SIMILARITY(b.name, :keyword)
    ) FROM Brand b
""")
List<SimilarityDto> findSimilar(@Param("keyword") String keyword);

/*
org.hibernate.query.SemanticException: Missing constructor for type 'com.example.SimilarityDto'
Caused by: java.lang.IllegalArgumentException: Could not find matching constructor
*/
```

### 1.2. 해결 방법
```java
// ✅ 성공하는 케이스
public record SimilarityDto(
    UUID id,
    String name,
    Object similarity  // Object로 받아서 변환
) {
    public Double getSimilarity() {
        return ((Number) similarity).doubleValue();
    }
}
```

## 2. 데이터베이스 함수별 반환 타입과 자주 발생하는 에러

### 2.1. COUNT 함수
```java
// ❌ 실패 케이스
public record CountDto(Integer count) {}  // Integer 대신 Long 사용해야 함

/*
org.hibernate.query.SemanticException: Missing constructor for type 'CountDto'
Caused by: java.lang.IllegalArgumentException: No matching constructor
*/
```

```java
// ✅ 성공 케이스
public record CountDto(Long count) {}
```

### 2.2. AVG 함수
```java
// ❌ 실패 케이스
public record AvgDto(Float average) {}  // Float 대신 Double 사용해야 함

/*
org.hibernate.query.SemanticException: Missing constructor for type 'AvgDto'
Caused by: java.lang.IllegalArgumentException: No suitable constructor found
*/
```

```java
// ✅ 성공 케이스
public record AvgDto(Double average) {}
```

## 3. 계산된 필드의 타입 매칭

```java
// ❌ 실패 케이스
public record PriceDto(
    Long id,
    Integer calculatedPrice  // 계산 결과는 보통 Double
) {}

@Query("""
    SELECT new com.example.PriceDto(
        p.id,
        p.price * (1 - p.discount)
    ) FROM Product p
""")

/*
org.hibernate.query.SemanticException: Missing constructor for type 'PriceDto'
Caused by: java.lang.IllegalArgumentException: Parameter type mismatch
*/
```

```java
// ✅ 성공 케이스 1: 명시적 캐스팅
@Query("""
    SELECT new com.example.PriceDto(
        p.id,
        CAST(p.price * (1 - p.discount) AS int)
    ) FROM Product p
""")

// ✅ 성공 케이스 2: Object로 받기
public record PriceDto(
    Long id,
    Object calculatedPrice
) {
    public Integer getCalculatedPrice() {
        return ((Number) calculatedPrice).intValue();
    }
}
```

## 4. 해결 전략

### 4.1. 디버깅을 위한 SQL 로그 활성화
```properties
# application.properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

### 4.2. 안전한 타입 변환 유틸리티
```java
public class ProjectionUtils {
    public static Double toDouble(Object value) {
        if (value == null) return null;
        if (value instanceof Number) {
            return ((Number) value).doubleValue();
        }
        throw new IllegalArgumentException(
            String.format("Cannot convert %s to Double: %s", 
                value.getClass(), value)
        );
    }
}
```

### 4.3. Native Query 사용
```java
@Query(
    value = """
        SELECT 
            id,
            name,
            CAST(SIMILARITY(name, :keyword) AS float8) as similarity
        FROM brands
        """,
    nativeQuery = true
)
List<SimilarityProjection> findSimilar(@Param("keyword") String keyword);
```

## 결론

Spring Data JPA Projections에서 생성자 타입 매칭 오류를 해결하기 위한 핵심 포인트:

1. 데이터베이스 함수의 실제 반환 타입 확인
2. 에러 메시지를 통한 정확한 원인 파악
3. `Object` 또는 `Number`를 사용한 유연한 타입 처리
4. 필요한 경우 명시적 타입 캐스팅 사용

이러한 가이드라인을 따르면 "Missing constructor" 에러를 효과적으로 해결하고 안정적인 Projection을 구현할 수 있습니다.

#SpringBoot #JPA #Hibernate #DatabaseProjection #ErrorHandling #JavaTips

