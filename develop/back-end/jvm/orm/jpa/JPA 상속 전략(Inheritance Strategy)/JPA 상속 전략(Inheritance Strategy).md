# JPA 상속 전략(Inheritance Strategy)

- JPA에서 상속 전략(Inheritance Strategy)은 **엔티티 간의 상속 관계**를 데이터베이스에 어떻게 매핑할지 결정하는 방법
- PA는 객체 지향 프로그래밍의 **상속 구조**를 데이터베이스 테이블로 표현하는 다양한 전략을 제공
- 이 전략을 통해 상속 관계에 있는 엔티티들이 데이터베이스에서 어떻게 관리될지 결정할 수 있음.

---

# **단일 테이블 전략 (Single Table)**

- **모든 상속 관계의 엔티티를 하나의 테이블에 저장**하는 방식.
- 부모 클래스와 자식 클래스의 모든 속성이 **하나의 테이블에 모두 포함**됨. 자식 클래스의 구분을 위해 차별화 컬럼(Discriminator Column)을 사용하여 저장된 행이 어떤 자식 클래스에 해당하는지 구분함.

### **장점**:

- **성능이 좋음**:
    - 데이터가 **한 테이블에 모두 저장**되기 때문에 조회 시 **조인 없이 빠르게** 데이터를 가져올 수 있음.
- **단순한 구조**:
    - 테이블 구조가 단순하여 관리가 쉬움.

### **단점**:

- **테이블이 비대해질 수 있음**:
    - 상속 계층이 복잡할수록 테이블의 컬럼이 많아지고, **불필요한 컬럼**이 많아질 수 있음.
    - 예를 들어, 상속 관계에서 특정 자식 엔티티에서만 사용하는 컬럼도 테이블에 존재하게 됨.
- **데이터 무결성 문제**:
    - 한 테이블에 많은 컬럼이 포함되기 때문에 **NULL 값**이 많이 생길 수 있음.

### **예시**

```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "dtype")
public abstract class Vehicle {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}

@Entity
@DiscriminatorValue("Car")
public class Car extends Vehicle {
    private int numberOfDoors;
}

@Entity
@DiscriminatorValue("Bike")
public class Bike extends Vehicle {
    private boolean hasCarrier;
}
```

- **생성되는 테이블**

```sql
CREATE TABLE vehicle (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    dtype VARCHAR(255),
    name VARCHAR(255),
    numberOfDoors INT,     -- Car에서만 사용
    hasCarrier BOOLEAN     -- Bike에서만 사용
);
```

- **dtype** 컬럼으로 각 행이 `Car`인지 `Bike`인지 구분됨.
- **Car**와 **Bike**의 속성 모두가 **vehicle 테이블**에 포함되어 있음.

# **조인드 테이블 전략 (Joined Table)**

- **부모 클래스의 속성을 저장하는 테이블**과 **각 자식 클래스의 속성을 저장하는 테이블**을 따로 만들어서 **조인**을 통해 상속 구조를 구현
- 부모 클래스에 대한 정보는 **부모 테이블**에 저장하고, 자식 클래스에 대한 정보는 **자식 테이블**에 저장함.
- 조회 시 **부모 테이블과 자식 테이블을 조인**해서 데이터를 가져옴.

## **장점**:

- **정규화된 구조**:
    - 각 클래스에 맞는 **테이블이 분리**되어 있으므로 **중복 데이터**가 없고, 테이블이 깔끔하게 유지됨.
- **무결성**:
    - 자식 클래스에만 해당하는 컬럼은 자식 테이블에만 존재하므로, NULL 값이 생길 가능성이 적음.

## **단점**:

- **조회 성능이 저하될 수 있음**:
    - 데이터를 조회할 때 **조인이 발생**하므로, 복잡한 상속 관계에서는 성능이 저하될 수 있음.
- **구현 복잡성**:
    - 테이블이 여러 개로 나뉘어 있어 관리가 복잡해질 수 있음.

