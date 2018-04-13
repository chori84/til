# Test Scope Dependencies

- ```spring-boot-starter-test``` “Starter”(```test``` ```scope``` 안에서) 다음과 같은 라이브러리를 포함하고 있다.
    - JUnit
    - Spring Test & Spring Boot Test : SpringFramework에서 제공하는 테스트 관련 기능들
        - Mock Object를 만들 수 있다.
        - Reative Web 테스트를 위해서 WebTestClient가 있다.
        - 테스트에서 쓰는 Application Context를 캐싱한다.
        - Dependency Injection, Transaction management 지원
        - JdbcTestUtils 제공
        - WebAppConfiguration : WebApplicationContext를 만들어 준다.
        - DirtiesContext
        - 기타 등등....
    - AsserJ : xxx.(xxx).(xxx).(xxx)... 으로 체이닝 자동 완성을 이용해서 편하게 사용할 수 있다.
    - Hamcrest
        - 전부 static으로 import해야 되서 IDE에서 잘 지원해주지 않으면 사용하기 힘들다.
        - matcher들을 제공 (is, not, equalTo, hasProperty, array, ....)
    - Mockito
        - mock을 만든 후에 verify 한다.(?)
        - when을 이용하여 stubbing(?) 할 수 있다. (A를 하면 B를 해라)
    - JSONassert : JSONObject를 가져오고 (ex. REST 호출 결과), expected를 생성하여 테스트 함.
    - JsonPath
        - Json 객체를 여러 쿼리스트링으로 값을 접근한다.
        - Front에서 특정 element의 value를 추출하는 것과 비슷하다.
- 테스트를 작성할 때 위와 같은 일반적이 라이브러리들을 사용하는 것이 유용하다.
- 위 라이브러리들이 필요로 하는 것에 적합하지 않으면 다른 테스트 dependency를 추가할 수 있다.

## 참고
https://youtu.be/pnkBfsIqdK4
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-test-scope-dependencies
