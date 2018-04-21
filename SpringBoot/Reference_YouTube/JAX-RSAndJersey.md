# JAX-RS and Jersey

- REST endpoint를 만들 때 JAX-RS 프로그래밍 모델이 더 마음에 든다면, Spring MVC 대신에 사용 가능한 구현체를 사용할 수 있습니다.
- 사용 가능한 구현체로는 Jersey 1.x와 Apache CXF가 아주 잘 동작하는데, 이들이 제공하는 ```Servlet``` 또는 ```Filter```를
application context에 ```@Bean```으로 등록하면 된다.
- Jersey 2.x는 Spring를 네이티브하게 지원하는 기능이 들어 있다. 그래서 Spring Boot에서 auto-configuration을 제공한다.
- Jersey 2.x를 사용하고 싶으면, ```spring-boot-starter-jersey```를 의존성에 추가하고
endpoint를 등록하는 ```ResourceConfig``` 타입의 ```@Bean```을 하나 만들면 된다.


## 참고
https://youtu.be/xfPcoV_-uuE