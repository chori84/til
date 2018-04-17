# User Configuration and Slicing

- 만약 코드를 sesible한 방법으로 구조를 짰다면, ```@SpringBootApplication``` 클래스를 기본적으로 사용할 수 있다.
- 특정한 기능의 국한된 설정으로 애플리케이션의 main 클래스를 지저분하게 하지 말아야한다.
- Spring Batch를 사용하고 있고 자동 설정에 의존하고 있다고 가정해 보자, Application 클래스 위에 ```@SpringBootApplication```를 달 수 있다.
```java
@SpringBootApplication
@EnableBatchProcessing
public class SampleApplication { ... }
```
- 이 클래스는 테스트의 소스 설정이기 때문에, 모든 slice 테스트들이 실제로 Spring Batch를 시작하려고 한다. 이것은 분명히 원하는 일이 아니다.
    - 특정 영역을 위한 설정 파일들은 분리해야 한다.
    - 한곳에 설정 파일을 몰아 넣으면 혼란을 줄 수 있다.
- 권장되는 방법은 특정 영역을 위한 설정을 애플리케이션과 동일한 레벨의 ```@Configuration``` 클래스로 옮기는 것이다.
```java
@Configuration
@EnableBatchProcessing
public class BatchConfiguration { ... }
```
- 애플리케이션의 복잡도에 따라 ```@Configuration``` 클래스를 하나만 가질 수도 있고 도메인 영역에 따라 여러개를 가질 수도 있다.
- 필요하면 ```@Import```를 사용해서 여러가지 설정 파일들을 읽어 올 수도 있다.
- 또 다른 혼동을 주는 주요한 원인 중에 하나는 classpath 스캐닝이다.
- 추가적인 패키지를 설정하기 위해서 다음과 같이 설정을 했다
```java
@SpringBootApplication
@ComponentScan({ "com.example.app", "org.acme.another" })
public class SampleApplication { ... }
```
- default component scan을 override 한거고, 여러분이 하고 싶은 slice와 상관 없이 둘의 패키지에 모든게 다 읽히게 된다.
- 가령, ```@DataJpaTest```를 붙였으면, 컨트롤러라 다른 서비스 같은 bean들이 등록이 안되고, repository만 등록 되야 하는데
```@ComponenetScan```이 적용되서 다른 bean들도 다 등록이 될 수 있다.

### Spock은 생략
- Spock을 쓰면 테스트도 눈에 잘 들어와서 좀 더 잘 보이고, 에러 메시지도 좋고, 여러모로 장점이 있다.

## 참고
https://youtu.be/Tb0guL8hURs
https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-user-configuration