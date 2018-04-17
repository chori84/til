# The “Spring WebFlux Framework”
- Spring WebFlux는 Spring 5.0에서 소개 된 새로운 reactive 웹 프레임워크이다.
- Spring MVC랑은 다르게, Servlet API를 필요로 하지 않는다.
- 완전히 비동기, non-blocking이고, Reactor project를 통해 Reactive Streams 스펙을 구현한다.
- Spring WebFlux는 두가지 형태가 있다. functional한 형태와 어노테이션 기반.
- 어노테이션 기반은 기존 Spring MVC 모델을 사용하는 사람에겐 매우 가까운 모습이다.
```java
@RestController
@RequestMapping("/users")
public class MyRestController {

	@GetMapping("/{user}")
	public Mono<User> getUser(@PathVariable Long user) {
		// ...
	}

	@GetMapping("/{user}/customers")
	public Flux<Customer> getUserCustomers(@PathVariable Long user) {
		// ...
	}

	@DeleteMapping("/{user}")
	public Mono<User> deleteUser(@PathVariable Long user) {
		// ...
	}

}
```
- “WebFlux.fn”(functional한 형태의 WebFlux)는 라우팅 configuration을 실제 요청 handler와 분리를 시킨다.
    - functional 형태는 라우팅이 handler랑 따로간다. 
	- 따라서 handlers는 더이상 controller라고 하지 않고 Component(bean)이다.
	- 응답은 Mono<ServerResponse>, 파라미터는 ServerRequest 이다.
```java
@Configuration
public class RoutingConfiguration {

	@Bean
	public RouterFunction<ServerResponse> monoRouterFunction(UserHandler userHandler) {
		return route(GET("/{user}").and(accept(APPLICATION_JSON)), userHandler::getUser)
				.andRoute(GET("/{user}/customers").and(accept(APPLICATION_JSON)), userHandler::getUserCustomers)
				.andRoute(DELETE("/{user}").and(accept(APPLICATION_JSON)), userHandler::deleteUser);
	}

}

@Component
public class UserHandler {

	public Mono<ServerResponse> getUser(ServerRequest request) {
		// ...
	}

	public Mono<ServerResponse> getUserCustomers(ServerRequest request) {
		// ...
	}

	public Mono<ServerResponse> deleteUser(ServerRequest request) {
		// ...
	}
}
```
- ```RouterFunction```을 여러개 정의 할 수 있다. bean들을 우선 순위에 따라 정렬 할 수 있다.
- ```spring-boot-starter-webflux``` 모듈을 추가하여 사용할 수 있다.
- ```spring-boot-starter-web```과 ```spring-boot-starter-webflux```가 둘다 있으면, Spring Boot는 Spring MVC를 자동으로 설정한다.
- 이렇게 동작하는 이유는, 많은 Spring 개발자들이 ```spring-boot-starter-webflux```를 Spring MVC 어플리케이션에 추가하는 이유가 reactive ```WebClient```를 사용하기 위해서이기 때문이다.
- 하지만 둘다 추가한 상태에서 WebFlux를 사용하고 싶으면 애플리케이션에 ```SpringApplication.setWebApplicationType(WebApplicationType.REACTIVE)```로 설정하면 된다.

- Mono : 0-1 Item, 0개 아니면 한개의 객체를 가지고 있는 Publisher
- Flux : 0-N Item, 여러개의 아이템, 리스트와 같이 여러개의 객체를 가지고 있는 Publisher

- 실제적으로 사용하게 되면 Autowired로 bookRepository가 있다고 가정하고, bookRepository가 findAll을 하면 이 bookRepository가 여러개의 Book이 들어있는 Flux를 return 한다.
- 나는 이 Flux를 return 해도 되고, Flux로 들어온 Book을 변환해서 쓸 수도 있다.

- handler의 시그니쳐는 항상 return은 Mono<ServerResponse> 파라미터는 ServerRequest
- request에 들어 있는 것을 bodyToFlux, bodyToMono로 받을 수 있다.
    - java9에 있는 flow, rxJava에 있는 Observable로는 안준다.
- RouterFunction을 return하는 bean을 만들고, 만든 handler를 Autowired로 받아서 router에 등록해준다.
- handler엔 무조건 Mono만 return 한다. 책이 여러권 넘어오려면? Flux에 담고, ServerResponse의 body에 Mono대신 Flux를 넣으면 된다.
    - 둘다 Publisher 이기 때문에 둘 중 아무거나 넣어도 된다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-webflux

## 참고
https://youtu.be/j6SFTTxGCK4
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-webflux