# JSP 제한

- embedded servlet container를 사용하여 Spring Boot를 실행 할 때( 사용가능한 archive로 패키징해서), JSP를 지원하는데 몇 가지 제약사항이 있다.
    - Tomcat 사용시, war 패키징을 할 때만 동작한다. 실행 가능한 war 파일이나 standard container에 배포하여 동작할 수 있지만, 실행 가능한 jar 파일에선 Tomcat의 하드코딩된 파일 패턴 때문에 동작하지 않는다.
    - Jetty 사용시, 실행 가능한 war 파일과 standard container에서 배포할 때 사용할 수 있다. 
    - Undertow는 JSP를 지원하지 않는다.
    - 커스텀한 ```error.jsp```가 error handling에 필요한 기본 view를 override하지 않는다. 대신 Custom error pages를 사용해야 한다.

## 참고
https://youtu.be/V-gBDmR6gZk
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-jsp-limitations