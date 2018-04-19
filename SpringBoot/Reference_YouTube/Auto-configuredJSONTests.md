# Auto-configured JSON Tests

- JSON serialization과 deserialization이 제대로 동작하는지 확인하려면, ```@JsonTest``` 어노테이션을 사용하면 된다.
- ```@JsonTest``` 어노테이션을 붙이면 사용 가능한 JSON 매퍼를 자동으로 설정해준다.
    - Jackson의 ```ObjectMapper```, ```@JsonComponent``` bean, Jackson의 ```Module```들
    - ```Gson```
    - ```Jsonb```
- auto-configuration이 해주는 elements 설정이 필요하면, ```@AutoConfigureJsonTesters``` 어노테이션을 붙이면 된다.
- Spring Boot는 JSON이 예상하는대로 생성 됐는지 확인하는 JSONassert나 JsonPath 라이브러리를 사용해서
AssertJ 기반의 헬퍼를 제공한다.
- ```JacksonTester```, ```GsonTester```, ```JsonbTester```, ```BasicJsonTester``` 클래스들
각각 Jackson, Gson, Jsonb, String에 사용된다.
- ```@JsonTest```를 쓸 때, ```@Autowired```로 받아서 쓸 수 있다.
- JSON 헬퍼 클래스는 (new를 사용하여) 직접 단위 클래스에서 쓸 수 있다.
- ```@JsonTest``` 없이 그렇게 하려면 ```@Before``` 안에서 ```initFields```라는 메소드를 호출해서 만든다. 
- ```@JsonTest```로 auto-configuration 되는 목록
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#test-auto-configuration
    - ```org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration```
    - ```org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration```
    - ```org.springframework.boot.test.autoconfigure.json.JsonTestersAutoConfiguration```

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://www.youtube.com/watch?v=GECCfXZ0W6w
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-json-tests
