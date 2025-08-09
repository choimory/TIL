1. script 태그에 `th:inline="javascript"` 속성을 부여
2. `<![CDATA[ ]]>`태그를 작성한 뒤 해당 태그 안에 `[[${key.val}]]`지시자를 비롯한 코드들을 집어 넣은 뒤
3. `CDATA` 태그와 `지시자`에 주석처리

```jsx
<script th:inline="javascript">
	/* <![CDATA[ */
	alert( /* [[${key.val}]] */ );
	/* ]]> */
</script>
```