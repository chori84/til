# The ServletWebServerApplicationContext
- Spring Boot의 밑 단에는 embedded servlet container를 지원하기 위해서 여러가지 타입의 ```ApplicationContext```를 사용한다.
- ```ServletWebServerApplicationContext```는 단일 ```ServletWebServerFactory``` bean을 검색해서 스스로 bootstrap하
```WebApplicationContext```의 특별한 타입이다.
- 보통은 ```TomcatServletWebServerFactory```, ```JettyServletWebServerFactory```, ```UndertowServletWebServerFactory```가
자동으로 설정 되있다.
- 보통 이 구현체들에 대해서 알 필욘 없다.
- 대부분의 애플리케이션들은 자동으로 설정 되고, 적당한 ```ApplicationContext```와 ```ServletWebServerFactory```가 생성 된다.

## 참조
https://www.youtube.com/watch?v=xfPcoV_-uuE
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-embedded-container-application-context
