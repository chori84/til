# Testing Spring Boot Application

- Spring Boot 애플리케이션은 Spring ```ApplicationContext```이다.
- Spring Boot의 External Properties, 로킹, 다른 기능들은 ```SpringApplication```을 사용하 context에 설치 된다.
- Spring Boot가 제공하는 ```@SpringBootTest```라는 어노테이션은 Spring Boot features 가 필요한 경우에
```spring-test```의 ```@ContextConfiguration```을 대체할 수 있다.
- 어노테이션은 ```SpringApplication```을 이용해서 테스트에 ```ApplicationContext```를 만들어준다.
- ```@SpringBootTest```의 ```webEnviroment``` 어트리뷰트를 써서 테스트를 개선할 수 있다.
    - ```Mock```: ```WebApplicationContext```를 로딩하고 mock servlet 환경을 제공한다.
    이 어노테이션을 사용하면 embedded servlet container들이 시작되지 않는다. servlet API가 classpath에 없으면
    웹이 아닌 ```ApplicationContext```를 만든다. ```MockMvc```기반의 테스트를 하기 위해서
    ```@ApplicationConfigurerMockMvc```를 사용할 수 있다.
    - ```RANDOM_PORT```: ```ServletWebServerApplicationContext```를 만들고 실제 servlet 환경을 제공한다.
    embedded servlet container가 실행되고, random port에서 listen한다.
    - ```DEFINED_PORT```: ```ServletWebServerApplicationContext```를 만들고 실제 servlet 환경을 제공한다.
    embedded servlet container가 실행되고, 설정한 port에서 listen한다.
    (이 port는 ```application.properties```에서 정의하거나 기본 8080 포트를 사용한다.)
    - ```NONE```: ```SpringApplication```을 사용하여 ```ApplicationContext```를 사용하지만
    servlet 환경과 관련된 어떠한 기능도 제공하지 않는다.
- 만약 테스트가 ```@Transactional```이면, 모든 transaction은 기본적으로 roll back 된다.
하지만 ```RANDOM_PORT```나 ```DEFINED_PORT```를 사용하는 경우, 클라이언트와 서버가 다른 thread에서 동작하기 때문에 roll back 되지 않는다.
- ```@SpringBootTest```를 추가해서 테스트를 위한 여러 어노테이션들도 제공된다.
- ```@RunWith(SpringRunner.class)```를 붙이는 것을 잊지마라!! 다른 어노테이션들을 모두 무시한다.
- ```@SpringBootTest```의 기본 값은 ```Mock```이다.

## 예제 소스
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/pnkBfsIqdK4
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications
