# Auto-configured Spring REST Docs Tests

- Mock MVC나 REST Assured의 테스트에서 Spring REST Docs를 사용하기 위해서```@AutoConfigureRestDocs``` 어노테이션을 사용할 수 있다.
- 이것은 Spring REST Docs에서 JUnit rule의 필요성을 제거한다.
- ```@AutoConfigureRestDocs```은 기본 아웃풋 디렉토리(tartget/generated-snippets)를 override 할 때 사용할 수도 있다.

## Auto-configured Spring REST Docs Tests with Mock MVC
- ```RestDocsMockMvcConfigurationCustomizer```를 써서 ```@AutoConfigureRestDocs``` 관련 설정들을 바꿀 수 있다.
- 아래와 같이 하면 모든 클래스에 andDo(XXX)를 안해줘도 된다.
```java
@TestConfiguration
static class ResultHandlerConfiguration {

	@Bean
	public RestDocumentationResultHandler restDocumentation() {
		return MockMvcRestDocumentation.document("{method-name}");
	}

}
```

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## Auto-configured Spring REST Docs Tests with REST Assured
- REST Assured로 테스트를 작성할 때도 REST Docs를 사용할 수 있다.
- RequestSpecification이 주요한 클래스인듯?

## 참고
https://youtu.be/Tb0guL8hURs
https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-rest-docs
