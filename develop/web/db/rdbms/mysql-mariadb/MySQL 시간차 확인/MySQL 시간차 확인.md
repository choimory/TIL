# 사용법

```sql
SELECT TIMESTAMPDIFF(YEAR||MONTH||WEEK||DAY||HOUR||MINUTE||SECOND, 비교날짜1, 비교날짜2)
```

# 활용 예시

```sql
select (select TIMESTAMPDIFF (MINUTE, date_format(#{REGIST_DT}, '%Y-%m-%d %H:%i'), date_format(now(), '%Y-%m-%d %H:%i')  )) as timediffmin 
from tb_mark_answer tma 
```