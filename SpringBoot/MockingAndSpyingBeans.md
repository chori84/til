# Mocking and Spying Beans

- 테스트를 실행할 때 가끔 애플리케이션 컨텍스트에 있는 bean들을 Mocking할 필요가 있다.
- 예를 들어, 개발하는 중간에 리모트 서비스를 하는 가짜를 만들 필요가 있을 것이다.
- Mocking은 또한 진짜 환경에선 실험해 보기 어려운 실패 상황을 시뮬레이션 할 때 유용할 수 있다.
- Spring Boot는 ```@MockBean```이라는 어노테이션을 제공하는데 이것을 사용해서
Mockito의 mock을 bean 대신 ```ApplicationContext```에 끼워 넣을 수 있다.
- 이 어노테이션을 사용해서 새로운 빈을 만들거나 기존의 빈을 교체할 수 있다.
- 이 어노테이션은 테스트 클래스 또는 테스트, ```@Configuration``` 클래스나 field에 직접 사용할 수 있다.
- field에서 사용 할 때는 생성된 mock이 주입이 된다.
- Mock bean은 각각의 테스트마다 자동으로 reset 된다.
- 만약 테스트가 Spring Boot에서 제공하는 테스트 어노테이션(```@SpringBootTest``` 같은)을 사용하고 있다면
이 기능을 자동으로 쓸 수 있겠끔 되어있다.
- 이 기능을 다르게 사용하고 싶으면, 리스너를 명시적으로 다음과 같이 등록해야 된다.
    - @TestExecutionListeners(MockitoTestExecutionListener.class)
 
- 추가적으로 ```@SpyBean```을 쓸 수 있다. 기존 bean을 Mockito ```spy```로 만들어 준다.
- Spring의 테스트 프레임 워크는 테스트 사이에 애플리케이션 컨텍스트를 캐싱하고
동일한 설정을 공유하는 테스트의 컨텍스트를 다시 사용한다.
- ```@MockBean```이나 ```@SpyBean```을 사용하면 캐쉬 키에 영향을 미치기 때문에 컨텍스트 수가 증가 할 가능성이 높다.
- Mock은 구현이 아무것도 안되있기 때문에 기본 값(empty String)을 준다.
- Mock bean은 객체를 새로 만들어서 등록하지만 Spy bean은 기존의 객체를 wrapping해서 사용한다.
    - mocking 하는 메소드만 원하는대로 바뀌고 mocking하지 않는 다른 메소드들은 그대로 동작한다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/yJ_2eHBQW40
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-mocking-beans
