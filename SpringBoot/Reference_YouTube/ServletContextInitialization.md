# Servlet Context Initialization
- 임베디드 서블릿 컨테이너는 Servlet 3.0이상에서 사용하는 ```javax.servlet.ServletContainerInitializer```이나
```org.springframework.web.WebApplicationInitializer```를 실행하지 않는다.
- war 안에서 실행되는 서드파티 라이브러리가 Spring Boot 애플리케이션을 깨뜨리는걸 줄이기 위한 의도적인 설계 결정이다.
- Spring Boot 애플리케이션에서 servlet context initialization을 실행해야 될 경우가 있다면,
```org.springframework.boot.web.ServletContextInitializer``` 인터페이스를 구현한 bean을 등록해야 된다.
- 단일의 ```onStartup``` 메소드는, ```ServletContext```의 액세스를 제공하고
필요하다면 기존 ```WebApplicationInitializer```의 어댑터로 쉽게 사용할 수 있다.
- WebApplicationInitializer
    - xml 기반의 web.xml을 대체하여 xml 없이 자바 기반으로 작성할 수 있게 한다.
    - servlet 같은 것을 등록할 수 있다.

### Scanning for Servlets, Filters, and listeners
- embedded container를 사용할 때는 ```@WebServlet```, ```@WebFilter```, ```@WebListener``` 같은 어노테이션을 쓰면
```@ServletComponentScan``` 어노테이션으로 스캐닝 할 수 있다.
- ```@ServletComponentScan```는 독립형 container(embedded container가 아닌, tomcat 자체를 띄운 것?)에선 효과가 없다.

##
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-filter

## 참고
https://youtu.be/xfPcoV_-uuE
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-embedded-container-context-initializer
