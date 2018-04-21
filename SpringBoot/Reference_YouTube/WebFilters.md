# Web Filters

- Spring WebFlux는 ```WebFilter``` 인터페이스를 제공하는데, 이것은 HTTP 요청, 응답을 처리하는 과정에 필요한 filter를 만들 수 있다.
- ```WebFilter``` 빈이 application context에 들어 있으면 자동으로 요청, 응답 처리에 적용된다.
- filter가 다 적용되는데 이 filter 간에 순서를 주고 싶으면 filter 구현체에 ```Ordered``` 인터페이스를 구현하거나
```@Order``` 어노테이션을 붙이면 된다.
- Spring Boot auto-configuration은 웹 필터를 설정할 수 있다.
- 그렇게 하면 순서는 다음표와 같다.

|Web Filter|Order|
|----------|-----|
|MetricsWebFilter|Ordered.HIGHEST_PRECEDENCE + 1|
|WebFilterChainProxy(Spring Security)|-100|
|HttpTraceWebFilter|Ordered.LOWEST_PRECEDENCE - 10|

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-webfilter

## 참고
https://youtu.be/xfPcoV_-uuE