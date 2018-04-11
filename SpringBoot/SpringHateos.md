# Spring HATEOAS

- Hypermedia as the Engine of Application State
- 내가 보내는 본문과 관련 있는 링크 정보들을 같이 보낸다.
- title과 주소
- hypermedia를 어플리케이션 상태 엔진으로 사용
- Spring Boot는 Spring HATEOAS의 auto-configuration을 제공해서 대부분의 어플리케이션에서 잘 동작 된다.
- auto-configuration이 ```@EnableHypermediaSupport``` 없어도 잘 동작한다.
- hypermedia application을 만들기 쉽게 여러가지 bean을 제공한다.
    - ```LinkedDiscoverers``` 
    - ```ObjectMapper```
        - ```ObjectMapper```는 spring.jackson.*의 property들에 의해서 이미 셋팅 되어 있다.
        - ```Jackson2ObjectMapperBuilder```를 통해서도 커스터 마이징 할 수 있다.
- ```@EnableHypermediaSupport```를 사용해서도 configuration을 수정 할 수 있지 있다.
    - 커스터 마이징하면 기본적으로 설정되어 있는 ObejctMapper도 커스터 마이징 되지 않는다.
- HATOAS 검색 조건 추가

```xml
<dependency>
    <groupId>org.springframework.boot</group>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```
```java
public class BangsongResource extends ResourceSupport {
    private String title;
    
    public String getTitle() {
        return this.title;
    }
    public void setTitle(String title) {
        this.title = title;    
    }
}

@RestController
public class BangsongController {
	@GetMapping(“/bs/{id}”)
	public BangsongResource getBangsong(@Pathvariable Bangsong bangsong) {
		Bangsong bangsong = new Bangsong();
		bangsong.setId(id);
		
		BangsongResource resource = new BangsongResource();
		resource.setTitle(bangsong.getId() + " 번째 방송 중입니다.");
		
		Link link = linkTo(BangsongController.class).slash("bs").slash(bangsong.getId())
		    .withSelRel();
		Link liskLink = linkTo(BangsongController.class).slash("bs")..withRel("bangsongList");
        resource.add(new Link(link);
        // resource.add(new Link("http://localhost/bs/" + bangsong.getId()));
		
		return resource;
	}
}
```

```http://localhost:8080/bs/1```

```json
{
    "_links": {
        "bangsongList": {
            "href": "http:/localhost:8080/bs"
        },
        "self": {
            "href": "http:/localhost:8080/bs/1"
        }
    },
    "title": "1 번째 방송 중입니다."
}
```

- 다른 링크도 추가할 수 있다.
- client에서 rel(이름) 정보 기반으로 url을 이용할 수 있다. 
- url을 바꾸더라도 client에선 안전하다.

## 참고
https://youtu.be/tfpvEdq0xdU
https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-spring-hateoas