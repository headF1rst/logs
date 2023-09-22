Job의 Life Cycle
![[스크린샷 2023-09-22 오전 11.02.03.png]]
> 작업이 완료된 경우 바로 `COPLETED`가 되지 않고, 왜 한 단계 거쳐 갈까?

자식 코루틴들이 모두 완료될 때까지 기다릴 수 있고, 자식 코루틴들 중 하나에서 예외가 발생하면 다른 자식 코루틴들에게도 취소 요청을 보내기 때문.

`Structured Concurrency`: 부모 - 자식 관계의 코루틴이 한 몸 처럼 움직이는 것

- 수많은 코루틴이 유실되거나 누수되지 않도록 보장한다.
- 코드 내의 에러가 유실되지 않고 적절히 보고될 수 있도록 보장한다.
```kotlin
fun main(): Unit = runBlocking {
	launch {
		delay(600L)
		printWithThread("A")
	}
	
	launch {
		delay(500L)
		printWithThread("코루틴 실패!")
	}
}

// Exception in thread "main" java.lang.IllegalArgumentException: !
```

두 번째 코루틴에서 발생한 예외가 부모 코루틴(runBlocking)에 취소 신호를 보내고, 이 취소 신호를 받은 부모 코루틴이 첫 번째 코루틴을 취소 시킨다.

- 자식 코루틴에서 예외 발생할 경우, Structured Concurrency에 의해 부모 코루틴이 취소되고, 부모 코루틴의 다른 자식 코루틴들도 취소된다.
- 자식 코루틴에서 예외가 발생하지 않더라도 부모 코루틴이 취소되면, 자식 코루틴들이 취소된다.
- `CancellationException` 은 정상적인 취소로 간주하기 때문에 부모 코루틴에게 전파되지 않고, 부모 코루틴의 다른 자식 코루틴을 취소시키지도 않는다.