# Embedded Servlet Container Support

- Spring Boot는 embedded Tomcat, Jetty, Undertow 서버를 지원한다.
- 대부분의 개발자들은 적당한 "Starter"를 추가하면 fully configured instance를 사용할 수 있다.
- 기본적으로 embedded 서버는 ```8080```에서 요청을 listen 한다.
- CentOS에서 Tomcat을 사용하기로 선택했다면 조심해야 한다.
- 기본적으로 temporary 디렉토리가 컴파일된 JSP, 파일 업로드 등등으로 사용된다.
- 애플리케이션이 실행 중에 이 디렉토리는 ```tmpwatch```에 의해서 지워질 수 있고, 문제가 발생한다.
- 이 상황을 피하고 싶다면 ```tmpwatch``` 설정을 ```tomcat.*``` 같은 디렉토리가 지워지지 않도록
또는 ```server.tomcat.basedir``` 프로퍼티를 embedded Tomcat이 다른 위치의 디렉토리를 사용하도록 설정하도록 커스터마이징 해야된다.

## 참고
https://youtu.be/xfPcoV_-uuE