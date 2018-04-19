# Detecting Test Configuration

- Spring Test 프레임워크에 대해서 알고 있다면, 여러분은 ```@ContextConfiguration(classes=...)```를 써서
스프링의 ```@Configuration```이 달린 클래스를 읽어 들였을 것이다.
- 또는 테스트 안에 ```@Configuration``` 클래스를 만들어서 사용했을 것이다.
- Spring Boot 어플리케이션을 테스트 할 때는 그렇게 할 필요는 없다.
- Spring Boot의 ```@*Test``` 어노테이션이 명시적으로 정의하지 않아도 여러분의 primary configuration을 자동으로 찾아준다.
- 검색 알고리즘은 테스트가 포함된 패키지에서 ```@SpringBootApplication``` 또는 ```@SpringBootConfiguration``` 어노테이션을 가지고 있는
클래스를 찾을 때까지 동작한다.
- 코드를 sensible 방식으로 구조화하면 메인 configuration이 일반적으로 발견된다.
    - root 패키지에 main 메소드를 가진 class를 만들어서 거기에 어노테이션을 붙인다.
    - https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#using-boot-structuring-your-code
- 애플리케이션을 일부분만 테스트하고 싶으면 특정 부분에 대한 설정을 메인 메소드 애플리케이션 클래스에서 빼줘야 한다.
- primary 설정을 커스터마이징하고 싶다면, ```@TestConfiguration```라는 nested된 클래스를 사용할 수 있다.
- 애플리케이션의 primary 설정 대신 사용하게 되는 ```@Configuration```을 단 클래스와 다르게 
```@TestConfiguration```을 단 클래스는 primary 설정에 추가적으로 사용된다.
- Spring의 테스트 프레임워크는 애플리케이션 컨텍스트를 캐싱한다.
- 같은 설정을 사용하는 테스트라면 시간이 오래 걸릴 가능성이있는 컨텍스트를 로드하는 프로세스는 한 번 발생한다.
    - 애플리케이션을 테스트간에 재사용하는 것
    - 똑같은 bean 설정 파일을 쓰는 테스트끼리는 애플리케이션 컨텍스트를 새로 만드는 것이 아니라 재사용해서 쓴다.

## 예제
https://github.com/chori84/spring-boot-sample/tree/master/spring-boot-test

## 참고
https://youtu.be/yJ_2eHBQW40
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-detecting-config
