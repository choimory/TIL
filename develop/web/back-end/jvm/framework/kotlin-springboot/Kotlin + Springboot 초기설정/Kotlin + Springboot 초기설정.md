# init

- Spring Initializer가 Kotlin을 지원하므로 해당 방법으로 진행합니다

# Gradle Kotlin

- Gradle Groovy가 아닌 Gradle Kotlin으로 생성시, 문법 문제로 오류가 날 수 있으므로, Kotlin 문법으로 Gradle를 수정해줍니다

```kotlin
// 예시 - JUnit 관련 오류
```

# kt lint

- Homebrew, Gradle, IntelliJ IDEA 세가지 방법 중 선택하여 설치가 가능합니다

## Homebrew를 이용한 설치

- 사용되는 시스템에 설치하여 CLI로 사용하는 방식입니다 
- 권장되지 않는 방법이며, 효율적이지도 않다는 생각이 들어 생략하는것으로..

## Gradle를 이용한 설치

- 프로젝트에 ktlint 플러그인을 설치하는 방법입니다
- 해당 프로젝트 내에 lint 설정 파일인 `.editorconfig` 파일을 생성하여 공유하므로 모두가 같은 컨벤션을 유지할 수 있습니다.

## IntelliJ를 이용한 설치

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
    id("org.springframework.boot") version "2.7.6"
    id("io.spring.dependency-management") version "1.0.15.RELEASE"
    id("org.asciidoctor.convert") version "1.5.8"
    id("org.jetbrains.kotlin.plugin.allopen") version "1.6.21" //all-open 추가
    //id("org.jlleitschuh.gradle.ktlint") version "9.1.0"
    kotlin("jvm") version "1.6.21"
    kotlin("plugin.spring") version "1.6.21"
    kotlin("plugin.jpa") version "1.6.21"
}

//추가
allOpen {
    //final이 되어선 안되는 특정 클래스에 활용하기 위한 커스텀 어노테이션
    annotation("com.choimory.kotlininaction.common.annotation.AllOpen")

    //Hibernate의 Entity class 및 instance field는 final이 되어선 안된다 (프록시 패턴으로 동작하기 때문에) - 공식문서
    annotation("javax.persistence.Entity")
    annotation("javax.persistence.MappedSuperclass")
    annotation("javax.persistence.Embeddable") 
}
....
```
- 
- all-open 플러그인을 추가하고, 만들어둔 어노테이션을 all-open 타겟으로 추가합니다

# Gradle Kotlin JUnit 오류 수정

```kotlin
val snippetsDir by extra { file("build/generated-snippets") }

tasks.withType<KotlinCompile> {
    kotlinOptions {
        freeCompilerArgs = listOf("-Xjsr305=strict")
        jvmTarget = "11"
    }
}

tasks.withType<Test> {
    useJUnitPlatform()
}

tasks.test {
    outputs.dir(snippetsDir)
}
```

- Gradle Kotlin으로 진행할 시, JUnit 관련 문법 오류를 수정합니다

# 참고

- https://blog.benelog.net/ktlint.html#gradle_%EB%B9%8C%EB%93%9C_%EC%84%A4%EC%A0%95
- https://velog.io/@eastperson/Kotlin-Spring-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B01-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1