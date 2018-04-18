# Static Content

- ```/static```, ```/public```, ```/resource``` 안에 있는 static component들은 ```ResourceWebHandler```로 제공해주고,
커스터마이징하고 싶으면 ```WebFluxConfigurer```를 추가하고, ```addResourceHandler``` 메소드를 오버라이딩 하면 된다.
- 기본적으로 리소스들은 모든 요청이 다 ```/**```에 매핑되있다. 하지만 ```spring.webflux.static-path-pattern```으로 세팅할 수 있다.
- 리소스를 요청하는 기본 url을 ```/resources/**```로 변경하고 싶으면 다음과 같이 하면 된다.
```spring.webflux.static-path-pattern=/resources/**```
- static resource의 위치도 ```spring.resources.static-locations```로 바꿀 수 있다.
- Spring WebFlux 애플리케이션은 Servlet API에 의존하지 않는다. 그래서 그들은 war 파일로 배포할 수 없다.
그러니 ```src/main/webapp``` 디렉토리를 사용하지 말자.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-webflux

## 참고
https://youtu.be/JuQiY0yqwCI
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-webflux-static-content