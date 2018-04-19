# Webjar

- 프론트엔드 라이브러리를 jar로 패키징함.
- maven으로 프론트엔드 라이브러리를 추가할 수 있다.
- [WebJars](http://www.webjars.org/)에서 dependency를 찾을 수 있다.
```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.3.1-1</version>
</dependency>
```
- 모든 리소스는 ```/webjars/**``` 경로로 제공된다.
```html
<script src="webjars/jquery/3.3.1-1/jquery.min.js"></script>
```
- jar로 패키징 할거면 ```src/main/webapp```을 더 이상 사용하지 말자.
    - war로 패키징할 때만 사용할 수 있다.
    - jar로 패키징하면 무시 된다.
- ```webjars-locator-core``` 의존성을 추가하면 버전 명시 없이 라이브러리를 추가할 수 있다.
- Spring에서 버전을 관리해주고 있기 때문에 Spring Boot 사용시 버전을 명시하지 않아도 된다.
```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator-core</artifactId>
</dependency>
```
```html
<script src="webjars/jquery/jquery.min.js"></script>
```
- JBoss를 사용한다면 ```webjars-locator-core``` 대신 ```webjars-locator-jboss-vfs```를 추가해야 된다.

## 참고
- https://youtu.be/-jaRc_78b4I
- https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-static-content