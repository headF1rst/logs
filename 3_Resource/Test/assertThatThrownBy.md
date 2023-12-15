#papercut 

sut의 예외 발생 여부를 검증하기 위한 메서드.

메서드 체이닝을 통해 특정 예외를 확인과 예외 메시지를 검증할 수 있다.

`특정 예외 확인`: `isInstanceOf(Class)`
`예외 메시지 검증`: `hasMessage(메시지값)`

```java
assertThatThrownBy(() -> tcOrderStatusChangeService.checkOrderSubmissionStatus(failureMessage))  
        .isInstanceOf(IllegalArgumentException.class).hasMessage("에러 메세지가 존재하지 않습니다.");

assertThatIllegalArgumentException().isThrownBy(  
        () -> tcOrderStatusChangeService.checkOrderSubmissionStatus(failureMessage))  
        .withMessage("에러 메세지가 존재하지 않습니다.");
```

![[스크린샷 2023-12-14 오후 11.20.04.png]]

https://loopstudy.tistory.com/287