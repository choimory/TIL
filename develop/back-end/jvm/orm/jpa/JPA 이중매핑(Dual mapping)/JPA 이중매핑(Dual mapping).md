# JPA 이중매핑(Dual mapping)

- 이중 매핑(Dual Mapping)은 JPA에서 **같은 데이터**를 **다른 방식으로 두 번 이상 매핑해야 할 경우**를 의미함.
- 즉, 동일한 컬럼이나 속성을 서로 다른 방식으로 매핑하여 사용할 필요가 있을 때 활용됨.
- 이중 매핑은 주로 **복잡한 데이터 모델링 요구**나 **엔티티의 다양한 뷰를 제공**할 때 사용됨.

---

# **이중 매핑이 필요한 경우**

## **엔티티 속성을 다른 형식으로 매핑해야 할 때**:

- 예를 들어, 같은 데이터가 **다른 형식**으로 필요할 경우.
- 날짜를 **포맷팅된 문자열**과 **`Date` 객체**로 동시에 사용해야 하거나, **금액**을 다른 단위로 표시할 때.

## **다른 용도로 같은 데이터를 관리해야 할 때**:

- 데이터베이스의 **외래 키**를 **객체 참조**와 **직접적인 ID** 값 두 가지 방식으로 동시에 다루어야 할 경우.
- 같은 필드를 **일반 필드**와 **임베디드 타입**으로 다르게 사용해야 할 때.

## **엔티티 상태 관리가 서로 다른 방식으로 필요할 때**:

- 특정 컬럼을 **하나의 엔티티**에서는 **읽기 전용**, 다른 엔티티에서는 **수정 가능**하게 처리해야 할 때.

## **복잡한 상속 구조를 처리할 때**:

- 상속 관계에서 **부모 엔티티**의 속성을 **다르게 해석**해야 하는 경우, 두 가지 방식으로 매핑할 수 있음.

---

# **이중 매핑 처리 방법**

## **필드와 관계를 동시에 매핑**

- 하나의 컬럼을 **객체 관계**와 **단순 필드 값** 두 가지로 매핑하는 경우.
- 예를 들어, **외래 키**를 객체와 필드 값으로 동시에 관리할 때 사용할 수 있음.
- **예시: 외래 키를 객체 참조와 ID 값으로 이중 매핑**

    ```java
    @Entity
    public class Order {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        // 고객 객체 참조 (관계 매핑)
        @ManyToOne
        @JoinColumn(name = "customer_id")
        private Customer customer;
    
        // 고객 ID 직접 매핑 (필드 매핑)
        @Column(name = "customer_id", insertable = false, updatable = false)
        private Long customerId;
    
        // Getters and Setters
    }
    
    ```

- **설명**:
    - `customer`는 **객체 참조**를 통해 `Customer` 엔티티와 **다대일 관계**로 매핑됨.
    - 동시에 `customerId`는 **고객의 ID 값**을 직접 매핑한 필드로, **insertable**과 **updatable**을 `false`로 설정하여 **수정 불가**로 처리함.
    - 이렇게 하면 **객체 참조 방식**과 **단순 필드 방식** 모두로 같은 외래 키를 처리할 수 있음.

## **포맷팅된 값과 실제 값을 동시에 매핑**

- 데이터베이스의 값은 하나지만, 이를 **여러 가지 형태**로 표현할 필요가 있을 때. 예를 들어, **날짜**를 **`Date` 객체**와 **포맷팅된 문자열**로 동시에 사용할 수 있음.
- **예시: 날짜 필드를 포맷팅된 문자열과 `Date`로 이중 매핑**

    ```java
    @Entity
    public class Event {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @Temporal(TemporalType.DATE)
        private Date eventDate;
    
        @Formula("DATE_FORMAT(event_date, '%Y-%m-%d')")
        private String formattedDate;
    
        // Getters and Setters
    }
    
    ```

- **설명**:
    - `eventDate`는 **`Date` 객체**로 매핑되어 **날짜 형식**으로 데이터베이스에 저장됨.
    - `formattedDate`는 `@Formula`를 사용하여 **날짜를 특정 형식**으로 변환한 값이 제공됨. 이렇게 하면 동일한 컬럼을 두 가지 방식으로 사용할 수 있음.

## **읽기 전용 필드와 쓰기 가능한 필드를 함께 매핑**

- 특정 필드를 **읽기 전용**으로 다루어야 할 때와 **수정 가능**하게 다루어야 할 때, 동일한 필드를 이중 매핑으로 처리할 수 있음.
- **예시: 읽기 전용 필드와 수정 가능한 필드로 이중 매핑**

    ```java
    @Entity
    public class Product {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        // 수정 가능한 가격 필드
        @Column(name = "price")
        private BigDecimal price;
    
        // 읽기 전용 가격 필드
        @Column(name = "price", insertable = false, updatable = false)
        private BigDecimal readOnlyPrice;
    
        // Getters and Setters
    }
    ```

- **설명**:
    - `price` 필드는 **수정 가능한 필드**로, **가격 변경**이 가능함.
    - 반면, `readOnlyPrice`는 같은 컬럼을 참조하지만, **읽기 전용**으로 설정되어 수정이 불가능함. 이렇게 해서 **다른 용도**로 같은 데이터를 사용할 수 있음.

## **상속 구조에서 이중 매핑**

- 상속 관계에서 **부모 엔티티의 속성**을 서로 다른 방식으로 매핑해야 할 때 이중 매핑을 사용할 수 있음.
- 예를 들어, 부모 엔티티의 속성을 자식 엔티티에서 **다르게 해석**하여 처리할 수 있음.
- **예시: 상속 구조에서 이중 매핑**

    ```java
    @Entity
    @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
    @DiscriminatorColumn(name = "dtype")
    public class Payment {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private BigDecimal amount;
    }
    
    @Entity
    @DiscriminatorValue("CASH")
    public class CashPayment extends Payment {
    
        @Formula("amount * 1.1")  // 세금 포함 계산
        private BigDecimal totalAmountWithTax;
    }
    
    @Entity
    @DiscriminatorValue("CARD")
    public class CardPayment extends Payment {
    
        @Formula("amount + 5")  // 수수료 포함 계산
        private BigDecimal totalAmountWithFee;
    }
    ```

- **설명**:
    - `Payment` 엔티티는 기본적인 `amount`를 저장하지만, 자식 엔티티인 `CashPayment`와 `CardPayment`는 각각 **세금**이나 **수수료**를 적용하여 **다르게 해석**함.
    - 같은 `amount` 필드를 각기 다른 방식으로 처리할 수 있게 설정함.

---

# **주의할 점**

## **동일한 컬럼을 이중으로 매핑할 때는 일관성에 주의**:

- 컬럼이 여러 방식으로 매핑될 때, **두 방식이 서로 충돌하지 않도록** 주의해야 함. 예를 들어, **insertable**과 **updatable** 속성을 통해 명확하게 관리하는 것이 중요함.

## **성능 이슈**:

- 이중 매핑은 **복잡도를 증가시킬 수 있으며**, 특히 `@Formula`를 사용할 때 성능에 영향을 줄 수 있음. 필요에 따라 **적절한 인덱스**와 **최적화**를 고려해야 함.

## **데이터 일관성 문제**:

- 동일한 데이터를 여러 방식으로 접근하다 보면 **동기화 문제**나 **일관성 문제**가 발생할 수 있음. 따라서, 데이터를 업데이트할 때 **어떤 매핑을 사용할지 명확히 관리**해야 함.