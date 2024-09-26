# JPA의 CascadeType에 대해

- **엔티티 간의 연관 관계에서 특정 작업을 전파**하는 데 사용되는 옵션으로, 부모 엔티티에 대한 작업이 자식 엔티티에도 영향을 미치도록 설정하는 기능.
- 즉, 부모 엔티티가 수행하는 작업(저장, 삭제 등)이 자식 엔티티에도 **자동으로 적용**되도록 도와줌.
- `PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, `DETACH`, `ALL` 등의 옵션을 통해 특정 작업이 자식 엔티티에 어떻게 적용될지 설정 가능.
- 잘못된 사용은 데이터 손실을 초래할 수 있으므로, 전파할 작업의 범위를 신중하게 선택해야 함.
- PERSIST
    - 부모 엔티티가 **저장**(persist)될 때, 연관된 자식 엔티티도 자동으로 **저장**됨.
    - **저장 전파**: 부모 엔티티를 저장하면 자식 엔티티도 함께 저장됨.

    ```java
    @OneToMany(cascade = CascadeType.PERSIST)
    private List<OrderItem> orderItems;
    ```

  예시에서 부모 엔티티(`Order`)를 저장할 때 자식 엔티티(`OrderItem`)도 자동으로 저장됨.

- MERGE
    - 부모 엔티티가 **병합**(merge)될 때, 연관된 자식 엔티티도 자동으로 **병합**됨.
    - **병합 전파**: 부모 엔티티의 병합 작업이 자식 엔티티까지 전파되어, 자식 엔티티의 상태도 병합됨.

    ```java
    @OneToMany(cascade = CascadeType.MERGE)
    private List<OrderItem> orderItems;
    ```

  부모 엔티티(`Order`)를 병합할 때 자식 엔티티(`OrderItem`)도 병합되어, 데이터베이스에 갱신된 값이 반영됨.

- REMOVE
    - 부모 엔티티가 **삭제**될 때, 연관된 자식 엔티티도 자동으로 **삭제**됨.
    - **삭제 전파**: 부모 엔티티를 삭제하면 자식 엔티티도 함께 삭제됨.

    ```java
    @OneToMany(cascade = CascadeType.REMOVE)
    private List<OrderItem> orderItems;
    ```

  부모 엔티티(`Order`)를 삭제하면, 연관된 자식 엔티티(`OrderItem`)도 자동으로 데이터베이스에서 삭제됨.

- REFRESH
    - 부모 엔티티가 **새로고침**(refresh)될 때, 연관된 자식 엔티티도 자동으로 **새로고침**됨.
    - **새로고침 전파**: 부모 엔티티가 데이터베이스에서 다시 조회되어 상태가 새로고침될 때, 자식 엔티티도 새로고침됨.

    ```java
    @OneToMany(cascade = CascadeType.REFRESH)
    private List<OrderItem> orderItems;
    ```

  부모 엔티티가 새로고침되면 자식 엔티티도 데이터베이스에서 다시 조회되어 새로운 상태로 변경됨.

- DETACH
    - 부모 엔티티가 **준영속**(detached) 상태가 될 때, 연관된 자식 엔티티도 자동으로 **준영속 상태**로 전환됨.
    - **준영속 전파**: 부모 엔티티가 영속성 컨텍스트에서 분리되면 자식 엔티티도 영속성 컨텍스트에서 분리됨.

    ```java
    @OneToMany(cascade = CascadeType.DETACH)
    private List<OrderItem> orderItems;
    ```

  부모 엔티티가 영속성 컨텍스트에서 분리되면, 자식 엔티티도 영속성 컨텍스트에서 분리되어 준영속 상태로 전환됨.

- ALL
    - **모든 작업**(PERSIST, MERGE, REMOVE, REFRESH, DETACH)이 전파됨.
    - 부모 엔티티에 대한 모든 작업이 자식 엔티티에도 동일하게 적용됨.

    ```java
    @OneToMany(cascade = CascadeType.ALL)
    private List<OrderItem> orderItems;
    ```

  부모 엔티티에 대한 저장, 병합, 삭제, 새로고침, 준영속 상태 전환 등이 자식 엔티티에도 동일하게 전파됨.