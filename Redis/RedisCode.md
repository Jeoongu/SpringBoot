# Redis Code

### Redis 사용에 필요한 2가지 요소
1. **Redis-Server**
2. **Redis-Cli**

- 윈도우에서는 Redis-server를 직접 실행하거나 서비스에 등록해서 서버를 가동시키고, 이 서버에 데이터를 저장하고 사용하는 것은 Redis-cli에서 한다.

### implementation
```java
    // redis
    implementation 'org.springframework.boot:spring-boot-starter-data-redis'
    // :2.3.1.RELEASE'
```
- 상황에 맞춰 버전을 사용한다.

### application.yml
```java
spring:
    data:
        redis:
            port: 6379
            host: 127.0.0.1
            password: 1234
```
- port는 기본적으로 6379가 기본이다. 변경도 가능하다.
- host도 127.0.0.1이 기본이지만 변경이 가능하다.
- passWord도 설정이 가능하다.

### RedisConfig
```java
@RequiredArgsConstructor
@Configuration
@EnableRedisRepositories
public class RedisConfig {
    @Value("${spring.data.redis.host}")
    private String host;

    @Value("${spring.data.redis.port}")
    private int port;

    @Value("${spring.data.redis.password}")
    private String password;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() { //lettuce
        RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration();
        redisStandaloneConfiguration.setHostName(host);
        redisStandaloneConfiguration.setPort(port);
        redisStandaloneConfiguration.setPassword(password);
        LettuceClientConfiguration clientConfig = LettuceClientConfiguration.builder().build();

        return new LettuceConnectionFactory(redisStandaloneConfiguration, clientConfig);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() { //redis-cli 사용을 위한 설정
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
```
- Redis 연결을 위해 RedisConnectionFactory를 사용한다.
- 여기에 RedisStandaloneConfiguration 객체를 생성하여 setHostName, setPort, setPassWord로 connection에 대한 정보를 입력해준다.
- key와 value를 입력하고 사용하기 위해서 하나의 Template를 생성한다. 
- 앞에 들어가는 String이 key이고 뒤에 들어가는 Object가 value이며 setConnectionFactory를 진행하여 조건에 맞추어 연결이 된다.

### RedisController & Service
```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class RedisService {
    private final RedisTemplate<String, String> redisTemplate; // 내가 만든 config의 템플릿을 가져와서 사용한다. 많은 경우 <String, String> 으로 사용한다.
    public void setValues(String key, String value) {
        redisTemplate.opsForValue().set(key, value);
    }
    @Transactional
    public void setValuesWithTimeout(String key, String value, long timeout) { // 만료 시간을 설정해서 자동 삭제 가능
        redisTemplate.opsForValue().set(key, value, timeout, TimeUnit.MILLISECONDS);
    }
    public String getValues(String key) {
        return redisTemplate.opsForValue().get(key);
    }
    public void deleteValues(String key) {
        redisTemplate.delete(key);
    }
}
```
- 기본적으로 redisTemplate의 `opsFor~` 메서드를 사용한다.