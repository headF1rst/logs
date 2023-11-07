#papercut 

하나의 어플리케이션은 **다양한 환경에서** 다른 **환경변수값**을 통해 실행될 수 있다. (테스트 환경에서는 H2 DB를 사용하고 운영 환경에서는 PostgresQL을 사용하는 것 처럼.)

하나의 `application.yml` 파일에서 프로퍼별 설정을 관리하고 환경변수를 주입할 수 있다.

```java
@Component
public class ComponentWithSecretKey {

	private final String commonData;
	private final String secretKey;

	public ComponentWithSecretKey(@Value("${common.data}") String commonData,
								  @Value("${security.jwt.token.secret-key}") String key) {
		this.commonData = commonData;
		this.secretKey = secretKey;	  
	}
}
```

위의 스프링 빈은 `yml`로 부터 두 환경변수를 주입받아 사용한다.

```yml
# 경계 없이
spring.profiles.active: test
common.data: '모든_프로파일_공통_사용_데이터'

---
# 현재 프로파일이 prod인 경우, 해당 프로퍼티들을 사용
spring.config.activate.on-profile: prod
security.jwt.token.secret-key: "prod일때 사용되는 키"

---
# 현재 프로파일이 test인 경우, 해당 프로퍼티들을 사용
spring.config.activate.on-profile: test
security.jwt.token.secret-key: "test일때 사용되는 키"
```

**---** : `yml` 파일내 프로파일별 경계를 설정.

- 특정 영역에 **spring.config.activate.on-profile** 프로퍼티 설정하는 경우
	- 해당 프로파일로 설정되었을 때만 해당 영역의 프로퍼티들이 사용된다.
- **spring.config.activate.on-profile** 프로퍼티가 없는 영역의 경우
	- 해당 영역의 프로퍼티가 모든 프로파일들에서 **공통적으로** 사용된다.

**spring.profiles.active**: 디폴트로 사용될 프로파일을 명시.
### 사용할 프로파일 설정

인텔리제이 상단의 `Edit Configurations`/ `Active profiles`에서 활성화시킬 프로파일 선택
