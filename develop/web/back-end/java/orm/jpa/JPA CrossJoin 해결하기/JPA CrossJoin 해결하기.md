# TODO

---

# 개요

- JPA의 기본 조인은 cross, left Join
- cross join은 1:1 관계를 즉시 로딩할때, left는 1:N 관계를 즉시 로딩할때 구성됨
- cross join은 모든 경우를 고려하여, 자기 자신을 카테시안의 곱으로 레코드를 늘리게 되므로 성능 문제를 야기할 수 있음
- 직접 쿼리를 작성하여 inner, 혹은 left로 join을 변경해주어야 한다

# 1:1 EAGER

# 카테시안 프로덕트 (Cross Join)

# 개선

# 참고

- [카테시안 프로덕트](https://runtoyourdream.tistory.com/95)