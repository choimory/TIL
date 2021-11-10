# 개요

- index size 문제로 인덱스가 안걸릴때 해야할 것
    1. cnf 설정 변경
    2. 테이블에 row_format=dynamic 추가하기
- 그 중 2번 방법에 대해 작성함

# CREATE

```sql
CREATE TABLE 테이블명(
	...
)COLLATE='utf8_bin'
ENGINE=InnoDB
ROW_FORMAT=DYNAMIC
;
```

# ALTER

```sql
ALTER TABLE 테이블명 ROW_FORMAT=DYNAMIC
```

# 글로벌 설정

```sql
SET GLOBAL innodb_default_row_format=DYNAMIC;
```

- 글로벌 설정 (mariadb 10.2 이상)

```sql
OPTIMIZE TABLE DB명.테이블명;
```

- 테이블 설정을 글로벌 설정 값으로 반영