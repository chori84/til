# Auto-configured Tests

- Spring Boot의 애플리케이션을 위해서 auto-configuration 시스템은 잘 동작하지만,
테스트를 하기 위해서는 (auto-configuration을) 너무 많이 하는 경우가 있다.
- 가끔은 애플리케이션을 조각내서 일부분만 가져오고 싶은 경우가 있다.
- 예를들어, Spring MVC의 URL 매핑이 잘 되는지 확인하고 싶을 때가 있다.
그 이하 서비스 타고, DAO 타고 등등 모든 과정을 제외하고 매핑만 테스트하고 싶거나
웹 계층에 상관 없이 JPA entities만 확인하고 싶을 때
- ```spring-boot-test-autocoonfigure``` 모듈이 자동적으로 "slice" 설정할 수 있도록 여러가지 어노테이션을 제공한다.
- 각각의 어노테이션은 비슷하게 동작을 하는데, ```ApplicationContext```를 만들어 주는
```@...Test```라는 어노테이션과,
auto-configuration을 세팅하는데 사용할 수 있는 ```@AutoConfigure...``` 어노테이션을 하나 이상 제공해 준다.
- 각각의 slice는 매우 제한된 auto-configuration 클래스 셋을 로딩한다.
- 만약 그것들 중 하나를 제거하고 싶으면, 대부분의 ```@...Test``` 어노테이션들은
```excludeAutoConfiguration```이라는 애트리뷰트를 제공한다.
- 대신 ```@ImportAutoConfiguration#exclude```를 사용해도 된다.
- ```@AutoConfigure...``` 어노테이션들은 ```@SpringBootTest``` 어노테이션과 같이 사용하는거도 가능하다.
- "slicing"에 관심 없지만 auto-configuration 된 test bean들을 사용하고 싶으면 이 조합을 사용하면 된다.

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests
