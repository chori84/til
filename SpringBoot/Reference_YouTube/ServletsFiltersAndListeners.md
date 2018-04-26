# Servlets, Filters, and listeners

- embedded servlet container를 사용할 때, Spring bean을 사용하거나 Servlet component를 스캔해서
Servlet 스펙의 모든 servlet, filter, listener를 등록할 수 있다.

### Registering Servlets, Filters, and Listeners as Spring Beans
- 모든 ```Servlet```, ```Filter```, servlet ```*Listener```들이 Spring bean이면 embedded container에 등록이 된다.
- ```application.properties```에 있는 value를 참조하고 싶을 때 특히 유용할 수 있다.
- 기본적으로, 만약에 context가 single Servlet만 가지고 있으면 ```/```(모든 요청)이 이 Servlet에 매핑된다.
- multiple servlet bean이 있으면, 빈 이름이 path prefix에 사용된다.
- Filter는 ```/*```(모든 요청)에 매핑된다.
- 만약에 convention-based 매핑이 충분히 유연하다고 생각되지 않다면(위의 컨벤션들이 나랑 맞지 않다고 생각되면),
```ServletRegistrationBean```, ```FilterRegistrationBean```, ```ServletListenerRegistrationBean``` 클래스를 사용할 수 있다.
- Spring Boot는 여러 auto-configuration을 제공하는데, 아마 그들 중에 Filter bean을 등록하는 애들도 있을지도 모른다.
- 여기에 몇몇 Filter들이 있고 순서들이 정의 되있다.

|Servlet Filter|Order|
|--------------|-----|
|OrderedCharacterEncodingFilter|Ordered.HIGHEST_PRECEDENCE|
|WebMvcMetricsFilter|Ordered.HIGHEST_PRECEDENCE + 1|
|ErrorPageFilter|Ordered.HIGHEST_PRECEDENCE + 1|
|HttpTraceFilter|Ordered.LOWEST_PRECEDENCE - 10|

- Filter bean은 순서가 없이 놔둬도 괜찮다.
- 특정한 순서가 필요하면, request body를 읽는 Filter의 경우에는 ```Ordered.HIGHEST_PRECEDENCE```를 가지면 안된다.
왜냐면, ```OrderedCharacterEncodingFilter``` 보다 먼저 실행되서 캐릭터 인코딩이 잘 못 될 가능성이 있다.
- Servlet filter가 요청을 감싸면(시큐리티 처리를 한다던가, 로깅한다던가), ```FilterRegistrationBean.REQUEST_WRAPPER_FILTER_MAX_ORDER```
보다 순서가 같거나 작아야 된다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-filter

## 참고
https://youtu.be/xfPcoV_-uuE
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-embedded-container-servlets-filters-listeners