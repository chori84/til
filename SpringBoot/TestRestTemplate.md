# TestRestTemplate

- Spring Framework 5.0에서 WebFlux integration 테스트와 WebFlux와 MVC의 end-to-end 테스팅 할 때 쓸 수 있도록
```WebTestClient```가 추가됐다.
- ```TestRestTemplate```과 다르게 assertion을 위한 유연한 API를 제공한다.
- ```TestRestTemplate```은 integration 테스트할 때 유리한 Spring의 ```RestTemplate```의 편리한 대체제이다.
- 바닐라 템플릿 또는 Basic HTTP authentication 전송하는 템플릿을 얻을 수 있다.
- 둘다 server-side 에러가 발생하지 않고 test-friendly하게 동작한다.
- 필수는 아니지만 Apache HTTP Client를 쓰는 것을 권장한다.(4.3.2 이상 버전)
- Apache HTTP Client가 classpath에 있으면 ```TestRestTemplate```은 클라이언트를 적절히 설정해서 응답합니다.
- Apache HTTP Client를 사용하면 다음과 같은 추가적인 장점이 있다.
    - Redirect가 수행되지 않는다. (그래서 response location을 확인할 수 있다.)
    - 쿠키가 무시된다. (그래서 템플릿이 stateless 해진다.)
    - ```RestTemplate```과 똑같이 요청을 보내서 응답을 가지고 오는건데 ```RestTemplate```을 개량해서 테스트용으로 만든 것이다.
- 대안으로 ```WebEnvironment.RANDOM_PORT```나 ```WebEnvironment.DEFINED_PORT```를 사용해서
```@SpringBootTest``` 어노테이션을 사용하면 완전히 설정 된 ```TestRestTemplate```를 inject하여 사용 할 수 있다.
- 필요하면 ```RestTemplateBuilder``` bean을 통해서 추가적인 커스터마이징을 제공할 수 있다.
- host와 port를 지정하지 않은 모든 URL은 embedded 서버로 자동으로 연결된다.
```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
public class SampleWebClientTests {

	@Autowired
	private TestRestTemplate template;

	@Test
	public void testRequest() {
		HttpHeaders headers = this.template.getForEntity("/example", String.class)
				.getHeaders();
		assertThat(headers.getLocation()).hasHost("other.example.com");
	}

	@TestConfiguration
	static class Config {

		@Bean
		public RestTemplateBuilder restTemplateBuilder() {
			return new RestTemplateBuilder().setConnectTimeout(1000).setReadTimeout(1000);
		}

	}

}
```
- 호출 주소 앞에 다 host를 지정하지 않는다.
- 다 로컬 테스트 하는 것이다.

## 참고
https://youtu.be/Tb0guL8hURs