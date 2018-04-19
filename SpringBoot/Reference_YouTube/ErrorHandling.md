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

- ```@ControllerAdvice``` 어노테이션을 가진 클래스를 만들어서 특정한 컨트롤러와 exception 타입의 반환용으로 JSON document를 커스터 마이징 할 수 있다.
    - @ControllerAdvice(basePackageClasses = AcmeController.class)
    - @ExceptionHandler(YourException.class)
        - AcmeController가 있는 패키지에서 YourException이 발생하면 ExceptionHandler가 붙은 메소드를 실행한다.
        - 메소드에서 HttpServletRequest와 Throwable을 파라미터로 받을 수 있다.

## Custom Error Pages
- 상태 코드에 따라 특정한 에러 페이지를 보여주고 싶으면 ```/error``` 폴더에 상태 코드 값을 이름으로 static HTML이나 template을 만들어 넣으면 된다.
- 파일 이름을 ```404``` 같은 정확한 상태 코드로 사용할 수도 있고, ```5xx``` 같은 시리즈 마스크(5로 시작하는 모든 에러를 처리)로 사용할 수도 있다.
    - 404.html, 5xx.ftl
- ```ErrorViewResolver```를 구현한 bean을 등록해서 더 복잡한 매핑을 할 수 있다.
- ```@ExceptionHandler``` 메소드와 ```@ControllerAdvice```를 이용할 수도 있고, 처리 되지 않은 exception들은 ```ErrorController```가 받아서 처리한다.

## Mapping Error Pages outside of Spring MVC
- Spring MVC를 사용하지 않는 어플리케이션에서 직접 ```ErrorPages```를 등록하기 위해서 ```ErrorPageRegisterar``` 인터페이스를 사용할 수 있다.
- 밑에 있는 servlet container와 직접 일하는 것이다.
- Spring MVC ```DispatcherServlet```이 없어도 동작한다.
- ```ErrorPage```의 경로를 처리하기 위한 ```Filter```를 등록해줘야 한다.

## 참고
https://youtu.be/V-gBDmR6gZk
https://youtu.be/tfpvEdq0xdU
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-error-handling