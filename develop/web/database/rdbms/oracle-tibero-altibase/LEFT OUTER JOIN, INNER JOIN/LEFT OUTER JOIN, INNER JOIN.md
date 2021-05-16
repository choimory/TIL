# 예시 Query

SELECT *

FROM A

JOIN B

ON A.id = B.id

ORDER BY A.id, B.id

# LEFT OUTER JOIN

- 요약

메인 테이블은 모든 레코드를 출력, 조인 테이블은 매칭되는 레코드 혹은 null을 붙여서 출력

## 1:N

A|B
---|---
1|A
1|B
1|C

1:N일시 A테이블의 레코드가 실제로는 한줄이여도, B테이블의 여러 레코드들에 맞추어 여러번 출력됨

## 1:Noting

A|B
---|---
1|null

- 1:Noting일시 2번처럼 A테이블 레코드는 출력되고 B테이블은 값이 null로 채워진채 출력됨

## N:Noting

A|B
---|---
1|null
1|null
1|null

## N:N

A|B
---|---
1.1|A
1.2|B
2.1|C
2.2|D
2.2|E
3.1|F
3.2|null

A테이블 레코드가 더 많은 경우는 B가 null로 붙어서 출력되고

B테이블 레코드가 더 많은 경우는 A가 실제갯수보다 많이 출력됨

# INNER JOIN

- 요약

어느쪽이건 한곳이라도 매칭이 안될시 출력안함

## 1:N

INNER JOIN과 같음

## 1:Noting

조인테이블에 매칭대상이 없으므로 메인테이블도 출력안됨