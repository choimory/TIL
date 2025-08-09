# @EntityGraph란 무엇이며 언제 어떻게 사용하는가

- `@EntityGraph`는 JPA에서 **N+1 문제**를 해결하거나, **연관된 엔티티를 효율적으로 가져오기 위한 방법**으로 사용되는 어노테이션.
- JPA에서 기본적으로 제공하는 페치 전략(FetchType.LAZY)를 사용하면 **지연 로딩**으로 인해 **추가 쿼리**가 발생할 수 있는데, 이를 제어하거나 즉시 로딩(FetchType.EAGER)을 대체하여 **지연 로딩이 필요한 상황에서 특정 연관 엔티티만 즉시 로딩**하도록 할 때 사용함.

---

# **`@EntityGraph`의 개념**

- `@EntityGraph`는 특정 엔티티를 조회할 때, 그 엔티티에 **연관된 엔티티를 어떤 방식으로 로딩할지**를 명시적으로 지정하는 어노테이션.
- 기본적인 페치 전략 대신 **특정 연관 필드만 즉시 로딩**하거나 **여러 필드를 동시에 로딩**할 수 있음.
- JPA가 엔티티를 조회할 때, 기본적으로 설정된 지연 로딩 전략을 **일부만 즉시 로딩**으로 변경하거나, 필요한 연관 엔티티를 **한 번에 로딩**하여 **N+1 문제**를 해결하고 **성능을 최적화**하는 데 사용.

---

# **`@EntityGraph`의 사용 시점**

## **N+1 문제 해결**:

- **N+1 문제**는 **지연 로딩(LAZY)** 방식에서 발생하며, 부모 엔티티를 조회한 후에 **연관된 자식 엔티티를 각각 조회할 때 발생하는 다수의 추가 쿼리 문제**를 말함.
- `@EntityGraph`를 사용하면 **한 번의 쿼리로 연관된 엔티티를 모두 로딩**할 수 있어, 이러한 성능 문제를 해결할 수 있음.

## **즉시 로딩(EAGER) 대체**:

- 즉시 로딩(FetchType.EAGER)는 모든 연관 엔티티를 무조건 로딩하는 방식이지만, `@EntityGraph`를 사용하면 필요한 **일부 연관 엔티티**만 **즉시 로딩**할 수 있음.
- 이를 통해 성능을 더 세밀하게 제어 가능.

## **JPQL 쿼리나 리포지토리 메서드와 함께 사용**:

- JPQL 쿼리 또는 Spring Data JPA 리포지토리 메서드와 함께 사용하여 **동적으로 연관 엔티티를 로딩하는 전략**을 적용할 수 있음.

---

# **`@EntityGraph`의 장점**

## **N+1 문제 해결**:

- `@EntityGraph`는 **N+1 문제**를 해결하는 데 매우 유용.
- 여러 번의 추가 쿼리가 발생하는 문제를 **한 번의 쿼리**로 해결 가능.

## **성능 최적화**:

- 필요한 **일부 연관 엔티티**만 **즉시 로딩**하여, 성능을 효율적으로 최적화할 수 있음.
- `EAGER`는 모든 연관 엔티티를 로딩하지만, `@EntityGraph`는 **필요한 부분만 로딩**함.

## **동적 로딩**:

- **JPQL** 쿼리와 함께 사용하면, 특정 조건에 맞는 엔티티만 **동적으로 즉시 로딩**할 수 있어, **불필요한 로딩을 줄일 수 있음**.

---

# 정리

- `@EntityGraph`는 JPA에서 **연관 엔티티를 효율적으로 로딩**하고, **N+1 문제를 해결**하기 위한 도구.
- 기본적으로 **지연 로딩(LAZY)** 전략을 사용하는 경우에도 **특정 연관 필드만 즉시 로딩**할 수 있음.
- **JPQL 쿼리**와 함께 사용하면 **복잡한 조회 쿼리**에서 동적으로 **성능 최적화** 가능.
- **N+1 문제 해결**과 **성능 최적화**를 위해 연관 엔티티를 **필요한 시점에 한 번에 로딩**할 때 유용하게 사용됨.

