# Detecting Web Application Type

- Spring MVC를 이용할 수 있으면, 일반적인 MVC 기반의 application context가 설정 된다.
- Spring WebFlux만 있으면 WebFlux 기반의 application context를 설정해 준다.
- 둘다 있으면, Spring MVC가 더 우선 순위가 높다. 이때 reactive web application으로 테스트하고 싶으면
```spring.main.web-application-type``` 프로퍼티를 설정하면 된다.

```java
@RunWith(SpringRunner.class)
@SpringBootTest(properties = "spring.main.web-application-type=reactive")
public class MyWebFluxTests { ... }
```

## 참고
https://youtu.be/pnkBfsIqdK4
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-detecting-web-app-type