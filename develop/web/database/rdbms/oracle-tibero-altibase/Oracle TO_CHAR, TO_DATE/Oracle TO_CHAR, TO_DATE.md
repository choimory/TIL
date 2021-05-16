# 개요

날짜 형식 포매팅해서 넣고 받는 SQL함수

DBMS별로 다르다

# 오라클

- TO_CHAR(값, 형식)

  DATE 컬럼 SELECT 할때 사용 (DATE 컬럼 → 문자열)

    ```sql
    SELECT COL
    				, COL
    				, TO_CHAR(REG_DT, 'YYYY-MM-DD HH24:mm:ii') as REG_DT
    FROM TB
    ```

- TO_DATE(값, 형식)

  DATE 컬럼 INSERT할때 사용 (문자열 → DATE 컬럼)

    ```sql
    INSERT INTO TB (COL
    								, COL
    								, REG_DT) 
    VALUES (var
    				, var
    				, TO_DATE(datetime, 'YYYY-MM-DD HH24:mm:ii'))
    ```

  DATE 컬럼 WHERE 조건에 사용

    ```sql
    SELECT *
    FROM TB
    WHERE REG_DT
    ```

- 형식
    - YYYY-MM-DD HH24:mm:ss

# MySQL, MariaDB

- DATE_FORMAT(값, 형식)

  INSERT, SELECT 모두 해당함수 사용

    ```sql

    ```

- 형식
    - %Y-%m-%d %H:%i