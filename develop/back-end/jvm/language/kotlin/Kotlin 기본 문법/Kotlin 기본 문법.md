# 추론

```kotlin
fun cal(a:Int, b:Int):Int {
	return a+b
}

->

fun cal(a:Int, b:Int) = a+b
```

- 함수 추론 가능할시 축약이 가능합니다