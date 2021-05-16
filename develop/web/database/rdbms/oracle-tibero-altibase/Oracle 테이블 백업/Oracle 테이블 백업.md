# 백업방식

1. 백업테이블을 하나 만든다
2. .sql 파일 덤프뜬다
3. .csv 파일로 저장해놓는다

보통 1번 방법을 사용

# 1번

```sql
/*복제 테이블 만들고 레코드까지 저장*/
CREATE TABLE 테이블명 AS SELECT * FROM 원본테이블명

/*복제 테이블만 생성하고 레코드는 저장하지 않음*/
CREATE TABLE 테이블명 AS SELECT * FROM 원본테이블명 WHERE 1=0
```

- Key등의 Table DDL 관련은 저장되지 않고 순수히 레코드만을 저장하는 방식으로 사용된다