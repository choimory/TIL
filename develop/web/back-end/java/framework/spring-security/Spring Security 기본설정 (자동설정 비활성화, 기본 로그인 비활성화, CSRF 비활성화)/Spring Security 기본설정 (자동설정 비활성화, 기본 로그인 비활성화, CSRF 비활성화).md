# 개요

- Spring security를 시작할때 가장 기본적인 설정을 작성함

# 스프링 시큐리티 비활성화

```java
@SpringBootApplication(exclude = {SecurityAutoConfiguration.class})
public class UserApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(UserApiApplication.class, args);
    }

}
```

- `@EnableWebSecurity` 작성하지 않았을시,  `@SpringBootApplication(exclude = {SecurityAutoConfiguration.class})` 해주는것으로 스프링 시큐리티 비활성화가 가능함.
- 이는 하이버네이트에서 스프링 시큐리티 자동설정을 잡아주는것을 제외시켜놓는것으로, `@EnableWebSecurity`하였을시엔 유효하지 않다. 
- 보통은 `@EnableWebSecurity` 및 `@Configuration`으로 스프링 시큐리티 설정값을 잡아주므로, 위의 방법은 해당 작업 전에 임시적으로 사용할 수 있다.

# 기본 로그인 비활성화

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        /*스프링 시큐리티 기본 로그인 해제*/
        http.httpBasic().disable();
    }
}
```

- 스프링 시큐리티의 기본 제공 로그인은 서비스에 맞게 커스터마이징하기 번거롭고 난감할 수 있어, 보통 기본적으로 비활성화 하고 들어간다.
- 설정을 작성할 경우, 해당 방법으로 스프링 기본 로그인을 간단히 비활성화 할 수 있음.

# CSRF 비활성화

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        /*스프링 시큐리티 csrf 해제*/
        //REST API의 경우 stateless하므로 CSRF를 비활성화하여도 괜찮다.
        http.csrf().disable();
    }
}
```

- CSRF는 요청을 브라우저로부터 받을때, 활성화를 권고하고 있음.
- 서버가 REST API로 운영되고, JWT 등의 토큰으로 인증처리를 할 경우 CSRF에서 안전하다고 볼 수 있기 때문에 비활성화하여도 무방하다.

# 참고

- https://jjeong.tistory.com/1491
- https://velog.io/@woohobi/Spring-security-csrf%EB%9E%80 - CSRF 비활성화, CSRF는 무엇인지, REST API에서 CRSF를 왜 비활성화 하여도 괜찮은지
- https://wave1994.tistory.com/150 - CSRF 활성화에 대해