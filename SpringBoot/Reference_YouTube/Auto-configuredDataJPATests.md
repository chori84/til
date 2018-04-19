# Auto-configured Data JPA Tests

- MVC가 아닌 JPA를 테스트하고 싶으면 ```@DataJpaTest```를 사용하면 된다.
- 기본적으로 in-memory embedded database를 설정해주고, ```@Entity``` 클래스를 전부 스캔해주고,
Spring Data JPA repository들을 설정해준다.
- 일반적인 ```@Component``` bean 들은 ```ApplicationContext```에 로딩이 안된다.
- 기본적으로 JAP 테스트는 transactional하고 각 테스트가 끝날 때 마다 roll back을 한다.
    - https://docs.spring.io/spring/docs/5.0.5.RELEASE/spring-framework-reference/testing.html#testcontext-tx-enabling-transactions
    - 원래 Spring Framework에서 제공해주는 기능이다.
    - 테스트에 ```@Transactional```이라고 붙이면 테스트 안에 모든 테스트는 roll back 된다.
- roll back을 하고 싶지 않은 경우에는 ```@Transactional(propagation = Propagation.NOT_SUPPORTED)```로 설정하면 된다.
    - 트랜젝션을 안쓰겠다는 뜻
- Data JPA 테스트는 표준 JPA ```EntityManager```를 대신해서 테스트를 위해 특별히 설계된
```TestEntityManager```를 주입 받을 수 있다.
- ```@DataJpaTest```가 안붙어 있는 곳에서 ```TestEntityManager```를 쓰고 싶으면
```@AutoConfigureTestEntityManager``` 어노테이션을 사용해서 주입 받을 수 있다.
- ```JdbcTemplate```도 ```@DataJapTest```를 사용해서 쓸 수 있다.
- in-memory embedded database는 테스트에서 잘 동작하고, 빠르고 추가로 설치할 것도 없다.
- 하지만 진짜 데이터베이스를 테스트하고 싶은 경우엔 ```@AutoConfigureTestDatabase```를 사용해서 데이터베이스를 replace하지 않도록 기능을 끌 수 있다.
```java
@RunWith(SpringRunner.class)
@DataJpaTest
@AutoConfigureTestDatabase(replace=Replace.NONE)
public class ExampleRepositoryTests {

	// ...

}
```
- ```@DataJpaTest```가 제공해주는 auto-configuration 목록
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.jdbc.TestDatabaseAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.orm.jpa.TestEntityManagerAutoConfiguration```

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-jpa-test
