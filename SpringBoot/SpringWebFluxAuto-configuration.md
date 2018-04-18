# Spring WebFlux Auto-configuration

- Spring Boot는 대부분의 애플리케이션에서 잘 동작할 만한 Spring WebFlux용 auto-configuration을 제공한다.
- auto-configuration이 추가해주는 기능은 다음과 같다.
    - ```HttpMessageReader```와 ```HttpMessageWriter```용 코덱을 설정해준다.
    - static 리소스를 지원한다.
- Spring Boot WebFlux 기능을 유지하면서 추가적인 WebFlux 설정을 하고 싶으면 ```@EnableWebFlux``` 없이
```Configuration```를 애플리케이션에 붙이고 ```WebFluxConfigurer```를 구현하면 된다.
    -  ```@EnableWebFlux```를 붙이면 Spring Boot가 제공하는 WebFlux용 자동설정을 모두 무시하고 하나 하나 다 설정해야 한다.
    
## 참고
https://youtu.be/JuQiY0yqwCI
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-webflux-auto-configuration
