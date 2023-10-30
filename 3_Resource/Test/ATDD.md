`인수 주도 테스트 개발(Acceptance Test Driven Development)`: 클라이언트가 의뢰했던 소프트웨어를 인수 받을 때, 미리 전달했던 요구사항이 충족되었는지를 확인하는 테스트.

애자일 실천 방법 중 하나로서, 다향한 관점을 가진 팀원들과 협업하기 위해 고안된 프로세스.
### ATDD Cycle

템플릿을 활용해서 무엇을 테스트할 것인지 인수조건을 작성한다.
```text
Feature: 간략한 기능 서술
Background: 각 시나리오 사전 조건
Scenario: 시나리오(예시) 제목
Given: 사전조건
When: 발생해야하는 이벤트
Then: 사후조건

And: 앞선 내용에 추가적인 내용 기술
```

```text
Feature: 주문관리 기능

	Scenario: 주문을 생성한다. (주문등록)
		When: 주문 생성을 요청한다.
		Then: 주문이 생성된다.

	Scenario: 주문을 삭제한다.
		Given: 주문이 등록되어있다.
		When: 주문 삭제를 요청한다.
		Then: 주문이 삭제된다.
```

클라이언트 관점에서의 BlackBox 테스트이다.
포스트맨으로 API 요청하는것과 크게 다르지 않지만, 응답값을 코드로서 검증할 수 있다는 차이점이 있다.

인수 테스트는 [[rest-assured]] 테스트 도구를 사용해서 작성한다.
### 데이터베이스 초기화

각 테스트마다 데이터를 초기화하여 테스트간 격리성을 보장해줘야한다.
인수 테스트에서는 `@DirtiesContext`와 같이, spring-test에 사용되는 어노테이션을 사용하지 못한다.

때문에 다음과 같이 하나의 클래스가 마칠때마다 Context를 초기화 시키는 방식을 사용한다.
```java
SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ApiTest {
	
	@LocalServerPort
	int port;

	@Autowired
	private DatabaseCleanup databaseCleanup;

	@BeforeEach
	public void setUp() {
		RestAssured.port = port;
		databaseCleanup.execute();
	}
}
```
