# 상황

AJAX로 보낼 JSON에 JS배열을 포함시켜 AJAX 전송

```jsx
var data={
.....
arr : myArray
}
```

# 오류

```
java.lang.NumberFormatException: For input string: ""
.......이하생략
```

# 원인

배열을 AJAX로 전송시 `data[]=val & data[]=val` 형식으로 넘어가는데 여기서 `[]`같은 괄호로 인해 DispatcherServlet가 매핑하는 과정에서 오류가 발생함.

# 해결

## 1번 방법. $.ajax() 호출 전 설정

```jsx
$.ajaxSettings.traditional=true;
```

## 2번 방법. $.ajax() 호출 내 설정

```jsx
$.ajax({
	traditional:true,
	......
});
```

설정시 `data=val & data=val`형식으로 변경되어 전송됨