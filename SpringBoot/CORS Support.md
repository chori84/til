# CORS Support

- Cross-origin resource sharing(CORS)는 대부분의 브라우저에서 구현한 W3C에서 스펙이다.
- 애플리케이션의 API(resources)는 같은 도메인에서만 요청이 가능하다.
- Cross Domain은 다른 도메인에서도 요청할 수 있게 해준다.
- 우회하는 방법으로 IFRAME으로 호출하거나 JSONP의 callback 형태로 호출하는데 CORS로 공식적으로 지원하게 됐다.
1. 내 도메인
```Origin: http://www.example.com```
2. 서버에서 ```Access-Control-Allow-Origin``` (ACAO) 설정으로 CORS 허용 도메인 설정
```Access-Control-Allow-Origin: http://www.example.com```
    - cross-origin을 지원하지 않으면 error 페이지로 보내진다.
- 모든 요청을 허용할 경우
```Access-Control-Allow-Origin: *```
- Spring MVC 4.2에서 부터 CORS를 지원한다.
- Spring Boot에선 특별한 설정 없이 ```@CrossOrigin``` 어노테이션을 붙여 controller의 메소드에 CORS 설정을 한다.
- Global CORS 설정은 ```WebMbcConfigurer``` bean의 ```addCorsMappings(Corsregistry)```을 커스터마이즈해서 사용해서 설정할 수 있다.
- CORS를 허용하지 않을 때 에러
    - ```Failed to load http://localhost:8080/hello: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:9000' is therefore not allowed access.```

## 예제 소스
https://github.com/jongno-study/rest-service-cors/tree/chori84/rest-service-cors

## 참고
https://youtu.be/HGUgrrqtK8U
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-cors