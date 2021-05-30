# 개요

- 매개변수 위/경도와 특정 DB 레코드의 위/경도간의 반경(거리차)를 구하는 SQL
- 보통 매개변수는 나의 위/경도, DB 레코드는 특정 건물의 위/경도가 활용됨
- 단위는 KM ( 1 = 1km, 0.1 = 100m)
- 구한 거리차를 조건으로 다양하게 활용할 수 있다
    - 나와의 거리차가 특정 거리차 이내인 레코드만 조회
    - 나와의 거리차가 가까운순으로 5개 조회
    - 등등

# MySQL

## 1. 거리식

```sql
( 6371 * acos( cos( RADIANS('현재위도') ) * cos( RADIANS('대상DB위도') )
          * cos( RADIANS('대상DB경도') - RADIANS('현재경도') )
          + sin( RADIANS('현재위도') ) * sin( RADIANS('대상DB위도') ) 
				) 
) AS distance

---

(6371 * ACOS( COS( RADIANS(#{la}) ) * COS(RADIANS(LA) ) 
							* COS( RADIANS(LO) - RADIANS(#{lo}) ) 
							+ SIN( RADIANS(#{la}) ) * SIN(RADIANS(LA) )
				)
) AS DISTANCE
```

- 6371은 지구의 반지름 6371km이므로 고정값이다

## 2. 거리식을 이용한 조건처리

```sql
SELECT
			*, 
			(6371 * ACOS(COS(RADIANS(#{la})) * COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo})) + SIN(RADIANS(#{la})) * SIN(RADIANS(LA)))) AS DISTANCE
FROM TB_SNS_CHANNEL
HAVING DISTANCE <![CDATA[ <= ]]> #{distance} /* 0.1 = 100m */
ORDER BY DISTANCE
```

- 특정 거리차 내의 레코드만 조회하는 SQL문이다

# Oracle

## 1. Radians 함수 선언

```sql
CREATE OR REPLACE FUNCTION SNS.RADIANS(nDegrees IN NUMBER) RETURN NUMBER DETERMINISTIC IS
BEGIN
	RETURN nDegrees / 57.29577951308232087679815481410517033235;
END RADIANS;
```

- 오라클은 Radians 함수가 없으므로 직접 선언해준다

## 2. 거리식

```sql
(6371 * ACOS(
		    	CASE
		    	    WHEN COS(RADIANS('현재위도'))* COS(RADIANS('DB위도')) * COS(RADIANS('DB경도') - RADIANS('현재경도')) + SIN(RADIANS('현재위도')) * SIN(RADIANS('DB위도')) > 1 THEN 1
		    	    WHEN COS(RADIANS('현재위도'))* COS(RADIANS('DB위도')) * COS(RADIANS('DB경도') - RADIANS('현재경도')) + SIN(RADIANS('현재위도')) * SIN(RADIANS('DB위도')) < -1 THEN -1
		    		ELSE COS(RADIANS('현재위도'))* COS(RADIANS('DB위도')) * COS(RADIANS('DB경도') - RADIANS('현재경도')) + SIN(RADIANS('현재위도')) * SIN(RADIANS('DB위도'))
		    	END
		    )
		) AS DISTANCE

---

(6371 * ACOS(
		    	CASE
		    	    WHEN COS(RADIANS(#{la, jdbcType=INTEGER}))* COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo, jdbcType=INTEGER})) + SIN(RADIANS(#{la, jdbcType=INTEGER})) * SIN(RADIANS(LA)) <![CDATA[ > ]]> 1 THEN 1
		    	    WHEN COS(RADIANS(#{la, jdbcType=INTEGER}))* COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo, jdbcType=INTEGER})) + SIN(RADIANS(#{la, jdbcType=INTEGER})) * SIN(RADIANS(LA)) <![CDATA[ < ]]> -1 THEN -1
		    		ELSE COS(RADIANS(#{la, jdbcType=INTEGER}))* COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo, jdbcType=INTEGER})) + SIN(RADIANS(#{la, jdbcType=INTEGER})) * SIN(RADIANS(LA))
		    	END
		    )
		) AS DISTANCE
```

