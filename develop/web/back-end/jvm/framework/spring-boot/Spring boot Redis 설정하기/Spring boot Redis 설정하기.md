# 개요

- Spring boot에서 Redis 연결 설정하는법을 알아본다.
- Redis를 사용하는 방법에는 다른 NoSQL인 Elastic Search와 마찬가지로 Redis측에서 자체 제공하는 라이브러리(ES의 RestHighLevelClient에 해당되는 RedisTemplate)를 사용하는 방법과, Spring측에서 이를 추상화한 Spring Data Redis를 사용하는 방법이 있는데, 일단 무엇을 사용하던 연결방식은 동일하다.
- 연결 방식에는 Jedis와 Lettuce가 있는데, Jedis는 과거에 사용되었으나 많은 문제들로 인해 deprecated 되어 현재는 lettuce를 이용해 연결한다.

# yml

```yaml
spring:
  redis:
    host: ${redis-host}
    port: ${redis-port}
    password: ${redis-password}
```

# Configuration

```java
@Configuration
@EnableRedisRepositories
public class RedisConfig {
    @Value("${spring.redis.host}")
    private String host;
    @Value("${spring.redis.port}")
    private int port;
    @Value("${spring.redis.password}")
    private String password;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration();
        redisStandaloneConfiguration.setHostName(host);
        redisStandaloneConfiguration.setPort(port);
        redisStandaloneConfiguration.setPassword(password);
        return new LettuceConnectionFactory(redisStandaloneConfiguration);
    }

    @Bean
    public RedisTemplate<?, ?> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
```

# 참고

- https://sabarada.tistory.com/106
- https://sabarada.tistory.com/105
- https://oingdaddy.tistory.com/239
- https://bcp0109.tistory.com/328