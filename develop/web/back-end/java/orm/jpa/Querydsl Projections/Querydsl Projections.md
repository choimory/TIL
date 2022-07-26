# 개요

- Querydsl Projections에 대해 알아본다

# Querydsl Projections을 권장한다

- 기본 엔티티를 넘기는것보다 Projections를 통해 매핑한 DTO 등의 객체를 넘기는것을 권장한다.
  - 이는 서비스단에서 매핑 작업을 추가적으로 거치는것보다 성능상으로 이득이 있기 때문이다.
    - 필요한 컬럼만 조회할 수 있다는 점.
    - 엔티티를 건내면 Hibernate 1차 캐싱이 적용되어 성능이 하락되므로, 엔티티 대신 DTO로 바로 건내주는것에 이득이 있다. 

# Querydsl Projections의 종류

- Projections에는 constructor, fields, bean 세가지가 존재하며 각각의 유의사항이 있다.

## Projections.constructor

- 생성자를 통해 객체를 생성한다
- 주의사항: 생성자를 빌더패턴으로 적용하였다 하더라도 특정 필드만 초기화하는것이 불가능하다.
  - Projections.constructor()를 통해 특정 필드만 초기화하고 싶을시, 해당 필드들을 넘겨받는 생성자를 선언해야 한다.
    - 상황에 따라 동적으로 하고 싶을땐 케이스별로 모든 생성자가 있어야 한다. 빌더로는 불가능.
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
  - field에 직접 접근하지만 접근제어자가 private인것은 문제되지 않는다.

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

## DTO 안의 객체에 매핑하기

- `MemberDto`내의 `MemberAuthorityDto` 필드를 매핑해보자

```java
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Getter
public class MemberDto {
  private Long id;
  private String identity;
  private String nickname;
  private String email;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private LocalDateTime deletedAt;
  private MemberAuthorityDto memberAuthority;
  private List<MemberSocialDto> memberSocials;
  private List<MemberSuspensionDto> memberSuspensions;

  @Builder
  @NoArgsConstructor
  @AllArgsConstructor
  @Getter
  public static class MemberAuthorityDto {
    private AuthLevel authLevel;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime createdAt;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime modifiedAt;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime deletedAt;
  }
}
```

```java
@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;

  @Override
  public List<MemberDto> findAllNoOffset(int lastId, int size, String identity, String nickname, String email, AuthLevel authLevel, LocalDateTime createdFrom, LocalDateTime createdTo, LocalDateTime modifiedFrom, LocalDateTime modifiedTo, LocalDateTime deletedFrom, LocalDateTime deletedTo) {
    return query.select(Projections.fields(MemberDto.class,
            member.identity,
            Projections.fields(MemberDto.MemberAuthorityDto.class,
                    memberAuthority.authLevel).as("memberAuthority")))
            .from(member)
            .innerJoin(member.memberAuthority, memberAuthority)
            .where(gtId(lastId),
                    eqIdentity(identity),
                    containsNickname(nickname),
                    containsEmail(email),
                    eqAuthLevel(authLevel),
                    betweenCreatedAt(createdFrom, createdTo),
                    betweenModifiedAt(modifiedFrom, modifiedTo),
                    betweenDeletedAt(deletedFrom, deletedTo))
            .limit(size)
            .fetch();
  }
}
```

- Projections 안에 추가로 Projections를 넣어주고 as로 alias를 지정해주면 된다

## DTO 안의 컬렉션 객체에 매핑하기

- `MemberDto`내의 `List<MemberSocialDto>` 필드를 매핑해보자

```java
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Getter
public class MemberDto {
  private Long id;
  private String identity;
  private String nickname;
  private String email;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private LocalDateTime createdAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private LocalDateTime modifiedAt;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
  private LocalDateTime deletedAt;
  private MemberAuthorityDto memberAuthority;
  private List<MemberSocialDto> memberSocials;
  private List<MemberSuspensionDto> memberSuspensions;

  @Builder
  @NoArgsConstructor
  @AllArgsConstructor
  @Getter
  @Setter
  public static class MemberSocialDto {
    private SocialType socialType;
    private String socialId;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime createdAt;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime modifiedAt;
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")
    private LocalDateTime deletedAt;
  }
}
```

