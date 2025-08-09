# 개요

- 첫번째 매개변수가 NULL이 아닐시 첫번째 매개변수를 반환, 첫번째 매개변수가 NULL일경우 두번째 매개변수를 반환해주는 SQL 함수
- 둘 다 NULL 일시 NULL을 반환
- CASE WHEN THEN ELSE으로 구현할 수 있는 NULL처리를 간단히 사용하기 위해
- Oracle, MySQL 모두 지원

# 사용법

```sql
COALESCE ( PARAM1, PARAM2 )
```