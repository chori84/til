# Path Matching and Content Negotiation

- Spring MVC는 Controller 메소드에 붙은 ```@GetMapping``` 같은 어노테이션을 이용하여 request path를 보고
HTTP request를 handler로 매핑할 수 있다.
- Spring Boot는 suffix 패턴을 기본으로 사용하지 않기 때문에 ```GET /projects/spring-boot.json```와 같은 요청이
```@GetMapping("/projects/spring-boot")```에 매핑되지 않는다.
- Http Header의 ```Accept```를 이용한다.
    - Accept = application/json
    - Accept = application/xml
- query 파라미터를 이용해서 요청하는 방법도 지원한다.
    - ```application.properties```에 property를 추가한다.
    - query 파라미터에 format을 넣는다.
        - ```GET /projects/spring-boot?format=json``` 요청이 ```@GetMapping("/projects/spring-boot")```에 매핑된다. 
```properties
spring.mvc.contentnegotiation.favor-parameter=true
```
- suffix 패턴이 좋지 않음에도 꼭 사용하고 싶다면 application.properties에 property를 추가하여 사용한다.
    - ```GET /projects/spring-boot.json``` 요청이 ```@GetMapping("/projects/spring-boot")```에 매핑된다.
```properties
spring.mvc.contentnegotiation.favor-path-extension=true
spring.mvc.pathmatch.use-registered-suffix-pattern=true
```

## 참고
- https://youtu.be/-jaRc_78b4I
- https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-pathmatch