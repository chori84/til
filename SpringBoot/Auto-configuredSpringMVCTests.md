# Auto-configured Spring MVC Tests

- MVC controller가 기대하는대로 동ㅈ가하는지 보려면, ```@WebMvcTest```를 사용하면 된다.
- ```@WebMvcTest```는 Spring MVC infrastructure를 자동 설정해준다. 스캐닝하는 bean을
```@Controller```, ```@ControllerAdvice```, ```@JsonComponent```, ```Converter```,
```GenericConverter```, ```Filter```, ```WebMvcConfigurer```, ```HandlerMethodArgumentResolver```
로 제한한다.
- 일반적인 ```@Component```는 이 어노테이션으로 스캐닝 해주지 않는다.
- 만약에 추가적인 Jackson ```Module``` 같은 컴포넌트를 등록할 필요가 있으면,
```@Import```를 사용해서 테스트에 추가적인 설정 클래스를 import 할 수 있다.
- 보통 ```@WebMvcTest```는 controller 하나를 테스트하는데 주로 사용하고, ```@MockBean```이랑 사용해야 한다. 
- ```@WebMvcTest```는 ```MockMvc```를 자동으로 설정해준다.
Mock MVC는 전체 HTTP 서버를 실행할 필요 없이 MVC controller를 테스트하는 강력한 기능을 제공한다.
- ```@AutoConfigureMockMvc```을 달아서 non-```WebMvcTest``` (```@SpringBootTest```와 같은)에서
```MockMvc```를 자동 구성 할 수도 있습니다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/yJ_2eHBQW40
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-mvc-tests

