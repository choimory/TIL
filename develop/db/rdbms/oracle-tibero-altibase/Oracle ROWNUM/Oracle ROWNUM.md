오라클은 Limit 대신 Rownum 사용

# Mysql, Mariadb

```sql
SELECT * 
FROM TB
LIMIT (0, 10)
```

# Oracle

데이터를 출력하는 실제 쿼리를 FROM절 서브쿼리로 처리한뒤 ROWNUM 관련 쿼리로 한겹 씌워줘야함

```sql
SELECT a.* 
FROM (SELECT * 
			FROM TB) a 
WHERE ROWNUM ≤10
```