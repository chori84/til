# Testing with a running server

- 전체 실행 서버를 시작하는 경우, 랜덤 port를 사용해야 한다.
- ```@SpringBootTest(webEnvironment=WebEnvironment.RANDOM_PORT)```를 사용하면 진싸 servlet container를 실행한다.
- ```@LocalServerPort```를 사용해서 실제 port를 가져올 수 있다.
- REST 호출을 사용하고 싶으면 ```@Autowire```해서 ```WebTestClient```를 추가해서,
실제로 동작하는 서버에 요청을 보내고 응답을 확인하는 코드를 작성할 수 있다.
- Spring Boot는 또한 ```TestRestTemplate``` 기능을 제공한다.
- ```@WebMvcTest```, ```@SpringBootTest```, ```@WebFluxTest```
    - ```@WebFluxTest```랑 ```@WebMvcTest```는 단 하나의 controller만 테스트하는 용도
    - ```@SpringBootTest``` classpath에 따라 알아서 동작한다. (WebFlux가 있으면 WebFlux 기반으로, MVC가 있으면 MVC 기반으로)
- ```@AutoConfigureMockMVc```는 MockMvc를 만들어주는데 MVC 기반에서만 만들어진다.
- ```@AutoConfigureWebTestClient```는 WebTestClient를 만들어주는데 WebFlux 기반에서만 만들어진다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/yJ_2eHBQW40
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-with-running-server
