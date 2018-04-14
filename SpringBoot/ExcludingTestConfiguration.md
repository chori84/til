# Excluding Test Configuration

- 애플리케이션이 컴포넌트 스캔을 사용하고 있다면(```@SpringBootApplication```이나 ```@ComponenetScan```을 사용)
특정 테스트에 대해서만 만든 최상위 설정 클래스가 실수로 모든 곳에서 선택 될 수 있습니다.
- ```@TestConfiguration```은 테스트의 inner 클래스에서 primary 설정을 커스터마시징하는데 사용할 수 있다.
- 최상위 클래스에 배치되면 ```@TestConfiguration```은 ```src/test/java```에있는 클래스를 검사하여 가져 오지 않아야 함을 나타냅니다.
- 그런 다음에 필요한데서 직접 import해서 쓸 수 있다.
```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Import(MyTestsConfiguration.class)
public class MyTests {

	@Test
	public void exampleTest() {
		...
	}

}
```
- ```@ComponentScan```을 직접 사용하면(```@SpringBootApplication```을 사용하는게 아니라) 
```TypeExcludeFilter```를 등록해 주어야 한다.
    - ```@SpringBootApplication```에 달려있는 ```@ComponentScan```을 보면 ```TypeExcludeFilter```가 등록 되있다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/yJ_2eHBQW40
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-excluding-config
