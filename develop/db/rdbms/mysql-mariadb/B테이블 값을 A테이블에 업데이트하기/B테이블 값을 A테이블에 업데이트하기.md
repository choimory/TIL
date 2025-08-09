# 개요

- 자식 테이블에 있는 컬럼값을 부모 테이블에 씌우고 싶을때\
- B테이블 값을 조인걸어서 A테이블에 업데이트하기

# 쿼리

```sql
UPDATE 테이블A
JOIN 테이블B
ON A테이블컬럼 = B테이블컬럼
SET A테이블컬럼 = B테이블컬럼
WHERE 조건
```

```sql
UPDATE 테이블A
JOIN 테이블B
ON A테이블컬럼 = B테이블컬럼
SET A테이블컬럼 = CASE WHEN 조건 THEN B테이블컬럼 ELSE A테이블컬럼 END 
WHERE 조건
```

- 특정 조건에만 변경하고 싶을때