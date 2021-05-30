# 특정 년도

```sql

```

# 특정 월

```sql
//특정 월 (한달)
SELECT * FROM SYSTEM_MANAGER_LOG sml WHERE REG_DT BETWEEN TO_DATE('2017-10', 'yyyy-MM') AND ADD_MONTHS(TO_DATE('2017-10', 'yyyy-MM'), 1)
//특정 월 ~ 특정 월
SELECT * FROM SYSTEM_MANAGER_LOG sml WHERE REG_DT BETWEEN TO_DATE('2017-10', 'yyyy-MM') AND ADD_MONTHS(TO_DATE('2022-11', 'yyyy-MM'), 1)
//특정 월 ~ 특정 월로부터 N개월
SELECT * FROM SYSTEM_MANAGER_LOG sml WHERE REG_DT BETWEEN TO_DATE('2017-10', 'yyyy-MM') AND ADD_MONTHS(TO_DATE('2017-10', 'yyyy-MM'), N))
```

# 특정 일

```sql
//특정 일 (하루)
SELECT * FROM SYSTEM_MANAGER_LOG sml WHERE REG_DT BETWEEN TO_DATE('2017-10-26', 'yyyy-MM-dd') AND TO_DATE('2017-10-26', 'yyyy-MM-dd')+1
//특정 일 ~ 특정 일
SELECT * FROM SYSTEM_MANAGER_LOG sml WHERE REG_DT BETWEEN TO_DATE('2017-10-26', 'yyyy-MM-dd') AND TO_DATE('2017-11-11', 'yyyy-MM-dd')+1
//특정 일 ~ 특정 일로부터 N일
SELECT * FROM SYSTEM_MANAGER_LOG sml WHERE REG_DT BETWEEN TO_DATE('2017-10-26', 'yyyy-MM-dd') AND TO_DATE('2017-10-26', 'yyyy-MM-dd')+N(특정일로부터 원하는만큼의 범위)
```