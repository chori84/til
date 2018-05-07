# Security
- Spring Security가 classpath에 있으면 웹 애플리케이션은 기본적으 보안 처리가 된다.
- Spring Boot는 ```httpBasic```을 사용할지 ```formLogin```을 사용할지 판단하기 위해서,
Spring Security의 content-negotiation strategy를 사용한다.
- 웹 애플리케이션에 메소드 레벨의 시큐리티를 추가하려면, 원하는 설정과 같이 ```@EnableGlobalMethodSecurity```를 추가해서 사용할 수 있다.
- 추가적인 가이드는 [Spring Security Reference Guide](https://docs.spring.io/spring-security/site/docs/5.0.4.RELEASE/reference/htmlsingle/#jc-method)를 보면 된다.
- 기본 ```AuthenticationManager```는 user를 하나 가지고 있다.
- user 이름은 ```user``` password는 자동으로 생성되어 애플리케이션이 시작 될 때 다음과 같이 INFO 레벨로 프린트 해준다.
```
Using generated security password: 78fa095d-3f4c-48b1-ad50-e24c31d5cf35
```

- 로깅 설정을 조절할 때, ```org.springframework.boot.autoconfigure.security```를 ```INFO``` 레벨로 설정해야 한다.
- 안그러면 기본 password가 프린트 되지 않는다.

- ```spring.security.user.name```과 ```spring.security.user.password```를 사용해서 user 이름과 password를 변경할 수 있다.
- 웹 애플리케이션 시큐리티를 활성함으로써 얻을 수 있는 기본적인 기능은 다음과 같다.
    - in-memory store를 쓰고 single user와 자동생성 된 password가 들어 있는 ```UserDetailService```
    (또는 WebFlux 애플리케이션인 경우 ```ReactiveUserDetailService```) bean이 등록된다.
    ([```SecurityProperties.User```](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/api/org/springframework/boot/autoconfigure/security/SecurityProperties.User.html))
    를 보세요.
    - 애플리케이션 전반(actuator가 classpath에 있는 경우 actuator endpoints를 포함해서)에 걸친 Form-based login 또는 HTTP Basic security(Content-Type에 의존하는)
    - 인증 관련 이벤트를 publishing 하는 ```DefaultAuthenticationEventPublisher```
- 어떤 bean을 등록해서 다른 종류의 ```AuthenticationEventPublisher```를 제공할 수 있다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-security

## 참고
https://www.youtube.com/watch?v=1PeTUImu8DA
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-security
https://docs.spring.io/spring-security/site/docs/5.0.4.RELEASE/api/org/springframework/security/authentication/event/package-summary.html