```java
@Repository
@RequiredArgsConstructor
public class repo {
  private final QueryFactory query;
  
  @Override
  public List<MemberDto> findAllNoOffset(int lastId, int size, String identity, String nickname, String email, AuthLevel authLevel, LocalDateTime createdFrom, LocalDateTime createdTo, LocalDateTime modifiedFrom, LocalDateTime modifiedTo, LocalDateTime deletedFrom, LocalDateTime deletedTo) {
        /*return query.select(Projections.fields(MemberDto.class,
                member.identity,
                Projections.fields(MemberDto.MemberAuthorityDto.class,
                        memberAuthority.authLevel).as("memberAuthority")),
                Projections.list(Projections.fields(MemberDto.MemberSocialDto.class,
                        memberSocial.socialType,
                        memberSocial.socialId).as("memberSocials")))
                .from(member)
                .innerJoin(member.memberAuthority, memberAuthority)
                .where(gtId(lastId),
                        eqIdentity(identity),
                        containsNickname(nickname),
                        containsEmail(email),
                        eqAuthLevel(authLevel),
                        betweenCreatedAt(createdFrom, createdTo),
                        betweenModifiedAt(modifiedFrom, modifiedTo),
                        betweenDeletedAt(deletedFrom, deletedTo))
                .limit(size)
                .fetch();*/

    return query.selectFrom(member)
            .innerJoin(member.memberAuthority, memberAuthority)
            .leftJoin(member.memberSocials, memberSocial)
            .where(gtId(lastId),
                    eqIdentity(identity),
                    containsNickname(nickname),
                    containsEmail(email),
                    eqAuthLevel(authLevel),
                    betweenCreatedAt(createdFrom, createdTo),
                    betweenModifiedAt(modifiedFrom, modifiedTo),
                    betweenDeletedAt(deletedFrom, deletedTo))
            .limit(size)
            .transform(GroupBy.groupBy(member.id)
                    .list(Projections.fields(MemberDto.class,
                            member.identity,
                            Projections.fields(MemberDto.MemberAuthorityDto.class,
                                    memberAuthority.authLevel).as("memberAuthority"),
                            /*Projections.fields(MemberDto.MemberSocialDto.class,
                                    memberSocial.socialType,
                                    memberSocial.socialId).as("memberSocials"),
                            Projections.list(Projections.fields(MemberDto.MemberSocialDto.class,
                                    memberSocial.socialType,
                                    memberSocial.socialId).as("memberSocials")),*/
                            GroupBy.list(Projections.fields(MemberDto.MemberSocialDto.class,
                                    memberSocial.socialType,
                                    memberSocial.socialId).as("memberSocials"))
                    )));
  }
}
```

- 라고 하는데 일단 오류가 나서 안되고 있다(...)

- 불가능하다? 
  - https://www.inflearn.com/questions/149985
  - https://stackoverflow.com/questions/66366976/querydsl-how-to-return-dto-list
- 가능하다?
  - https://bbuljj.github.io/querydsl/2021/05/17/jpa-querydsl-projection-list.html
  - https://stackoverflow.com/questions/17116711/collections-in-querydsl-projections
  - 첫번째로 컬렉션을 조회한다는것은 1:N 관계의 테이블을 조회한다는점이므로 join이 들어간다는 뜻이 된다
  - 때문에 카테시안 곱으로 인해 주체 엔티티가 중복 조회되므로, distinct 혹은 groupby 처리가 들어가야 한다
    - 이때 groupby 쿼리를 날리는것보단 querydsl의 transfrom()을 이용해 후처리하는것이 성능 상 최선인것처럼 보인다
    - 하지만 이것 역시 성능상 이점을 가져가는것은 아니므로.. 된다고 해도 조회할 필드를 최소화한 뒤, 별도의 조회용 객체를 만들고 그 필드는 최대한 flat하게 최소한으로 설계하는것이 사실은 가장 바람직할 것 같다
      - 즉 이렇게까지 쿼리를 짜는것 자체가 결국은 Projections로 골라서 조회해도 성능상 이점은 크게 죽을것 같다..
