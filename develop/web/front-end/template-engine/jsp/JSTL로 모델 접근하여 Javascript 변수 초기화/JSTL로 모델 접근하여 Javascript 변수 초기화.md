- 백단에서 모델에 담아 넘겨준 객체를 자바스크립트 변수로 옮겨 자바스크립트 코드로 작업할 때 primitve 타입과 String은 바로 넘겨주는것이 가능하지만 그 외 Object는 불가능
    - 자바의 객체와 자바스크립트의 객체는 연관이 없기 때문
- 그래서 Model의 값이 기본형인 경우는 EL로 바로 집어넣어주지만 객체의 경우 객체에서 값을 JSTL로 꺼내서 넣어줌

기본형 혹은 String의 경우

```jsx
var tmp = "${string}";
```

그 외 객체의 경우

```jsx
var tmp = [];
tmp.push("voNm","${vo.val1}");
tmp.push("voNm2","${vo.val2}");
```