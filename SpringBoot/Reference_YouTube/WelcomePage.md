# Welcome Page

- static content location(```/static``` or ```/public``` or ```/resources``` or ```/META-INF/resources```)에서 ```index.html```을 찾는다.
- ```index.html```이 없으면 ```index``` 템플릿(```Thmeleaf```, ```FreeMarker``` 등등)을 찾는다.
- 그 중에 하나가 찾아지면 해당 파일을 welcome page로 사용한다.

## 참고
- https://youtu.be/-jaRc_78b4I
- https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-welcome-page