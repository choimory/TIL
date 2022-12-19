# init

- Spring Initializer가 Kotlin을 지원하므로 해당 방법으로 진행합니다

# kt lint

- TODO

# all-open

- Kotlin으로 작성된 클래스와 그 하위 필드들은 Java로 변환할때, 기본적으로 모두 `final`을 부여하여 변환합니다
- 하지만 Spring에서는 다양한 `Proxy 패턴`이 존재하여 final을 부여하여 설계할 시 많은 난관이 있을수 있습니다
- 그래서 Kotlin은 기본적인 `Spring 관련 Annotation`이 붙은 클래스에 대해서는, final이 아닌 open 키워드를 붙여 생성합니다
- 그 외에 부가적으로 final로 생성되면 안되는 클래스에 대해서는 `all-open`이라는 플러그인을 설치하여 반영합니다

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class AllOpen()
```

- 먼저 all-open 처리할 어노테이션을 작성합니다

```
plugins {
    id 'org.springframework.boot' version '2.7.6'
    id 'io.spring.dependency-management' version '1.0.15.RELEASE'
    id 'org.asciidoctor.convert' version '1.5.8'
    id 'org.jetbrains.kotlin.jvm' version '1.6.21'
    id 'org.jetbrains.kotlin.plugin.spring' version '1.6.21'
    id 'org.jetbrains.kotlin.plugin.jpa' version '1.6.21'
    //추가
    id 'org.jetbrains.kotlin.plugin.allopen' version '1.6.21' 
}

//추가
allOpen {
    annotation('com.choimory.common.annotation.AllOpen') 
}
....
```

- all-open 플러그인을 추가하고, 만들어둔 어노테이션을 all-open 타겟으로 추가합니다