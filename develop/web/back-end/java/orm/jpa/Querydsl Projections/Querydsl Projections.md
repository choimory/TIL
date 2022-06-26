# 개요

- Querydsl Projections에 대해 알아본다

# Querydsl Projections을 권장한다

- 기본 엔티티를 넘기는것보다 Projections를 통해 매핑한 DTO 등의 객체를 넘기는것을 권장한다. 
  - 이는 서비스단에서 매핑 작업을 추가적으로 거치는것보다 성능상으로 이득이 있기 때문이다.

# Querydsl Projections의 종류

- Projections에는 constructor, fields, bean 세가지가 존재하며 각각의 유의사항이 있다.

## Projections.constructor

- 생성자를 통해 객체를 생성한다
- 주의사항: 생성자를 빌더패턴으로 적용하였다 하더라도 특정 필드만 초기화하는것이 불가능하다.
  - Projections.constructor()를 통해 특정 필드만 초기화하고 싶을시, 해당 필드들을 넘겨받는 생성자를 선언해야 한다.
    - 상황에 따라 동적으로 하고 싶을땐 모둔 생성자가 있어야 한다. 빌더로는 불가능.
  - 결국 Projections.constructor()를 통해 동적으로 원하는 필드만 주입하는것이 사실상 매우 번거롭기 때문에 전체 필드를 초기화하는 경우에만 사용한다.

```java
@Builder
@RequiredArgsConstructor
@Getter
public class MemberDto {
    private final int id;
    private final String name;
    private final int age;
    private final Double height;
    private final Double weight;
}

@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;
    
  public List<MemberDto> selectMembers() {
    return query.select(Projections.constructor(MemberDto.class,
                    member.id,
                    member.name,
                    member.age,
                    member.height,
                    member.weight))
            .from(member)
            .fetch();
  }
}
```

## Projections.fields

- 객체 생성 후, 초기화 된 필드에 2차적으로 값을 직접 주입한다
- 주의사항: 기본적으로 객체를 생성 한 뒤, 필드에 직접 값을 주입하기 때문에, 필드가 final 변수일시 값 주입이 불가능하다.
  - 때문에 fields()를 사용할 객체는 final 변수 + 필수 생성자를 사용할 수 없고, 일반 변수 + 기본 생성자 + 전체 생성자로 설계해야함

```java
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Getter
public class MemberDto {
    private int id;
    private String name;
    private int age;
    private Double height;
    private Double weight;
}

@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;
    
  public List<MemberDto> selectMembers() {
    return query.select(Projections.fields(MemberDto.class,
                    member.id,
                    member.name,
                    member.age,
                    member.height,
                    member.weight))
            .from(member)
            .fetch();
  }
}
```

## Projections.bean

- 객체 생성 후, 초기화 된 필드에 2차적으로 setter를 통해 값을 주입한다
- 주의사항: 필드가 final 변수일시 값 주입이 불가능하며, setter 메소드도 존재해야 한다.
  - 때문에 bean()을 사용할 객체는 final 변수 + 필수 생성자를 사용할 수 없고, 일반 변수 + 기본 생성자 + 전체 생성자에 setter도 마련하도록 설계해야함

```java
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Setter
@Getter
public class MemberDto {
    private int id;
    private String name;
    private int age;
    private Double height;
    private Double weight;
}

@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;
    
  public List<MemberDto> selectMembers() {
    return query.select(Projections.bean(MemberDto.class,
                    member.id,
                    member.name,
                    member.age,
                    member.height,
                    member.weight))
            .from(member)
            .fetch();
  }
}
```

## as()

- 매핑할 객체의 필드와 엔티티의 필드의 이름이 동일해야 하며, 다를시 .as()를 통해 매핑할 객체의 필드에 맞춰주면 된다.

```java
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Getter
public class MemberDto {
    private int id;
    private String memberName;
    private int memberAge;
    private Double memberHeight;
    private Double memberWeight;
}

@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;
    
  public List<MemberDto> selectMembers() {
    return query.select(Projections.fields(MemberDto.class,
                    member.id,
                    member.name.as("memberName"),
                    member.age.as("memberAge"),
                    member.height.as("memberHeight"),
                    member.weight.as("memberWeight")))
            .from(member)
            .fetch();
  }
}
```

## @QueryProjection 객체 만들기

- 생성자에 @QueryProjection을 부여해 DTO QClass를 생성하여, Projections를 대신할 수도 있다.
- final 변수를 동적으로 생성할 수 있으므로 constructor()와 fields()의 장점을 모두 취한다고 할 수 있다.
- 하지만 Querydsl에 의존성이 생기는 객체가 되므로 유의한다.

```java
@Builder
@Getter
public class MemberDto {
    private final int id;
    private final String name;
    private final int age;
    private final Double height;
    private final Double weight;
    
    @Builder
    @QueryProjection
    public MemberDto (int id, String name, int age, Double height, Double weight){
        this.id = id;
        this.name = name;
        this.age = age;
        this.height = height;
        this.weight = weight;
    }
}

@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;
    
  public List<MemberDto> selectMembers() {
    return query.select(new QMemberDto(member.name, member.age))
            .from(member)
            .fetch();
  }
}
```