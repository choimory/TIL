# 요약

- fetch join은 지연로딩으로 설정된 연관관계 엔티티를, 주체가 되는 부모 엔티티를 조회할때 연관관계 엔티티도 한번에 같이 조회해오는 JPA 지원 조인 방식이다.
- fetch join으로 연관관계 엔티티별로 쿼리가 불어나는 N+1 현상, 지연로딩 초기화 예외 등을 방지할 수 있다.
- fetch join은 보통 N+1으로 인행 한번에 수행되는 쿼리가 너무 많을수 있는 경우, 연관관계 엔티티의 데이터도 활용되어야 할때 등에 사용할 수 있다.

# 상세

- 지연로딩으로 설정된 연관관계 엔티티는 보통 한번에 조회하지 않는다.
  - 먼저 영속성 컨텍스트가 모든 엔티티를 프록시 객체로 만들어놓고 SQL Storage에 쿼리를 준비해둔뒤
  - 해당 연관관계 엔티티 객체에 접근할때, 준비된 SQL로 조회하여 해당 연관관계 엔티티 데이터를 그때그때 가져오도록 되어있다.
  - 그래서 지연로딩으로 설정된 엔티티는 부모와는 별도의, 연관관계 엔티티만을 위한 조회 쿼리를 마련한 뒤, 객체에 접근할때 해당 쿼리를 추가 수행해서 가져온다.
- 그래서 지연로딩으로 설정된 연관관계 엔티티는 쿼리를 준비할때 join이 아닌 `부모엔티티 조회쿼리 + 연관관계엔티티 조회쿼리` 두개로 찢어져서 준비된다
  - 이로인해 생기는 여러 문제점들을 즉시로딩이 아닌 다른방법으로 해결하는것이 fetch join이다 

# N+1

- 조회된 부모 엔티티의 수만큼 자식 엔티티의 조회 쿼리가 추가 발생하는 현상 (부모 엔티티 조회 1 + 자식엔티티 조회 N)
- 부모 엔티티를 조회할때, 자식 엔티티들은 부모 엔티티를 조회하는 쿼리와 별개로 찢어져서 별도의 쿼리로 수행되게 된다.
- 그러면 부모 엔티티 하나를 조회하기 위해선 `부모엔티티 조회쿼리 + 자식엔티티조회쿼리`가 발생한다
  - 부모 하나에 연관관계에 있는 지연로딩 자식엔티티가 10개라면? 
    - 부모 조회 쿼리 1번 + 각각의 자식 조회 쿼리 10번 = 11개의 조회 쿼리가 수행된다
    - 이 상황에서 부모 목록 20개 조회한다면? 리포지토리 조회 쿼리 하나로 220번의 DB 요청이 들어가게 된다

# LazyInitializeException

# @Query

# @EntityGraph

# Querydsl

# spring.jpa.properties.hibernate.default_batch_fetch_size

# 참고

- [https://jojoldu.tistory.com/165](https://jojoldu.tistory.com/165)
- [https://jojoldu.tistory.com/457](https://jojoldu.tistory.com/457)
- [https://cobbybb.tistory.com/18](https://cobbybb.tistory.com/18)
- [https://www.popit.kr/jpa-n1-%EB%B0%9C%EC%83%9D%EC%9B%90%EC%9D%B8%EA%B3%BC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95/](https://www.popit.kr/jpa-n1-%EB%B0%9C%EC%83%9D%EC%9B%90%EC%9D%B8%EA%B3%BC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95/)
- [https://itmoon.tistory.com/77](https://itmoon.tistory.com/77)
- [https://blog.leocat.kr/notes/2019/05/26/spring-data-using-entitygraph-to-customize-fetch-graph](https://blog.leocat.kr/notes/2019/05/26/spring-data-using-entitygraph-to-customize-fetch-graph)