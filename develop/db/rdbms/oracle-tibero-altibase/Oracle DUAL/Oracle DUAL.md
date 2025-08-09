# 개요

오라클, mysql 등에서 제공하는 기본 테이블이며

함수, 연산용으로 계산한 결과값을 확인할때 사용하는 임시테이블로 활용됨

# SQL

```sql
SELECT 연산, 함수실행 결과, 문자 출력값 등
FROM DUAL
----------
SELECT 20*3 
FROM DUAL
---------
SELECT SUBSTR('20190820',1,4)
FROM DUAL

/*주로 SELECT에 연산작성 뒤 FROM을 명시하긴 해야하니까 대충 DUAL을 넣음*/
```