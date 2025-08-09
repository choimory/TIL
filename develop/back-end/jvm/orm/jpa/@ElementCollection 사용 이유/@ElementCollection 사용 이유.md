# @ElementCollection 사용 이유

- JPA에서 **엔티티의 필드가 복합 타입**(예: 컬렉션, 값 타입)을 가질 때 이를 **별도의 테이블에 저장**하여 관리하기 위한 어노테이션
- **기본 값 타입이나 복합 값 타입의 컬렉션을 매핑**할 때 사용됨.
- 주로 **리스트, 세트** 같은 컬렉션을 엔티티 필드로 정의하고, 해당 값들을 **별도의 테이블에 저장**하고자 할 때 유용함.
- **`@ElementCollection` 사용 이유**
    - **기본 값 타입 컬렉션 관리**
        - JPA에서 **기본 값 타입**(예: `String`, `Integer`, `LocalDate` 등)의 **컬렉션**을 엔티티 필드로 저장할 때, 해당 컬렉션을 `@ElementCollection`을 통해 관리할 수 있음.
        - 이 컬렉션의 값들은 **기본 테이블이 아닌, 별도의 테이블에 저장**됨.

      ### 예시: 사용자 엔티티에서 여러 개의 주소를 관리하는 경우

        ```java
        @Entity
        public class User {
        
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
        
            private String name;
        
            @ElementCollection
            @CollectionTable(name = "user_addresses", joinColumns = @JoinColumn(name = "user_id"))
            @Column(name = "address")
            private List<String> addresses;  // 기본 값 타입 컬렉션 (String)
        }
        ```

        - **설명**:
            - `addresses` 필드는 **`List<String>`** 타입으로 여러 주소를 저장.
            - **`@ElementCollection`**: 이 필드가 별도의 테이블에 저장됨을 의미.
            - **`@CollectionTable`**: 값 타입 컬렉션을 저장할 테이블을 정의.
                - **name**: 컬렉션이 저장될 테이블 이름 (`user_addresses`).
                - **joinColumns**: 외래 키로 **`user_id`*를 사용하여 사용자와 연결.
        - **데이터베이스 구조**:
            - **`user`** 테이블: `id`, `name`을 저장.
            - **`user_addresses`** 테이블: 각 사용자의 여러 주소를 저장. 외래 키(`user_id`)를 통해 사용자와 연결.
    - **엔티티에 포함된 복합 값 타입 관리**
        - **값 타입**은 엔티티와 달리 식별자가 없으며, 여러 엔티티에서 재사용 가능한 **불변 객체**로 사용될 수 있음.
        - 복합 값 타입을 **엔티티 필드**로 가질 때, 이를 **컬렉션으로 저장**하고 관리하는 경우 `@ElementCollection`을 사용할 수 있음.

      ### 예시: 사용자 엔티티에서 복합 값 타입을 사용하는 경우

        ```java
        java
        코드 복사
        @Embeddable
        public class Address {
            private String street;
            private String city;
            private String zipcode;
        
            // 기본 생성자, getter/setter
        }
        
        @Entity
        public class User {
        
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
        
            private String name;
        
            @ElementCollection
            @CollectionTable(name = "user_addresses", joinColumns = @JoinColumn(name = "user_id"))
            private List<Address> addresses;  // 복합 값 타입 컬렉션
        }
        
        ```

        - **설명**:
            - *`Address`*는 **값 타입**으로 정의된 클래스(`@Embeddable`).
            - `User` 엔티티의 **`addresses`** 필드는 **`List<Address>`*로, 여러 개의 주소를 저장할 수 있음.
            - **`@ElementCollection`**: 복합 값 타입인 `Address`가 별도의 테이블에 저장됨.
            - **`@CollectionTable`**: **`user_addresses`** 테이블에 값을 저장하고, **외래 키로 `user_id`*를 통해 연결.
        - **데이터베이스 구조**:
            - **`user`** 테이블: 사용자 정보 저장 (`id`, `name`).
            - **`user_addresses`** 테이블: 사용자의 주소(`street`, `city`, `zipcode`)를 **`user_id`*로 연결하여 저장.
    - **별도의 테이블에 값 타입을 저장**
        - `@ElementCollection`은 **값 타입 컬렉션을 별도의 테이블에 저장**함으로써, 엔티티 테이블과는 분리하여 관리할 수 있음.
        - 즉, 값 타입 컬렉션은 **다른 테이블에 저장**되고, 해당 엔티티와 **외래 키**를 통해 연결됨. 이를 통해 **정규화된 데이터 구조**를 만들 수 있음.
        - **장점**:
            - **테이블을 정규화**하여 동일한 엔티티 필드에 여러 값을 저장할 때, 데이터 중복을 방지.
            - 엔티티와 값 타입 컬렉션을 **명확하게 분리**하여 관리할 수 있음.
    - **엔티티 간의 연관 관계 없이 값 타입을 관리**
        - **값 타입**은 **엔티티**와 달리 **식별자**가 없고, 엔티티 간의 **명시적인 연관 관계를 설정하지 않고도** 관리할 수 있음.
        - 따라서 **엔티티 간의 복잡한 관계**를 만들지 않고도, 엔티티의 상태와 무관하게 값 타입 컬렉션을 쉽게 관리할 수 있음.
- **`@ElementCollection`의 주요 특징**
    - **별도의 테이블**에 컬렉션을 저장:
        - 값 타입 컬렉션은 **기본 엔티티 테이블과 분리된 테이블**에 저장됨.
    - **값 타입**만 사용할 수 있음:
        - **엔티티가 아닌 값 타입**(기본 타입이나 `@Embeddable` 타입)만 **컬렉션으로 저장** 가능.
    - **값의 불변성**:
        - 값 타입은 **불변 객체**로 사용되므로, 컬렉션 내 값이 **변경**될 때는 **전체를 삭제하고 다시 삽입**하는 방식으로 동작.
- 주의사항
    - **전체 삭제 후 삽입**:
        - 값 타입 컬렉션은 변경될 때마다 **기존의 컬렉션을 삭제하고 새로운 값을 삽입**함. 따라서 컬렉션 내 값이 자주 변경되는 경우에는 **트랜잭션 비용**이 높을 수 있음.
    - **성능**:
        - 값 타입 컬렉션을 자주 사용하는 경우, **컬렉션 테이블**에 대한 **조인**이 빈번하게 발생하여 성능이 저하될 수 있음. 이 경우 **페치 전략**을 조정하거나, **필요한 경우에만 로드**하도록 설정할 필요가 있음.

        ```java
        @ElementCollection(fetch = FetchType.LAZY)
        private List<Address> addresses;  // 지연 로딩 설정
        ```

- 정리
    - `@ElementCollection`은 **기본 값 타입**이나 **복합 값 타입**의 컬렉션을 엔티티의 필드로 정의할 때 사용하며, 이를 **별도의 테이블에 저장**하여 관리함.
    - 주로 **엔티티 간의 연관 관계 없이** 값 타입 데이터를 관리하거나, **여러 개의 값**을 저장해야 할 때 사용.
    - **별도의 테이블**을 통해 값 타입 컬렉션을 관리하므로, **정규화된 데이터 구조**를 유지할 수 있음.