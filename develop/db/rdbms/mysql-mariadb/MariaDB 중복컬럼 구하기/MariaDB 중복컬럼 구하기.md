# 중복건수 구하기

```sql
SELECT 컬럼명, COUNT(*) as cnt
FROM 테이블명
GROUP BY 컬럼명
HAVING cnt > 1;
```

# 중복 레코드 상세값 확인하기 (중복 레코드 중 한개를 대상으로 함)

```sql
SELECT *
FROM 테이블명
GROUP BY 컬럼명
HAVING COUNT(*) > 1;
```