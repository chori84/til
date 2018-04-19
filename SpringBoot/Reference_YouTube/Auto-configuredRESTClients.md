# Auto-configured REST Clients

- ```@RestClientTest``` 어노테이션을 사용해서 REST 클라이언트를 테스트 할 수 있다.
    - 지금까진 서버를 테스트 했는데 이번엔 클라이언트를 테스트한다.
- 기본적으로 Jackson, GSON, Jsonb를 자동 설정해주고, ```RestTemplateBuilder```를 설정해준다. 그리고 ```MockRestServiceServer```도 추가해준다.
- 더 테스트하고 싶은 bean들은 ```@RestClientTest```의 ```value```나 ```components``` 애트리뷰트에 설정해야 한다.

```java
@RunWith(SpringRunner.class)
@RestClientTest(RemoteVehicleDetailsService.class)
public class ExampleRestClientTest {

	@Autowired
	private RemoteVehicleDetailsService service;

	@Autowired
	private MockRestServiceServer server;

	@Test
	public void getVehicleDetailsWhenResultIsSuccessShouldReturnDetails()
			throws Exception {
		this.server.expect(requestTo("/greet/details"))
				.andRespond(withSuccess("hello", MediaType.TEXT_PLAIN));
		String greeting = this.service.callRestService();
		assertThat(greeting).isEqualTo("hello");
	}

}
```
- 동작하는 원리를 추측해 본다.
- ```@RestClientTest```가 ```RestTemplateBuilder```를 만들어 준다.
- SampleServiceImpl에서 RestTemplateBuilder를 주입 받는다.
    - RestTemplateBuilder를 bean에서 주입
    - 생성자가 하나 있고 레퍼런스가 있으면 레퍼런스에 해당하는 것을 bean에서 주입해준다.
    - 테스트를 만들 때 ```@RestClientTest```가 있으면 ```RestTemplateBuilder```를 생성해 주고,
    생성된 ```RestTemplateBuilder```가 SampleServiceImpl의 생성자에 주입된다.
    - 테스트에서 생성된 ```RestTemplateBuilder```는 ```RestTemplate```에서 나가는모든 요청을
    ```MockRestServiceServer``` 보내서 처리하게 한다. (마치 proxy 처럼)
    - 그래서 this.server.expect(requestTo("/greet/details")).andRespond(withSuccess("hello", MediaType.TEXT_PLAIN));
    에서 mock up 된대로 실행된다.
    - 실제 서버가 사용하는 서비스가 떠있지 않더라도 충분히 테스트를 작성할 수 있다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/Tb0guL8hURs
https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-rest-client