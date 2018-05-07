# MVC Security
```
첫 문장이 방송했을 때와 현재 레퍼런스 내용이 다름. (방송 이후 변경 된 듯 함)
레퍼런스 기준으로 내용 작성함.
```
- 기본 Security 설정은 ```SecurityAutoConfiguration```과 ```UserDetailsServiceAutoConfiguration```에서 구현하고 있다.
- ```SecurityAutoConfiguration```은 웹 security를 위한 ```SpringBootWebSecurityConfiguration```를 import 하고,
```UserDetailsServiceAutoConfiguration```는 웹 애플리케이션이 아닌 곳에서도 사용할 수 있는 authentication 설정한다. 
```text
여기까지 다른 내용
```

- 기본 웹 애플리케이션 security 설정을 완전이 끄려면 ```WebSecurityConfigurerAdapter``` 타입의 bean을 추가할 수 있다.
(그렇게 하더라도, authentication manager 설정과 Actuator의 security 설정을 꺼지지 않는다.)
- authentication manager 설정까지 끄고 싶으면 ```UserDetailsService```, ```AuthenticationProvider``` 또는
```AuthenticationManager``` 타입의 bean을 추가할 수 있다.
- [Spring Boot sample](https://github.com/spring-projects/spring-boot/tree/v2.0.1.RELEASE/spring-boot-samples/)에는
일반적인 사용 케이스들로 시작할 수 있는 몇가지 보안 애플리케이션이 있다.
- ```WebSecurityConfigurerAdapter``` 커스터마이징을 추가해서 Access rule을 변경할 수 있다.
- Spring Boot는 actuator endpoint나 static resources의 access rule를 수정할 때 사용할 수 있는 편리한 메소드를 제공한다.
- ```EndPointRequest```를 사용해서 ```management.endpoints.web.base-path``` 프로퍼티 기반의 ```RequestMatcher```를 만들 수 있다.
- ```PathRequest```를 사용해서 일반적으로 사용되는 위치의 resource들을 위한 ```RequestMatcher```를 만들 수 있다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-security

## 참고
https://www.youtube.com/watch?v=1PeTUImu8DA
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-security-mvc

## 참고 파일
StaticResourceLocation.java