# 개요

해당방식은 PK같은 테이블 key까지 똑같은 테이블을 생성하지는 않는다.

임시 테이블에 원본 레코드만 담아둔다고 생각하는게 편하다.

# Oracle

- CREATE 백업테이블명 AS SELECT * FROM 원본테이블명
- CREATE 백업테이블명 AS SELECT * FROM 원본테이블명 WHERE 1=0

  레코드 없이 테이블만 생성됨. 그러나 PK같은 Key설정은 제외됨

# MySQL, MariaDB

- CREATE 백업테이블명 SELECT * FROM 원본테이블명
- CREATE 백업테이블명 AS SELECT * FROM 원본테이블명 WHERE 1=0

  레코드 없이 테이블만 생성됨. 그러나 PK같은 Key설정은 제외됨