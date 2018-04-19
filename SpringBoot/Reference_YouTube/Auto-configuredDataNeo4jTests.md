# Auto-configured Data Neo4j Tests

- in-memory embedded Neo4j를 사용한다.
- ```@NodeEntity``` 클래스를 스캔한다.
- ```@Component```는 bean으로 등록 안된다.
- repository를 만들어주기 때문에 주입해서 쓸 수 있다.
```java
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.data.neo4j.DataNeo4jTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@DataNeo4jTest
public class ExampleDataNeo4jTests {

	@Autowired
	private YourRepository repository;

	//
}
```
- 기본적으로 트랜젝션이 roll back 되고, 트랜젝션을 끄고 싶으면 다음과 같이 한다.
```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.autoconfigure.data.neo4j.DataNeo4jTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

@RunWith(SpringRunner.class)
@DataNeo4jTest
@Transactional(propagation = Propagation.NOT_SUPPORTED)
public class ExampleNonTransactionalTests {

}
```
- ```@DataNeo4jTest```의 auto-configuration 리스트는 다음과 같다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration```

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-neo4j-test
