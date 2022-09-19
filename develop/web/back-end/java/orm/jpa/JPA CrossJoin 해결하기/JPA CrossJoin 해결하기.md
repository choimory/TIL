# 들어가며

- JPA의 기본 조인은 cross, left Join
- cross join은 1:1 관계를 즉시 로딩할때, left는 1:N 관계를 즉시 로딩할때 구성됨
- cross join은 모든 경우를 고려하여, 자기 자신을 카테시안의 곱으로 레코드를 늘리게 되므로 성능 문제를 야기할 수 있음
- 직접 쿼리를 작성하여 inner, 혹은 left로 join을 변경해주어야 한다

# 1:1 EAGER

- 기본적으로 JPA는 1:1 즉시로딩을 Cross join으로 처리한다.
- 이 Cross join은 모든 경우의 수를 고려한 레코드를 만들어내기 때문에, 레코드가 크게 증가하여 성능에 악영향을 끼친다.

# 카테시안 프로덕트 (Cross Join)

- Cross join은 Catesian product라고도 하는데, 모든 경우의 수를 만들기 위해 카테시안 곱으로 레코드가 증가한다.
- 때문에 Cross join을 inner join이나 left join으로 직접 명시한 쿼리로 변경해 주어야 할 필요가 있다.

# 개선

```java
    @Override
    public List<MemberDto> findAllNoOffset(int lastId, int size, String identity, String nickname, String email, AuthLevel authLevel, LocalDateTime createdFrom, LocalDateTime createdTo, LocalDateTime modifiedFrom, LocalDateTime modifiedTo, LocalDateTime deletedFrom, LocalDateTime deletedTo) {
        return query.select(
                    Projections.fields(MemberDto.class,
                            member.id,
                            member.identity,
                            member.nickname,
                            Projections.fields(MemberDto.MemberAuthorityDto.class,
                                    memberAuthority.authLevel
                            ).as("memberAuthority")
                    )
                )
                .from(member)
                .innerJoin(member.memberAuthority, memberAuthority);
                .where(gtId(lastId),
                        eqIdentity(identity),
                        containsNickname(nickname),
                        containsEmail(email),
                        eqAuthLevel(authLevel),
                        betweenCreatedAt(createdFrom, createdTo),
                        betweenModifiedAt(modifiedFrom, modifiedTo),
                        betweenDeletedAt(deletedFrom, deletedTo))
                .fetch();
    }
```

- Cross join의 대상이 되는 1:1 관계의 엔티티를 조인으로 명시하여 Projections으로 풀어 받는다

# 참고

- [카테시안 프로덕트](https://runtoyourdream.tistory.com/95)