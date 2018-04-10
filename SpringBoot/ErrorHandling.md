# Error Handling

- 기본적으로 Spring Boot는 모든 에러를 합리적인 방법(sensible way)으로 ```/error```에 매핑하고
서블릿 컨테이너에 "global" 에러 페이지로 등록되 있다.
- machine client에겐 HTTP status나 exception message 같은 에러의 자세한 내용은 JSON 응답으로 제공된다.
- 브라우저 client에겐 HTML 형식으로 "whitelabel" 에러 view를 제공해준다.
- 기본 행동을 바꾸려면 ```ErrorController```를 구현한 bean을 등록하거나, ```ErrorAttributes``` 타입의 bean을 등록한다.
- ```ErrorController```를 커스텀하기 위해 ```BasicErrorController```는 base class로 사용할 수 있다.
    - 다른 content type의 error handler를 추가하고 싶을 때 유용하다. (기본적으로는 ```text/html```만 지원하고 그 밖에 다른건 fallback 한다.)
    - ```BasicErrorController```를 상속 받고 ```produces``` attribute를 가진 ```@RequestMapping```이 붙은 public 메소드를 추가하고
     구현한 클래스를 빈으로 등해서 쓴다.

## 참고
https://youtu.be/V-gBDmR6gZk
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-error-handling