# 기본

- 차이 분

```sql
TRUNC((SYSDATE - 비교날짜)*24*60)  AS TIME_DIFF_MINUTE
```

- 날짜 차이

```sql
select to_date('2017-08-26','yyyy-mm-dd') - sysdate from dual;
```

- 차이 시간

```sql
select (to_date('2017-08-26','yyyy-mm-dd') - sysdate) * 24 from dual;
```

- 차이 분2

```sql
select (to_date('2017-08-26','yyyy-mm-dd') - sysdate) * 24 * 60 from dual;
```

- 차이 초

```sql
select (to_date('2017-08-26','yyyy-mm-dd') - sysdate) * 24 * 60 * 60 from dual;
```

# DATE컬럼간의 비교

- 두 날짜 사이가 몇 초 차이나는가?

```sql
SELECT ROUND((end_date-start_date)*24*60*60) FROM tb_name;
```

- 두 날짜 사이의 몇 분 차이나는가?

```sql
SELECT ROUND((end_date-start_date)*24*60) FROM tb_name;
```

- 두 날짜 사이의 몇 시간 차이나는가?

```sql
SELECT ROUND((end_date-start_date)*24) FROM tb_name;
```

# 데이터가 문자열, DATE가 아닌 TIMESTAMP 등일때

```sql
SELECT TRUNC(CAST(TS2 AS DATE)) - TRUNC(CAST(TS1 AS DATE)) FROM DUAL;
```

# 년월일 차이 (시간단위 소수점 생략)

```sql
SELECT TRUNC(SYSDATE) - TO_DATE('20171110', 'YYYYMMDD') FROM DUAL;
```

- SYSDATE는 소수점까지 나오기 때문에 그냥 사용하면 시간단위까지 소수점으로 표현됨
- 그래서 SYSDATE를 TRUNC하여 년월일만 남기고 시간을 자름

# 분단위 차이 (초단위 소수점 생략)

```sql
SELECT (TRUNC(SYSDATE, 'MI') - TO_DATE('2021-02-08 18:00:00','YYYY-MM-DD HH24:MI:SS')) * 24 * 60 
FROM DUAL;
```

- SYSDATE를 TRUNC할때 'MI'를 같이 사용하면 분단위까지는 살리고 초단위를 자름
- 24(시간) * 60(분) 해서 수치를 분으로 표현함

# 출처

- [https://blog.naver.com/PostView.nhn?blogId=gyowoog&logNo=222077724044](https://blog.naver.com/PostView.nhn?blogId=gyowoog&logNo=222077724044)