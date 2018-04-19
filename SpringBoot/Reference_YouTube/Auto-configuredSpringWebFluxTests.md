# Auto-configured Spring WebFlux Tests

- Auto-configured Spring MVC Tests와 설명이 비슷하고 사용하는 어노테이션만 다르다.
    - ```@WebMvcTest``` -> ```@WebFluxTest```
    - ```MockMvc``` 대신 ```WebTestClient```를 자동으로 등록
    - ```@AutoConfigureMockMvc``` -> ```@AutoConfigureWebTestClient```
    - Spring servlet 기반의 MVC를 사용하면서 쓸 수 있는건 아니다. WebFlux를 쓸 때만 가능하다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-webflux-tests
