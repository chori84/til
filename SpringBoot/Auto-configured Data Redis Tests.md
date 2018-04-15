# Auto-configured Data Redis Tests

- ```@DataRedisTest```를 사용한다.
- ```@RedisHash```가 붙은 클래스를 전부 스캔한다.
- ```@Component```는 bean으로 등록 안된다.
- repository를 만들어 주기 때문에 주입 할 수 있다.
```java
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.data.redis.DataRedisTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@DataRedisTest
public class ExampleDataRedisTests {

	@Autowired
	private YourRepository repository;

	//
}
```
- ```@DataRedisTest```의 auto-configuration 리스트는 다음과 같다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration```

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-redis-test
