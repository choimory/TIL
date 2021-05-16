# 개요

하나의 조건에 여러 값들을 대입

# SQL

```sql
/*col값이 a 혹은 b 혹은 c 혹은 d인것을 검색*/
SELECT *
FROM TB
WHERE COL IN (a,b,c,d)
```

```sql
/*col값이 a 혹은 b 혹은 c 혹은 d인것을 제외*/
SELECT * 
FROM TB
WHERE COL NOT IN (a,b,c,d)
```

```sql
/*with ibatis*/
SELECT *
FROM TB
WHERE COL IN 
<iterate property ="var" open="(" close=")" conjunction=",">
	#list[]#
</iterate>
```