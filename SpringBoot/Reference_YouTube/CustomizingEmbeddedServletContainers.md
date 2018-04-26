# Customizing Embedded Servlet Containers
- 일반적으로 servlet container 셋팅은 Spring의 ```Environment``` 프로퍼티를 사용하여 설정할 수 있다.
- 보통 ```application.properties``` 파일에 프로퍼티를 정의할 수 있다.
- 일반적인 서버 설정은 다음과 같다.
    - 네트워크 셋팅 : 리스닝할 포트(```server.port```), ```server.address```에 interface address를 바인드, 등등
    - 세션 셋팅 : 세션이 지속적인지 여부(```server.servlet.session.persistence```),
    세션 타임아웃(```server.servlet.session.timeout```), 세션 데이터 저장 위치(```server.servlet.session.store-dir```),
    세션 쿠키 설정(```server.servlet.session.cookie.*```)
    - 에러 관리 : 에러 페이지의 위치(```server.error.path```), 등등
    - SSL
    - HTTP 압축해서 보낼지 여부
- Spring Boot는 일반적인 셋팅을 노출하기 위해서 가능한 많이 시도한다. 하지만 항상 가능한건 아니다.
- 이런 경우, 전용 네임스페이스는 서버별 커스터마이징을 제공한다. (```server.tomcat```, ```server.undertow```를 보자)
- 예를 들어, access logs는 embedded servlet container의 특정 기능으로 설정 될 수 있다.

### 프로그래밍 방식 커스터마이징
- embedded servlet container에 프로그래밍 방식으로 설정하려면 ```WebServerFactoryCustomizer``` 인터페이스를
구현해서 Spring bean으로 등록할 수 있다.
- ```WebServerFactoryCustomizer```는 수 많은 커스터마이징 setter method를 포함하는 ```ConfigurableServletWebServerFactory```에
접근을 제공한다.
- Tomcat, Jetty, Undertow 전용 변형이 있다.

### Customizing ConfigurableServletWebServerFactory Directly
- 만약에 앞서 살펴본 커스터마이징 방법들이 너무 제약적이라고 생각한다면, ```TomcatServletWebServerFactory```,
```JettyServletWebServerFactory```, ```UndertowServletWebServerFactory``` bean을 직접 등록할 수 있다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/customizing-embedded-servlet-container

## 참조
https://www.youtube.com/watch?v=xfPcoV_-uuE
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-customizing-embedded-containers