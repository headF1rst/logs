#papercut 

외부로부터 환경변수를 받아서, `yml` 파일로 주입할 수 있다.

프로퍼티 값에 해당되는 부분에 **${환경변수}** 형식으로 주입받는다.
```yml
security.jwt.token.secret-key: ${secret-key}
```
### 환경변수 설정

인텔리제이를 통해서 환경변수를 `yml` 파일에 주입할 수 있다.

우측 상단의 `Edit Cofigurations / Modify options` 에 `secret-key=주입할 값` 으로 설정한다.

![[스크린샷 2023-11-07 오후 8.55.44.png]]![[스크린샷 2023-11-07 오후 8.55.56.png]]