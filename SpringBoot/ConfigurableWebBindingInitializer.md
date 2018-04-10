# ConfigurableWebBindingInitializer
- Spring MVC는 특정한 요청에서 데이터를 바인딩할 때 사용하기 위해 ```WebBindingInitializer```를 사용해서 ```WebDataBinder```를 만든다.
- ```@Bean```을 사용하여 ```ConfigurableWebBIndingInitializer```를 생성하면 Spring Boot가 자동으로 Spring MVC에서 사용한다.

## 참고
https://youtu.be/V-gBDmR6gZk
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-web-binding-initializer