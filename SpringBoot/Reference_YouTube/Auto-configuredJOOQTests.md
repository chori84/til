# Auto-configured jOOQ Tests

- jOOQ도 Jdbc랑 비슷하게 쓸 수 있지만 jOOQ는 데이터베이스 스키마인 Java 기반 스키마에 아주 많이 의존하고 있다.
- in-memory 데이터베이스로 바꾸고 싶으면, ```@AutoConfigureTestDatabase``` 써서 override 해야 된다.
- ```@JooqTest``` 어노테이션은 ```DSLContext```를 만들어 주어 주입 받을 수 있다.
```java
import org.jooq.DSLContext;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.autoconfigure.jooq.JooqTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@JooqTest
public class ExampleJooqTests {

	@Autowired
	private DSLContext dslContext;
}
```
- transactional하고 기본적으로 테스트가 끝날 때 roll back된다.
- transcation을 원하지 않으면 끌 수 있다.
- ```@JooqTest```의 auto-configuration 리스트는 다음과 같다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration```

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-jooq-test
