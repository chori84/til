# Template Engines

- 다이나믹하게 HTML content를 생성한다.
- Thymeleaf, FreeMarker, JSP 등 다양한 템플릿 기술을 지원한다.
- 기본 설정을 이용해서 템플릿 엔진을 사용하면, ```src/main/resources/templates``` 에서 템플릿을 찾는다.
```java
@Controller
public class HelloController {
    @GetMapping(“/hello”)
    public String hello(Model model, @RequestParam String name) {
        model.addAttribute(“name”, name);
        return “hello”;
    }
}
```
- Spring MVC
    - return 하는 String은 view의 이름이다.
    - ```ViewResolver```를 이용해서 view를 찾음
        - viewName(String) -> ViewResolver -> View(View)
    - model에 화면에 전달할 데이터를 저장한다.
        - view를 렌더링 할 때 model에 있는 데이터를 view에 넣어준다.
        - addAttribute를 할 때 key 값을 생략하면 value의 타입 이름이 key 값으로 들어간다.
```java
String name = “chori”;
model.addAttribute(name);
// key = string, value = chori
```
- ```ReqeustParam``` 어노테이션을 이용해서 query 파라미터를 받을 수 있다.
- 가능하면 JSP는 피하자. embedded servlet container를 사용할 떄 몇 가지 문제가 있다.
- 어플리케이션을 IDE에서 실행하는 것과 Maven이나 Gradle이나 실행 가능한 jar 파일을 이용해서 실행하는 것은 classpath 순서가 다르다.
    - 이런 문제가 발생하면 IDE의 classpath 순서를 조정해야 한다.
    - 모듈의 클래스 파일들과 리소스들이 다른 jar 파일들이나 dependency 보다 먼저 오도록 순서를 조정해야 한다.
    - 아니면, 모든 classpath의 ```templates``` 디렉토리를 다 찾아 볼 수 있도록 ```classpath*:/templates/```를 추가한다.

## 참고
https://youtu.be/V-gBDmR6gZk
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-template-engines