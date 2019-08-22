---
layout  : wiki
title   : JPA
summary : JPA 활용에 대한 모든 것 
date    : 2018-08-28 09:32:31 +0900
updated : 2019-08-22 13:05:26 +0900
tags    : jpa
toc     : true
public  : true
parent  : Spring
latex   : false
adsense : true
---
* TOC
{:toc}

# JPA (Java Persistence API)

## 복합키 매핑 전략

* 1. @Id annotaion을 여러개로 구성하는 전략
	* 선택해야하는 상황: 단순한 seq 의 조합의 복합키가 아니라 복합키의 각자의 값에 유의미한 데이터가 존재하는 경우, Id 값을 class 로 추출하는것 보단 @Id annotaion 을 이용하여 Entity class 에 노출시켜 주는것이 좋다.
* 2. @EmbeddedId 를 통해서 복합키를 위해 class 를 이용하는 방법
	* 선택해야하는 상황 : 단순 seq 의 조합으로 복합키가 이루어 졌을 경우, depth 구조가 복합하지 않은 경우

## Entity 연관관계 매핑

* 연관관계가 없음 0 으로 나타내고 있을떄,
	* Hibernate 의 `@JoinColumnOrFormular` 를 사용한다.
```
 @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumnOrFormula(column = @JoinColumn(name = "Ref_Pay_No", nullable = false),
            formula = @JoinFormula(value="CASE Ref_Pay_No WHEN 0 THEN NULL ELSE Ref_Pay_No END"))
    private Payment originPayment;
```

### Entity Manager 변경 감지(dirty checking)

* Entity 가 로딩될때, snapshot 을 저장 해둔다.
* flush 가 될때, 변경된게 없는지 체킹해서 update 를 해준다.
	* 참고: [The anatomy of Hibernate dirty checking mechanism](https://vladmihalcea.com/the-anatomy-of-hibernate-dirty-checking/)
	* 좀더 찾아보고 내용추가해야함.

## Hibernate Envers

* envers 를 사용하다 보면, `revinfo` 테이블의 rev 컬럼의 데이터 타입이 integer 로 생성이 된다. 이는 대용량 데이터를 다룰 시 금방 넘쳐버리는 이슈가 있어서 Long type 으로 변경해야함.
* 아래와 같이 Entity 를 추가하면 가능함.

```
/**
 * hibernate envers에서 사용하는 revision 정보를 저장하는 entity
 * default revision entity는 revision number가 {@link java.lang.Integer} 라서, 더 큰 사이즈의 revision number가 필요한 경우에 사용한다.
 */
@Getter
@Setter
@NoArgsConstructor
@Entity
@RevisionEntity
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
@Table(name = "REVINFO")
public class LongRevisionEntity implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @RevisionNumber
    @EqualsAndHashCode.Include
    @Column(name = "REV")
    private Long id;

    @RevisionTimestamp
    @EqualsAndHashCode.Include
    @Column(name = "REVTSTMP")
    private Long timestamp;


    @Transient
    public Date getRevisionDate() {
        return new Date(timestamp);
    }

    @Override
    public String toString() {
        return String.format("LongRevisionEntity(id = %d, revisionDate = %s)",
            id, DateFormat.getDateTimeInstance().format(getRevisionDate()));
    }

}
```