# 사용법

### 1. **간단한 사용 예시**

```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Order> orders;  // 연관된 주문 정보 (지연 로딩)

    // getters and setters
}

@Entity
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String product;

    @ManyToOne(fetch = FetchType.LAZY)
    private User user;

    // getters and setters
}

```

- **설명**: `User` 엔티티와 연관된 `Order` 엔티티가 **지연 로딩(FetchType.LAZY)**로 설정되어 있음. `@EntityGraph`를 사용하지 않으면 `User`를 조회할 때 **지연 로딩으로 인해 추가 쿼리**가 발생함.

### 2. **리포지토리에서 `@EntityGraph` 사용**

```java
public interface UserRepository extends JpaRepository<User, Long> {

    @EntityGraph(attributePaths = {"orders"})  // orders 필드를 즉시 로딩
    List<User> findAll();
}

```

- **설명**: `@EntityGraph(attributePaths = {"orders"})`를 사용하여 **User와 연관된 주문(Order) 엔티티를 즉시 로딩**. 즉, `findAll()` 메서드를 호출할 때 **N+1 문제 없이** 한 번의 쿼리로 `User`와 `Order`를 모두 가져옴.

### **쿼리 실행**

```java
List<User> users = userRepository.findAll();

```

- **결과**: `@EntityGraph`를 적용했기 때문에, `User`와 **연관된 `Order` 엔티티가 한 번에 로딩**되어 **N+1 문제를 해결**함.

---

### **`@EntityGraph` 속성**

1. **`attributePaths`**:
    - 즉시 로딩할 **연관 엔티티의 속성(필드) 목록**을 지정. **배열 형태**로 여러 연관 엔티티를 동시에 로딩 가능.
    - 예시:

        ```java
        @EntityGraph(attributePaths = {"orders", "address"})
        
        ```

2. **`type`** (선택 사항):
    - `EntityGraph.EntityGraphType.FETCH`: 지정한 연관 엔티티만 **즉시 로딩**.
    - `EntityGraph.EntityGraphType.LOAD`: 기본 **페치 전략을 무시**하고 지정된 연관 엔티티를 로딩.

---

### **복잡한 사용 예시 (다중 연관 엔티티 로딩)**

```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Order> orders;

    @OneToOne(fetch = FetchType.LAZY)
    private Address address;
}

@Entity
public class Address {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String city;
}

@Entity
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String product;

    @ManyToOne(fetch = FetchType.LAZY)
    private User user;
}

public interface UserRepository extends JpaRepository<User, Long> {

    @EntityGraph(attributePaths = {"orders", "address"})  // 여러 연관 엔티티를 즉시 로딩
    List<User> findAllWithOrdersAndAddress();
}

```

- **설명**: `orders`와 `address`라는 **두 개의 연관 필드**를 **동시에 즉시 로딩**하여, 한 번의 쿼리로 `User` 엔티티와 관련된 주문 및 주소 정보를 모두 가져옴.

---

### **JPQL과 함께 사용**

`@EntityGraph`는 **JPQL** 쿼리와도 함께 사용할 수 있음. 이렇게 하면 **동적으로 필요한 연관 엔티티만 즉시 로딩**하도록 쿼리를 최적화할 수 있음.

```java
@Query("SELECT u FROM User u WHERE u.name = :name")
@EntityGraph(attributePaths = {"orders"})  // orders 필드를 즉시 로딩
User findByNameWithOrders(@Param("name") String name);

```

- **설명**: JPQL 쿼리와 함께 `@EntityGraph`를 사용하여 **특정 이름을 가진 사용자를 조회할 때 그와 관련된 주문 정보도 즉시 로딩**.