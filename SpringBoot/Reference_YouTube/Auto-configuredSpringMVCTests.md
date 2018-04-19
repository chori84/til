# Auto-configured Spring MVC Tests

- MVC controller가 제대로 동작하는지 확인하려면, ```@WebMvcTest```를 사용하면 된다.
- ```@WebMvcTest```는 Spring MVC infrastructure를 자동 설정해주고, 스캐닝하는 bean을
```@Controller```, ```@ControllerAdvice```, ```@JsonComponent```, ```Converter```,
```GenericConverter```, ```Filter```, ```WebMvcConfigurer```, ```HandlerMethodArgumentResolver```
로 제한한다.
- 일반적인 ```@Component```는 이 어노테이션으로 스캐닝 해주지 않는다.
- 만약에 추가적인 Jackson ```Module``` 같은 컴포넌트를 등록할 필요가 있으면,
```@Import```를 사용해서 테스트에 추가적인 설정 클래스를 import 할 수 있다.
- 보통 ```@WebMvcTest```는 controller 하나를 테스트하는데 주로 사용하고, ```@MockBean```이랑 사용해야 한다. 
- ```@WebMvcTest```는 ```MockMvc```를 자동으로 설정해준다.
Mock MVC는 전체 HTTP 서버를 실행할 필요 없이 MVC controller를 테스트하는 강력한 기능을 제공한다.
- ```@AutoConfigureMockMvc```을 달아서 ```WebMvcTest``` 아닌 곳(```@SpringBootTest```와 같은)에서
```MockMvc```를 자동 구성 할 수도 있습니다.
- auto-configuration을 수정하고 싶으면, ```@AutoConfigureMockMvc```를 사용하면 된다.
- HtmlUnit이나 Selenium을 쓴다면, auto-configuration은 HTML Unit ```WebClient``` bean과
또는 ```WebDriver``` bean을 제공한다.
- 기본적으로 Spring Boot는 드라이버가 각각의 테스트마다 새로 만들어지고, 새로운 객체가 주입되도록
```WebDriver``` bean을 특별한 "scope"에 넣는다.
- ```WebDriver```를 싱글튼으로 사용하고 싶으면 ```WebDriver``` ```@Bean``` definition에
```@Scope("singleton")```을 주면 된다.
- ```@WebMvcTest```에서 auto-configuration 목록은 다음에서 볼 수 있다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.web.servlet.MockMvcAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.web.servlet.MockMvcSecurityAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.web.servlet.MockMvcWebClientAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.web.servlet.MockMvcWebDriverAutoConfiguration```

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/yJ_2eHBQW40
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-mvc-tests