## **예시**

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
public abstract class Vehicle {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}

@Entity
public class Car extends Vehicle {
    private int numberOfDoors;
}

@Entity
public class Bike extends Vehicle {
    private boolean hasCarrier;
}
```

- **생성되는 테이블**

```sql
CREATE TABLE vehicle (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255)
);

CREATE TABLE car (
    id BIGINT PRIMARY KEY,
    numberOfDoors INT,
    FOREIGN KEY (id) REFERENCES vehicle(id)
);

CREATE TABLE bike (
    id BIGINT PRIMARY KEY,
    hasCarrier BOOLEAN,
    FOREIGN KEY (id) REFERENCES vehicle(id)
);
```

- **vehicle 테이블**에는 부모 클래스인 `Vehicle`의 속성만 저장됨.
- 각 자식 클래스(`Car`, `Bike`)는 **자기 테이블**에 추가 속성을 저장하며, **vehicle 테이블과 조인**하여 상속 관계를 유지함.

# **구체 클래스별 테이블 전략 (Table per Class)**

- 상속 구조에 있는 각 클래스가 **자신만의 테이블**을 가짐.
- 즉, **부모 클래스의 테이블이 존재하지 않음**. 각 자식 클래스가 부모 클래스의 속성을 포함한 **모든 속성을 자신의 테이블에 저장**함.
- 상속 관계에 있는 클래스들은 **독립적인 테이블**을 가지지만, 객체 지향적으로는 상속 관계를 유지함.
- **장점**:
    - **조인 없이 조회**:
        - 각 클래스가 독립적인 테이블을 가지므로 조회 시 **조인 없이 빠르게** 데이터를 가져올 수 있음.
    - **테이블 간 참조가 없음**:
        - 부모 테이블과의 조인 관계가 없기 때문에 **조인 성능 문제**가 발생하지 않음.
- **단점**:
    - **데이터 중복**:
        - 부모 클래스의 속성들이 각 자식 클래스의 테이블에 **중복**되어 저장됨. 동일한 속성이 여러 테이블에 걸쳐 저장되므로 **데이터 정규화**가 깨짐.
    - **테이블이 많아짐**:
        - 상속 구조에 있는 클래스마다 별도의 테이블이 생성되므로, 관리가 복잡해질 수 있음.
- **예시**

    ```java
    @Entity
    @Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
    public abstract class Vehicle {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        private String name;
    }
    
    @Entity
    public class Car extends Vehicle {
        private int numberOfDoors;
    }
    
    @Entity
    public class Bike extends Vehicle {
        private boolean hasCarrier;
    }
    ```

    - **생성되는 테이블**

    ```sql
    CREATE TABLE car (
        id BIGINT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255),
        numberOfDoors INT
    );
    
    CREATE TABLE bike (
        id BIGINT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(255),
        hasCarrier BOOLEAN
    );
    ```

    - **Car**와 **Bike**가 각각 **독립적인 테이블**을 가지고 있으며, 부모 클래스의 `name` 필드를 **각각의 테이블에 중복** 저장함.
    - **Vehicle** 테이블은 존재하지 않음.

# **정리**

1. **단일 테이블 전략 (Single Table)**:
    - 장점: 성능이 좋고 테이블 구조가 단순함.
    - 단점: 테이블에 불필요한 컬럼이 많아지고 NULL 값이 발생할 가능성이 있음.
2. **조인드 테이블 전략 (Joined Table)**:
    - 장점: 테이블이 정규화되어 관리가 용이하고 데이터 무결성을 유지함.
    - 단점: 조회 성능이 저하될 수 있으며 조인이 복잡할 수 있음.
3. **구체 클래스별 테이블 전략 (Table per Class)**:
    - 장점: 조인이 필요 없어 조회 성능이 우수함.
    - 단점: 데이터 중복이 발생하고 테이블 관리가 복잡해질 수 있음.