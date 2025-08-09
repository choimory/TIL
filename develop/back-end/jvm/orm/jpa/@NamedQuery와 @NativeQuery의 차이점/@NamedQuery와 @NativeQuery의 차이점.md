# @NamedQuery와 @NativeQuery의 차이점

- `@NamedQuery`와 `@NativeQuery`는 JPA에서 **쿼리를 정의하고 실행하는 방식**에 차이가 있는 어노테이션으로, **쿼리 작성 방법**과 **실행 방식**에 차이가 있음.
- 복잡한 SQL을 사용해야 할 때는 `@NativeQuery`를, 객체 지향적이고 데이터베이스 독립적인 쿼리가 필요할 때는 `@NamedQuery`를 사용

## `@NamedQuery`

- **JPQL**을 사용하여 **객체 지향적 쿼리**를 미리 정의해 재사용할 수 있으며, 데이터베이스에 독립적인 쿼리 작성 방식임. 테이블과 컬럼이 아닌 **엔티티와 필드**를 기준으로 쿼리를 작성하므로 JPA의 객체 모델과 잘 맞음.

## `@NativeQuery`

- **순수 SQL**을 사용하여 **데이터베이스에 직접적인 쿼리**를 실행할 수 있음. 특정 데이터베이스의 **고유 기능**을 활용할 수 있지만, 그만큼 **데이터베이스 종속성**이 생길 수 있음.

---

# @NamedQuery

- **JPQL**(Java Persistence Query Language)을 사용하여 **엔티티 클래스에 미리 정의된 쿼리**. `@NamedQuery`는 **객체 지향 쿼리**를 실행할 때 사용하며, **엔티티 객체와 그 속성**을 기준으로 쿼리가 작성됨.

## **특징**:

- **JPQL**을 사용하여 **객체 기반** 쿼리를 작성.
- **엔티티와 필드**를 기준으로 쿼리를 작성하며, 데이터베이스 테이블 및 컬럼이 아닌 **클래스와 필드 이름**을 사용.
- 데이터베이스 독립적이며, 다른 데이터베이스 시스템에서도 호환됨.
- 쿼리를 **미리 정의**하고 재사용할 수 있음.
- **JPQL은 SQL과 달리** 관계형 테이블이 아닌 **객체**와 그 **속성**을 기준으로 동작함.

## **장점**:

- **데이터베이스 독립성**:
    - JPQL은 데이터베이스에 종속되지 않으므로 다양한 DB에서 호환 가능.
- **미리 정의된 쿼리**:
    - `@NamedQuery`로 미리 정의해 두면 여러 곳에서 재사용 가능.
- **객체 지향 쿼리**:
    - 테이블 대신 **엔티티 객체와 그 필드**를 사용하여 쿼리를 작성하므로, 객체 지향적이고 JPA 모델과 일관성이 있음.

## 코드

```java
@Entity
@NamedQuery(
    name = "User.findByName",  // 쿼리 이름
    query = "SELECT u FROM User u WHERE u.name = :name"  // JPQL 쿼리
)
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

- `User` 엔티티에 대해 **미리 정의된 JPQL 쿼리**. 이름이 "findByName"인 쿼리가 `User` 엔티티에서 이름을 기준으로 데이터를 조회함.

```java
List<User> users = em.createNamedQuery("User.findByName", User.class)
                     .setParameter("name", "John")
                     .getResultList();
```

- **쿼리 실행**

---

# @NativeQuery

- **네이티브 SQL**(Raw SQL)을 사용하는 쿼리로, 데이터베이스의 **고유한 SQL 문법**을 직접 사용할 수 있음. **SQL을 그대로 작성**하여 실행하는 방식이며, JPQL처럼 객체 모델에 의존하지 않고 **데이터베이스 테이블 및 컬럼**을 기준으로 쿼리를 작성함.

## **특징**:

- **순수 SQL**을 사용하여 쿼리를 작성.
- 특정 데이터베이스의 **고유 기능**을 활용할 수 있음 (예: Oracle의 `ROWNUM`, MySQL의 `LIMIT` 등).
- **데이터베이스에 종속적**이므로, 데이터베이스에 따라 쿼리가 다르게 동작할 수 있음.
- SQL 쿼리이므로 **테이블과 컬럼 이름**을 사용.
- 복잡한 SQL 문법이나 데이터베이스 고유의 기능을 사용할 때 유용.

## **장점**:

- **복잡한 SQL 지원**:
    - JPQL로 작성하기 어려운 복잡한 쿼리나 특정 데이터베이스 기능을 사용할 수 있음.
- **성능 최적화**:
    - 데이터베이스 고유의 기능을 활용하여 성능 최적화 가능.
- **유연성**:
    - 특정 DBMS에 맞춰 SQL을 최적화할 수 있음.

## **예시**

```java
String sql = "SELECT * FROM users WHERE name = ?";
List<User> users = em.createNativeQuery(sql, User.class)
                     .setParameter(1, "John")
                     .getResultList();
```

- `users` 테이블에서 **순수 SQL**을 사용해 이름이 "John"인 사용자를 조회하는 쿼리.
- 이 예시는 **JPA 엔티티와 SQL을 함께 사용**하여 특정 테이블에서 직접 데이터를 조회함.

---

# 비교

| **특징** | **`@NamedQuery`** | **`@NativeQuery`** |
| --- | --- | --- |
| **쿼리 언어** | **JPQL** (객체 지향 쿼리 언어) | **SQL** (순수 SQL) |
| **기반** | **엔티티 객체와 필드**를 기반으로 쿼리 작성 | **테이블과 컬럼**을 기반으로 쿼리 작성 |
| **데이터베이스 의존성** | 데이터베이스 독립적 (여러 DB에서 호환 가능) | 데이터베이스 종속적 (DBMS 고유 기능 사용 가능) |
| **사용 예** | 간단한 조회 및 엔티티 간 관계를 기반으로 한 쿼리 작성 | 복잡한 쿼리나 성능 최적화를 위해 DB 고유 기능을 사용할 때 |
| **호환성** | 여러 DBMS와 호환 가능 | 특정 DBMS에 종속적일 수 있음 |
| **사용 용도** | 객체 지향적 쿼리 작성 시 사용 | 복잡한 SQL, DB 고유의 기능을 사용할 때 유용 |