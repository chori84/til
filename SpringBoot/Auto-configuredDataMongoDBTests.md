# Auto-configured Data MongoDB Tests

- ```@DataMongoTest```를 붙이면 in-memory embedded MongoDB를 만들어 주고, ```MongoTemplate```을 설정해 주고,
```@Document``` 클래스를 스캔해 주고, Spring Data MongoDB repository를 만들어 준다.
- ```@Component```들은 bean으로 등록 안된다. 즉, repository만 생성 된다.
- in-memory embedded MongoDB는 테스트가 잘 되고, 빠르고 설치할 필요 없다
- 하지만 진짜 MongoDB를 사용하고 싶으면 override 해야 한다.

```java
import org.junit.runner.RunWith;
import org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration;
import org.springframework.boot.test.autoconfigure.data.mongo.DataMongoTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@DataMongoTest(excludeAutoConfiguration = EmbeddedMongoAutoConfiguration.class)
public class ExampleDataMongoNonEmbeddedTests {

}
```
- ```@DataMongoTest```를 사용한다. (다른 것들 처럼```@AutoConfigureTestDatabase```를 사용하지 않음.)
- ```@DataMongoTest```의 auto-configuration 리스트는 다음과 같다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration```

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-mongo-test
