# Text 출력

- `th:text`
- 단순 텍스트 출력
- 사용예시 :

    ```html
    <span th:text="${객체.변수}">없을시 출력값</span>
    ```

# HTML 출력

- `th:utext`
- 텍스트 내용이 HTML 태그이며, 태그를 적용시키게 하고 싶을때 사용
- 사용예시 :

    ```html
    <div th:utext="${객체.변수}">없을시 출력값</div>
    ```

# 조건문

- `th:if`
- `th:if`의 결과가 true일때만 해당 태그가 작성됨
- 사용예시 :

    ```html
    <td th:if="${result.isActivate == 1}" th:text="${result.isActivate}">isActivate가 1일때만 보임</td>
    ```

# IF ELSE 문

- `th:unless`
- `th:if` 태그와 같은 조건으로 하나의 태그를 더 쓰는데, else에는 `th:unless`를 사용함
- 사용예시 :

    ```html
    <span th:if="${result.isActivate == 1}">사용</span>
    <span th:unless="${result.isActivate == 1}">미사용</span>
    ```

# 삼항 연산자

- `th:text="${조건} ? 'true일시' : 'false일시'"`
- 사용예시 :

    ```html
    <td th:text="${result.isActivate == 1} ? '사용' : '미사용'">미사용</td>
    ```

# IF ELSE IF 문

# 반복문

- `th:each`
- `th:each`가 사용된 태그가 반복해서 작성됨
- 사용예시 :

    ```html
    <table>
    	<thead>
    		...
    	</thead>
    	<tbody>
    		<tr th:each="result:${resultList}">
    			<td th:text="${result.variable}"></td>
    		</tr>
    	</tbody>
    ```

- 객체없이 특정 횟수만큼 반복하고 싶을땐 numbers 객체를 생성하여 사용 :

```sql
<th:block th:each="num : ${#numbers.sequence(1, 10)}">
	..
</th:block>
```

# 반복문 Status 변수

- `[object]Stat` 혹은 `th:each="[object], [status] : ${[list]}"`
- forEach문 기본 변수 뒤에 Stat을 붙이거나, 두번째 변수를 지정
- 혹은 th:each="
    - index : 0부터
    - count : 1부터
    - ~Stat.size : 총 요소 수
    - ~Stat.current : 현재 요소
    - ~Stat.even : 현재 카운트 짝수 여부 boolean
    - ~Stat.odd : 현재 카운트 홀수 여부 boolean
    - ~Stat.first : 현재 반복 첫번째 여부 boolean
    - ~Stat.last : 현재 반복 마지막 여부 boolean
- 사용예시 :

    ```html
    <table>
    	<thead>
    		...
    	</thead>
    	<tbody>
    		<tr th:each="result:${resultList}">
    			<td th:text="${resultStat.index}"></td>
    		</tr>
    	</tbody>

    혹은

    <table>
    	<thead>
    		...
    	</thead>
    	<tbody>
    		<tr th:each="result, status:${resultList}">
    			<td th:text="${status.index}"></td>
    		</tr>
    	</tbody>
    ```

# 변수 선언

- `th:with`
- JSP의 `c:set`의 개념
- 사용 예시 :

    ```html
    <p th:with="varname=${modelname}" th:text="${varname}></p>
    ```

# 스크립트 변수에 초기화

- `"[[${object.variable}]]"`
- 사용예시 :

    ```html
    <script>
    	let userId = '[[${userDto.id}]]`;
    </script>
    ```

# 공통사항

- HTML 태그 없이 사용하고 싶을시 `th:inline`(Thymeleaf 3), `th:remove="tag"` 등을 사용