- 6371은 지구의 반지름 6371km이므로 고정값이다
- Oracle의 ACOS(arg)는 인자값이 -1보다 낮거나, 1보다 클 경우 오류를 발생시키므로 처리해준다

## 3. 거리식을 이용한 조건처리

```sql
SELECT *
		FROM (SELECT tsc.*
					, (6371 * ACOS(
					    	CASE
					    	    WHEN COS(RADIANS(#{la, jdbcType=INTEGER}))* COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo, jdbcType=INTEGER})) + SIN(RADIANS(#{la, jdbcType=INTEGER})) * SIN(RADIANS(LA)) <![CDATA[ > ]]> 1 THEN 1
					    	    WHEN COS(RADIANS(#{la, jdbcType=INTEGER}))* COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo, jdbcType=INTEGER})) + SIN(RADIANS(#{la, jdbcType=INTEGER})) * SIN(RADIANS(LA)) <![CDATA[ < ]]> -1 THEN -1
					    		ELSE COS(RADIANS(#{la, jdbcType=INTEGER}))* COS(RADIANS(LA)) * COS(RADIANS(LO) - RADIANS(#{lo, jdbcType=INTEGER})) + SIN(RADIANS(#{la, jdbcType=INTEGER})) * SIN(RADIANS(LA))
					    	END
					    )
					) AS DISTANCE
		    	FROM TB_SNS_CHANNEL tsc)
WHERE DISTANCE <![CDATA[ <= ]]> #{distance, jdbcType=INTEGER} /*0.1=100m*/
ORDER BY DISTANCE
```

- 특정 거리차 내의 레코드만 조회하는 SQL문이다

# 참고

- [https://devzeroty.tistory.com/entry/특정-위치위도-경도에서-반경-안에-드는-장소-SQL로-조회하기](https://devzeroty.tistory.com/entry/%ED%8A%B9%EC%A0%95-%EC%9C%84%EC%B9%98%EC%9C%84%EB%8F%84-%EA%B2%BD%EB%8F%84%EC%97%90%EC%84%9C-%EB%B0%98%EA%B2%BD-%EC%95%88%EC%97%90-%EB%93%9C%EB%8A%94-%EC%9E%A5%EC%86%8C-SQL%EB%A1%9C-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0)
- [https://tonyne.jeju.onl/2016/04/14/oracle-lat-lng-distnace-query/](https://tonyne.jeju.onl/2016/04/14/oracle-lat-lng-distnace-query/)
- [https://okky.kr/article/405632?note=1272836](https://okky.kr/article/405632?note=1272836)
- [https://pnot.tistory.com/8](https://pnot.tistory.com/8)
- [http://blog.naver.com/PostView.nhn?blogId=blogpyh&logNo=220464780466&parentCategoryNo=&categoryNo=20&viewDate=&isShowPopularPosts=true&from=search](http://blog.naver.com/PostView.nhn?blogId=blogpyh&logNo=220464780466&parentCategoryNo=&categoryNo=20&viewDate=&isShowPopularPosts=true&from=search)
- [https://aljjabaegi.tistory.com/336](https://aljjabaegi.tistory.com/336)
- [https://stackoverflow.com/questions/24937868/oracle-circle-distance-search-missing-results](https://stackoverflow.com/questions/24937868/oracle-circle-distance-search-missing-results)
- [https://ncube.net/경도-위도를-이용한-기준-반경-내-업체-구하기-php-mysql-이용/](https://ncube.net/%EA%B2%BD%EB%8F%84-%EC%9C%84%EB%8F%84%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EA%B8%B0%EC%A4%80-%EB%B0%98%EA%B2%BD-%EB%82%B4-%EC%97%85%EC%B2%B4-%EA%B5%AC%ED%95%98%EA%B8%B0-php-mysql-%EC%9D%B4%EC%9A%A9/)
- [https://stackoverflow.com/questions/28338856/lat-long-distance-calculation](https://stackoverflow.com/questions/28338856/lat-long-distance-calculation) (오라클 ACOS 에러 관련)