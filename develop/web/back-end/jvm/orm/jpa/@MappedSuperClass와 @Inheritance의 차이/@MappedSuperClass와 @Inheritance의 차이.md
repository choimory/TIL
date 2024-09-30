# @MappedSuperClass와 @Inheritance의 차이

- `@MappedSuperclass`와 `@Inheritance`는 JPA에서 **상속 관계를 처리하는 방식**을 설정할 때 사용하는 어노테이션이지만, 각기 다른 용도로 사용됨.
- 두 어노테이션은 **부모 클래스와 자식 클래스** 간의 관계를 정의하지만, **데이터베이스 테이블에 어떻게 매핑될지**에 따라 차이가 있음.
- @MappedSuperclass
    - `@MappedSuperclass`는 **데이터베이스에 직접 영향을 미치지 않음**. 부모 클래스는 엔티티로 취급되지 않고, **테이블로 생성되지 않음**. 대신, 부모 클래스의 **필드만 자식 클래스에 상속**되어 **자식 클래스의 테이블에 통합**됨.
    - 상속 구조가 **객체 모델에서만** 존재하고, 데이터베이스에는 **직접적인 상속 관계가 반영되지 않음**.
    - **공통 필드를 재사용**하거나 여러 엔티티에서 중복 없이 관리할 수 있을 때 유용.
    - 예시

        ```java
        @MappedSuperclass
        public class BaseEntity {
        
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
        
            private LocalDateTime createdAt;
            private LocalDateTime updatedAt;
        
            // Getter/Setter
        }
        
        @Entity
        public class User extends BaseEntity {
        
            private String name;
            private String email;
        
            // Getter/Setter
        }
        
        ```

        - `BaseEntity` 클래스는 **테이블로 매핑되지 않으며**, `User` 엔티티가 `BaseEntity`의 필드를 상속받아 **`users` 테이블에 직접 매핑**.
        - 이 방식은 공통 필드들을 여러 엔티티에서 **중복 없이 관리**할 수 있도록 도와줌.
- @Inheritance
    - `@Inheritance`는 **상속 관계를 데이터베이스에 반영**하여, 부모 클래스와 자식 클래스 모두 **엔티티로 관리**되고 **테이블로 생성**됨.
    - 상속 전략에 따라 **단일 테이블**, **조인 테이블**, **테이블 퍼 클래스** 전략을 선택할 수 있음.
        - **단일 테이블 전략(SINGLE_TABLE)**:
            - 모든 엔티티가 **하나의 테이블**에 저장되고, **구분 컬럼**을 사용해 엔티티를 구분.
        - **조인 전략(JOINED)**:
            - 부모와 자식 엔티티가 **각각의 테이블**로 분리되고, **조인**을 통해 관계를 나타냄.
        - **테이블 퍼 클래스(TABLE_PER_CLASS)**:
            - 각 엔티티마다 **별도의 테이블**이 생성되며, 중복된 데이터가 발생할 수 있음.
    - 예시(단일 테이블)

        ```java
        @Entity
        @Inheritance(strategy = InheritanceType.SINGLE_TABLE)
        @DiscriminatorColumn(name = "dtype")  // 구분 컬럼
        public class Product {
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
        
            private String name;
        }
        
        @Entity
        public class Book extends Product {
            private String author;
        }
        
        @Entity
        public class Movie extends Product {
            private String director;
        }
        
        ```

        - **단일 테이블 전략**에서는 `Product`, `Book`, `Movie`가 모두 **하나의 테이블**에 저장되며, **구분 컬럼**(`dtype`)을 통해 각 행이 어떤 엔티티에서 온 것인지 구분.
        - `@Inheritance`와 `@DiscriminatorColumn`을 통해 자식 엔티티의 구분이 가능함.
- 비교

    | **특징** | **`@MappedSuperclass`** | **`@Inheritance`** |
    | --- | --- | --- |
    | **부모 클래스의 테이블 존재 여부** | 부모 클래스는 테이블로 매핑되지 않음 | 부모 클래스도 엔티티로 매핑되며, 테이블로 생성됨 |
    | **상속 구조의 데이터베이스 반영 여부** | 상속 구조가 데이터베이스에 반영되지 않음 | 상속 구조가 데이터베이스에 반영됨 |
    | **목적** | 여러 엔티티에서 **공통 속성**을 재사용하기 위해 사용 | **엔티티 간의 상속 관계**를 데이터베이스에서 표현하기 위해 사용 |
    | **사용 시기** | 공통 필드나 로직을 여러 엔티티에서 공유할 때 | 엔티티 상속 구조를 데이터베이스 테이블로 반영할 때 |
    | **상속 전략 지원 여부** | 상속 전략 지원 없음 | `SINGLE_TABLE`, `JOINED`, `TABLE_PER_CLASS` 상속 전략 지원 |
