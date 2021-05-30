# 개요

> SQL문에 <, >등의 부등호가 들어갔을댄 해당 부분을 CDATA로 감싸서 처리해 주어야 한다.

> iBatis / Mybatis가 XML 태그를 이용해 작성하는데, 쿼리에 부등호가 들어가면 이게 XML 태그인지 쿼리인지 헷갈려하기 때문이다.

- 예시

    ```sql
    SELECT * FROM TABLE WHERE DATE > #{date}
    ->
    SELECT * FROM TABLE WHERE DATE <![CDATA[ > ]> #{date}
    ```

    ```sql
    SELECT REPLACE(content, '\r\n', '<br>') as content
    FROM....
    ->
    SELECT REPLACE(content, '\r\n', <![CDATA[ '<br>' ]]>) as content
    FROM....
    ```