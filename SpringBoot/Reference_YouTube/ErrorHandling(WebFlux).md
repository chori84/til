# Error Handling

- Spring Boot는 sensible한 방법으로 모든 에러를 처리하는 ```WebExceptionHandler```를 제공한다.
- 이 handler는 WebFlux가 제공하는 handler들의 바로 직전에 위치해 있다, 가장 마지막 위치라고 생각하면 된다.
    - 다른 handler에 다 매핑해 보고 마지막에 매핑한다.
    - Spring MVC의 Error Handling과 대칭되는 WebFlux용 Error Handling
- machine client 한테는 JSON으로 응답을 보내고, 브라우저 client 한테는 "whitelabel" 이라는
에러 정보를 담고 있는 HTML을 보여준다.
- HTML 템플릿을 바꾸고 싶으면 아래를 참조하면 된다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-webflux-error-handling-custom-error-pages
- 커스터마이징을 하기 위해서는 ```ErrorAttributea``` 타입의 bean을 추가할 수 있다.
- 에러를 처리하는 행위(machine client 한테는 JSON으로 응답을 보내고, 브라우저 client 한테는 "whitelabel" 이라는
에러 정보를 담고 있는 HTML을 보여준다)를 바꾸고 싶으면 ```ErrorWebExceptionHalder``` 구현하면 된다.


## 참고
https://youtu.be/xfPcoV_-uuE