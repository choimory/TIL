# Dirty Checking이란

- **Dirty Checking**은 JPA에서 **영속성 컨텍스트(Persistence Context)** 내에 관리되는 **엔티티의 변경 사항을 자동으로 감지**하여, **데이터베이스에 반영**하는것
- 이 기능은 JPA가 제공하는 중요한 자동화 기능 중 하나로, **개발자가 명시적으로 업데이트 쿼리를 작성하지 않아도** JPA가 알아서 변경된 엔티티의 상태를 데이터베이스에 동기화함.

---

# **Dirty Checking의 동작 원리**

## **엔티티를 영속성 컨텍스트에 로드**:

- `EntityManager`가 데이터베이스에서 엔티티를 조회할 때, 해당 엔티티는 **영속성 컨텍스트**에 저장되고, JPA는 초기 상태(Snapshot)를 저장함.

## **엔티티 변경**:

- 애플리케이션이 엔티티의 속성을 변경하면, 이 변경 사항은 일단 메모리에서만 발생함.
- JPA는 이 시점에 데이터베이스를 즉시 업데이트하지 않음.

## **트랜잭션이 커밋되거나 `flush()`가 호출될 때**:

- JPA는 **트랜잭션 커밋 시점** 또는 **명시적으로 `flush()`를 호출할 때** 영속성 컨텍스트에 있는 엔티티의 현재 상태와 초기 상태(Snapshot)를 비교.
- **변경된 속성**이 있는 엔티티에 대해 **자동으로 UPDATE 쿼리**를 생성하고, 데이터베이스에 반영.

## **데이터베이스 반영**:

- 변경된 엔티티의 **변경 사항만 데이터베이스에 반영**됨.
- 이 과정에서 개발자는 별도의 `UPDATE` 쿼리를 작성하지 않아도 JPA가 알아서 처리.

---

# **Dirty Checking의 장점**

## **개발자의 수고를 덜어줌**:

- 엔티티의 변경 사항을 명시적으로 `UPDATE`하지 않아도, JPA가 자동으로 변경된 부분만을 감지하고 업데이트하므로 **코드가 간결**해짐.

## **성능 최적화**:

- 변경된 속성만을 추적하여 **필요한 부분만 UPDATE**하기 때문에 불필요한 전체 업데이트가 발생하지 않음.

## **트랜잭션 관리의 일관성**:

- **영속성 컨텍스트**가 자동으로 변경 사항을 관리하므로, 데이터베이스와 애플리케이션 간의 **일관성**을 유지할 수 있음.

---

# **Dirty Checking의 주의사항**

## **영속성 컨텍스트 내에서만 동작**:

- **Dirty Checking**은 **영속 상태의 엔티티**에서만 동작.
- 즉, 영속성 컨텍스트에 포함된 엔티티에 대해서만 자동으로 변경 사항을 감지할 수 있음.
- **준영속 상태**(Detached) 또는 **비영속 상태**(Transient)의 엔티티는 Dirty Checking 대상이 아님.

## **지연된 업데이트**:

- 엔티티의 변경 사항은 **즉시 데이터베이스에 반영되지 않고**, 트랜잭션이 커밋되거나 `flush()`가 호출될 때 반영됨.
- 따라서 트랜잭션 종료 전에 엔티티를 조회하면 변경 사항이 반영되지 않을 수 있음.

## **성능 문제**:

- 많은 엔티티를 한꺼번에 변경하거나, 복잡한 연관 관계를 가진 엔티티에서 변경이 발생하면 **Dirty Checking으로 인한 성능 저하**가 발생할 수 있음.
- **최적화**가 필요한 경우도 있음.

## **컬렉션 필드의 Dirty Checking**:

- 엔티티에 포함된 컬렉션 필드(List, Set 등)는 기본적으로 **Deep Copy** 방식으로 변경 사항을 감지하지 않음.
- 따라서 컬렉션 필드의 변경 사항을 **수동으로 관리**하거나, **Hibernate의 확장 기능**을 사용할 수 있음.

---

# **Dirty Checking 동작 예시**

- **엔티티 클래스**

    ```java
    @Entity
    public class User {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private String name;
    
        private String email;
    
        // Getters and Setters
    }
    ```

- **Dirty Checking 사용 예시**

    ```java
    EntityManager em = entityManagerFactory.createEntityManager();
    em.getTransaction().begin();
    
    // 영속성 컨텍스트에 엔티티 로드
    User user = em.find(User.class, 1L);
    
    // 엔티티의 속성 변경 (변경 감지)
    user.setName("Updated Name");  // Dirty Checking 대상
    
    // 트랜잭션 커밋 (변경 사항 자동 반영)
    em.getTransaction().commit();  // UPDATE 쿼리 자동 발생
    em.close();
    ```

- **설명**:
    - `user` 엔티티가 데이터베이스에서 조회되면서 영속성 컨텍스트에 로드됨.
    - 이후 `user.setName("Updated Name")`을 호출해 엔티티의 `name` 속성을 변경하지만, **즉시 데이터베이스에 반영되지 않음**.
    - 트랜잭션을 **커밋하거나 `flush()`를 호출**할 때 JPA는 엔티티의 **초기 상태**와 **변경된 상태**를 비교하여 **자동으로 `UPDATE` 쿼리**를 생성.
    - 이로 인해 **Dirty Checking**을 통해 데이터베이스가 **자동으로 업데이트**됨.

---

# **Dirty Checking을 수동으로 동작시키기**

- **`flush()` 메서드**: 트랜잭션 커밋 전에 변경 사항을 데이터베이스에 반영하려면 **수동으로 `flush()`** 메서드를 호출할 수 있음.
- **예시: `flush()` 메서드를 사용한 변경 사항 반영**

    ```java
    em.getTransaction().begin();
    
    User user = em.find(User.class, 1L);
    user.setName("Updated Name");
    
    // 변경 사항을 즉시 반영
    em.flush();  // UPDATE 쿼리가 즉시 발생
    
    em.getTransaction().commit();
    
    ```

- **설명**:
    - `flush()` 메서드는 영속성 컨텍스트의 변경 사항을 즉시 데이터베이스에 반영하지만, 트랜잭션은 여전히 유지됨.

---

# 정리

- **Dirty Checking**은 JPA에서 **영속 상태의 엔티티 변경 사항을 자동으로 감지**하여, 트랜잭션이 커밋될 때 **자동으로 데이터베이스에 반영**하는 기능.
- 이를 통해 **개발자가 명시적으로 `UPDATE` 쿼리를 작성할 필요가 없고**, JPA가 알아서 변경 사항을 감지하여 데이터베이스에 반영함.
- 성능 최적화와 일관성을 유지하는 데 유용하지만, 많은 엔티티를 한꺼번에 변경하거나 컬렉션 필드를 사용할 때는 성능 이슈에 주의해야 함.