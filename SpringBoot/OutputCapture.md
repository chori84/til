# OutputCapture

- ```OutputCapture```는 JUnit의 ```Rule```인데 ```System.out```과 ```System.err```를 캡쳐 해준다.
- ```Rule```로 캡쳐를 선언하고 assertion에 ```toString()```을 사용할 수 있다.
```java
import org.junit.Rule;
import org.junit.Test;
import org.springframework.boot.test.rule.OutputCapture;

import static org.hamcrest.Matchers.*;
import static org.junit.Assert.*;

public class MyTest {

	@Rule
	public OutputCapture capture = new OutputCapture();

	@Test
	public void testName() throws Exception {
		System.out.println("Hello World!");
		assertThat(capture.toString(), containsString("World"));
	}

}
```
- 현실적으로 System.out.print를 사용하지 않고 ```Logger```를 사용하는데 과연 의미가 있는 것일까
- ```Logger```를 캡쳐하는 ```Rule```이 필요하지 않을까?

## 참고
https://youtu.be/Tb0guL8hURs
https://docs.spring.io/spring-boot/docs/2.0.0.RELEASE/reference/htmlsingle/#boot-features-output-capture-test-utility
