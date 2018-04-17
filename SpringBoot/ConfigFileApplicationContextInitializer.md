# ConfigFileApplicationContextInitializer

- ```ConfigFileApplicationContextInitializer```는 ```ApplicationContextInitializer``` 이다.
- ```application.properties```를 읽어서 테스트 할 수 있다.
- ```@SpringBootTest```에서 제공하는 모든 기능이 필요한게 아니면 이렇게 쓸 수 있다.
```java
@ContextConfiguration(classes = Config.class,
	initializers = ConfigFileApplicationContextInitializer.class)
```
- ```ConfigFileApplicationContextInitializer```는 ```@Value("${…​}")``` injection이 안된다.
- 하는 유일한 일은 ```applicaiton.properties``` 파일을 읽어서 Spring의 ```Enviroment```에 넣어주는 거다.
- ```@Value``` support를 받고 싶으면 ```PropertySourcesPlaceholderConfigurer```를 추가로 구성하거나
```@SpringBootTest```를 사용하여 자동으로 구성해야합니다.
    - ```@SpringBootTest```를 쓰면 되는데 못 쓸 일이 있나?
    - 그럴 경우에 ```application.properties``` 파일을 읽어서 ```Enviroment```에 넣고 싶으면 위와 같이 쓰면 된다.

## 참고
https://youtu.be/Tb0guL8hURs
https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-configfileapplicationcontextinitializer-test-utility
