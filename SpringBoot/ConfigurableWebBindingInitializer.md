# ConfigurableWebBindingInitializer
- Spring MVC는 특정한 요청에서 데이터를 바인딩할 때 사용하기 위해 ```WebBindingInitializer```를 사용해서 ```WebDataBinder```를 만든다.
- ```@Bean```을 사용하여 ```ConfigurableWebBIndingInitializer```를 생성하면 Spring Boot가 자동으로 Spring MVC에서 사용한다.

- WebDataBinder에 설정할 수 있는 것
    - PropertyEditor, Converter, Formatter 등

```java
public class Bansong {
	private int id;
	private String name;

	// getter, setter
}

@RestController
public class BangsongController {
	@GetMapping(“/{id}”)
	public Bangsong getBangsong(@Pathvariable Integer id) {
		Bangsong bangsong = new Bangsong();
		bangsong.setId(id);
		return bangsong;
	}
}
```

- http://localhost:8080/1 로 요청하면 문자열 ‘1’이 getBangsong의 파라미터 id에 Integer로 자동으로 바인딩 된다.

```java
@RestController
public class BangsongController {
	@GetMapping(“/{id}”)
	public Bangsong getBangsong(@Pathvariable Bangsong bangsong) {
		Bangsong bangsong = new Bangsong();
		bangsong.setId(id);
		return bangsong;
	}
}
```

- 파라미터를 Bansong으로 받고 싶으면?
    - Bangsong 타입의 bangsong variable이 없다고 에러가 발생한다.
        - @Pathvariable에 값을 넣지 않으면 변수명이 자동으로 들어간다.
        - @Pathvariable Bangsong bangsong = @Pathvariable(“bangsong”) Bangsong bangsong

```java
@RestController
public class BangsongController {
	@GetMapping(“/{id}”)
	public Bangsong getBangsong(@Pathvariable(“id”) Bangsong bangsong) {
		Bangsong bangsong = new Bangsong();
		bangsong.setId(id);
		return bangsong;
	}
}
```

- String 타입을 Bangsong 타입으로 convert할 수 없다고 에러가 발생한다.
- PropertyEditor, Converter, Formatter를 사용한다.
- PropertyEditor
    - JavaBean 규약.
    - Java SDK에 있다.
    - thread safe 하지 않다.
        - PropertyEditor를 Singleton Bean으로 등록해서 사용하면 안된다.
        - 따라서 객체를 다른 객체에 주입 받을 수 없다.
    - 그냥 쓰지말자
    - PropertyEditor가 제공해주는 getValue, setValue 자체가 thread safe 하지 않기 때문에 PropertyEditor를 상속한 자식 클래스도 thread safe 하지 않다.
- Converter를 등록한다.
    - Converter 인터페이스를 구현하는 클래스를 만들어 사용
    
```java
// 어떤 타입으로 바꿀꺼다. String -> Bangsong
public class BangsongConverter implements Converter<String, Bangsong> {
	@Override
	public Bangsong convert(String id) {
		Bangsong bangsong = new Bangsong();
		bangsong.setId(Integer.parseInt(id));
		return bangsong;
	}
}

@Bean
public ConfigurableWebBindingInitializer initializer() {
	ConfigurableWebBindingInitializer initializer = new ConfigurableWebBindingInitializer();
	ConfigurableConversionService conversionService = new FormattingConversionService();
	conversionService.addConverter(new BangsongConverter());
	initializer.setConversionService(conversionService);
	return initializer;
}
```

- Formatter를 등록한다.
    - Spring MVC에 부가적인 기능을 설정하기 위해 WebMvcConfigurer를 구현한다.

```java
@Configuration
public class MyWebConfig implements WebMvcConfigurer {
	@Override
	public void addFormatters(FormatterRegistry registry) {
		registry.addConverter(new BangsongConverter());
	}
}
```

- Converter를 bean으로 등록한다.
    - converter를 bean으로 등록하면 Spring Boot에서 자동으로 conveter로 등록해 주지 않을까?
    
```java
@Component
public class BangsongConverter implements Converter<String, Bangsong> {
	@Override
	public Bangsong convert(String id) {
		Bangsong bangsong = new Bangsong();
		bangsong.setId(Integer.parseInt(id));
		return bangsong;
	}
}
```

## 참고
https://youtu.be/V-gBDmR6gZk
https://youtu.be/tfpvEdq0xdU
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-web-binding-initializer