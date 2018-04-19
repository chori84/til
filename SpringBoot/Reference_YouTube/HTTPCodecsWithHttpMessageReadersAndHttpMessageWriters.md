# HTTP Codecs with HttpMessageReaders and HttpMessageWriters

- Spring WebFlux는 HTTP request와 response를 변활하기 위해서
```HttpMessageReader```와 ```HttpMesaageWriter``` 인터페이스를 사용한다.
    - WebMVC에서의 ```HttpMessageConverter```
- 이 둘은 classpath에서 가용할 라이브러리를 찾아서 sensible한 default 값들을 가진
```CodecConfigurer```안에 설정한다.
- Spring Boot는 ```CodecCustomizer```를 이용해서 커스터마이징을 할 수 있다.
- 예를들어 ```spring.jackson.*```의 설정 키는 Jackson 코덱에 적용됩니다.
- ```CodecCustomizer``` 자체를 추가하거나 커스터마이즈 할 수 있다.
```java
import org.springframework.boot.web.codec.CodecCustomizer;

@Configuration
public class MyConfiguration {

	@Bean
	public CodecCustomizer myCodecCustomizer() {
		return codecConfigurer -> {
			// ...
		}
	}

}
```
- Spring의 custom JSON serialize와 deserialize도 활용할 수 있다.

## 참고
https://youtu.be/JuQiY0yqwCI
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-webflux-httpcodecs