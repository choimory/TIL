- `EntityManager`는 JPA(Java Persistence API)에서 **엔티티를 관리하고 데이터베이스와 상호작용하는 핵심 인터페이스**.
- 엔티티의 생명주기(생성, 읽기, 수정, 삭제)를 관리하며, 영속성 컨텍스트(Persistence Context라는 메커니즘을 통해 **엔티티 객체와 데이터베이스의 데이터를 동기화**하는 역할을 함.
- **`EntityManager`의 주요 역할**
    - **영속성 컨텍스트 관리**
        - `EntityManager`는 **영속성 컨텍스트**를 통해 엔티티를 관리. **영속성 컨텍스트**는 JPA에서 엔티티를 **데이터베이스와 연결된 상태로 관리하는 메모리 공간**으로, 데이터베이스와의 상호작용을 통해 **엔티티 객체와 데이터베이스 간의 일관성을 유지**함.
        - **기능**:
            - 엔티티를 영속성 컨텍스트에 등록하거나 제거.
            - **1차 캐시**를 통해 **중복된 데이터베이스 조회를 방지**하고 성능을 최적화.

        ```java
        EntityManager em = entityManagerFactory.createEntityManager();
        em.getTransaction().begin();
        
        User user = em.find(User.class, 1L);  // 영속성 컨텍스트에서 조회
        user.setName("John Doe");
        
        em.getTransaction().commit();
        em.close();
        ```

    - **엔티티 생명주기 관리**
        - `EntityManager`는 엔티티 객체의 **생명주기 상태**(Transient, Persistent, Detached, Removed)를 관리함. 엔티티가 생성되거나 삭제되면, 해당 엔티티의 상태가 `EntityManager`에 의해 조정됨.
        - **생명주기**:
            - **Transient**: 아직 영속성 컨텍스트에 등록되지 않은 상태.
            - **Persistent**: 영속성 컨텍스트에서 관리되는 상태.
            - **Detached**: 영속성 컨텍스트에서 분리된 상태.
            - **Removed**: 삭제된 상태.

        ```java
        User user = new User();  // Transient 상태
        user.setName("John");
        
        em.persist(user);  // Persistent 상태로 전환
        em.remove(user);   // Removed 상태로 전환
        ```

    - **트랜잭션 처리**
        - `EntityManager`는 데이터베이스와 상호작용할 때 **트랜잭션**을 관리. 트랜잭션은 **데이터의 일관성**을 유지하고 **안정성**을 보장하는 역할을 하며, `EntityManager`는 **트랜잭션의 시작과 종료**를 제어함.
        - **기능**:
            - **트랜잭션 시작**: `begin()` 메서드를 통해 트랜잭션을 시작.
            - **트랜잭션 커밋**: `commit()` 메서드를 통해 트랜잭션을 성공적으로 종료하고, 변경 사항을 데이터베이스에 반영.
            - **트랜잭션 롤백**: 트랜잭션 중 오류가 발생하면 `rollback()`을 호출하여 변경 사항을 취소.

        ```java
        em.getTransaction().begin();  // 트랜잭션 시작
        User user = new User("John", "john@example.com");
        
        em.persist(user);  // 영속성 컨텍스트에 저장
        em.getTransaction().commit();  // 트랜잭션 커밋, 데이터베이스에 반영
        ```

    - **쿼리 생성 및 실행**
        - `EntityManager`는 **JPQL**(Java Persistence Query Language) 또는 **네이티브 SQL**을 사용하여 데이터베이스 쿼리를 생성하고 실행할 수 있음. 이를 통해 **복잡한 데이터 조회** 작업을 수행 가능.
        - **기능**:
            - **JPQL 쿼리 실행**: 엔티티를 대상으로 하는 객체 지향 쿼리 생성.
            - **네이티브 SQL 실행**: 데이터베이스에 종속적인 SQL 쿼리 실행.
            - **Criteria API**: 타입 세이프하게 동적 쿼리를 작성할 수 있음.

        ```java
        // JPQL 쿼리
        List<User> users = em.createQuery("SELECT u FROM User u WHERE u.name = :name", User.class)
                             .setParameter("name", "John")
                             .getResultList();
        ```

    - **캐시 관리**
        - `EntityManager`는 **1차 캐시**(영속성 컨텍스트 내에서 관리되는 캐시)를 통해 **데이터베이스의 불필요한 중복 조회를 방지**함. 이미 영속성 컨텍스트에 있는 엔티티는 데이터베이스에 다시 쿼리를 날리지 않고, **캐시된 데이터를 반환**.
        - **기능**:
            - 엔티티가 **1차 캐시에 저장**되면, 해당 트랜잭션이 유지되는 동안 같은 엔티티에 대한 조회가 발생할 때 데이터베이스에 다시 접근하지 않고, 캐시에서 엔티티를 반환.
            - 트랜잭션 종료 시 1차 캐시는 비워짐.

        ```java
        User user1 = em.find(User.class, 1L);  // 첫 번째 조회 시 DB에서 가져옴
        User user2 = em.find(User.class, 1L);  // 두 번째 조회 시 1차 캐시에서 가져옴
        ```

- **`EntityManager`의 주요 메서드**
    - **`persist(entity)`**:
        - 비영속 상태의 엔티티를 **영속 상태**로 전환.
    - **`merge(entity)`**:
        - 준영속 상태의 엔티티를 다시 **영속 상태**로 전환.
    - **`remove(entity)`**:
        - 영속 상태의 엔티티를 **삭제 상태**로 전환.
    - **`find(entityClass, primaryKey)`**:
        - **데이터베이스**에서 엔티티를 조회하여 **영속성 컨텍스트**에 로드.
    - **`createQuery(jpql)`**:
        - JPQL 쿼리를 생성하고 실행.
    - **`flush()`**:
        - 영속성 컨텍스트에 있는 변경 사항을 **즉시 데이터베이스에 반영**.
- 정리
    - `EntityManager`는 JPA의 핵심 인터페이스로, **영속성 컨텍스트**를 관리하고 **데이터베이스와 상호작용**하는 역할을 함.
    - **영속성 컨텍스트** 내에서 엔티티 객체를 관리하며, **엔티티의 생명주기**(Transient, Persistent, Detached, Removed)를 제어.
    - **트랜잭션 관리**, **쿼리 실행**, **캐시 관리** 등 데이터베이스와의 상호작용을 처리하는 데 중요한 역할을 수행.