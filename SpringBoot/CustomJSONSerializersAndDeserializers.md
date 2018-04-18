# Custom JSON Serializers and Deserializers

- 만약에 Jackson한테 어떤 JSON 데이터를 serialize, deserialize한걸 커스텀하게 넘겨주고 싶다면
Spring Boot는 ```@JsonComponent``` 어노테이션을 제공해서 쉽게 등록할 수 있도록 한다.
```java
import java.io.*;
import com.fasterxml.jackson.core.*;
import com.fasterxml.jackson.databind.*;
import org.springframework.boot.jackson.*;

@JsonComponent
public class Example {

	public static class Serializer extends JsonSerializer<SomeObject> {
		// ...
	}

	public static class Deserializer extends JsonDeserializer<SomeObject> {
		// ...
	}

}
```

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-webflux

## 참고
https://youtu.be/JuQiY0yqwCI
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-json-components
