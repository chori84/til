# Auto-configured Data LDAP Tests

- ```@DataLdapTest```을 사용한다.
- in-memory embedded LDAP을 사용한다.
- ```LDAPTemplate```을 만들어 주기 때문에 주입 할 수 있다.
```java
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.data.ldap.DataLdapTest;
import org.springframework.ldap.core.LdapTemplate;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@DataLdapTest
public class ExampleDataLdapTests {

	@Autowired
	private LdapTemplate ldapTemplate;

	//
}
```
- ```@Component```는 bean으로 등록 안된다.
- in-memory embedded LDAP을 끌 수 있다.
```java
import org.junit.runner.RunWith;
import org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration;
import org.springframework.boot.test.autoconfigure.data.ldap.DataLdapTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@DataLdapTest(excludeAutoConfiguration = EmbeddedLdapAutoConfiguration.class)
public class ExampleDataLdapNonEmbeddedTests {

}
```
- ```@DataLdapTest```의 auto-configuration 리스트는 다음과 같다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.ldap.LdapDataAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration```

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-ldap-test
