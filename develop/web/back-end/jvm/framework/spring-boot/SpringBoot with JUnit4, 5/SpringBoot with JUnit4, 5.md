# JUnit 4

```jsx
@RunWith(SpringJUnit4ClassRunner.class)
public class 테스트클래스명{
	...
}
```

# JUnit 5

```jsx
@ExtendWith(SpringExtension.class)
public class 테스트클래스명{
	...
}
```

- JUnit 5에서는 `@RunWith(SpringJnit4ClassRunner.class)`대신 `@ExtendWith(SpringExtension.class)`를 사용한다.
    - RunWith을 사용하고 싶으면 굳이 쓸수는 있다.

```jsx
@SpringBootTest
public class 테스트클래스명{
	...
}
```

- 최근 Springboot에서는 `@SpringBootTest`에 `@ExtendWith(SpringExtension.class)`을 포함시킴으로써 `@SpringBootTest`로 더 간략하게 사용할 수 있다.