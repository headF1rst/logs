`CoroutineScope`: 코루틴이 탄생할 수 있는 영역.

- [[코리틴 빌더]] (`launch`,  `async`)는 CoroutineScope의 확장함수이다.
	- launch와 async를 사용하려면 CoroutineScope이 필요하다.
- `runBlocking`을 통해 CoroutineScope을 제공해주고 있었다.
	- root 코루틴을 만들어서 직접 CoroutineScope을 만든다면 runBlocking이 굳이 필요하지 않다.

// launch의 시그니처
```kotlin
public fun CoroutineScope.launch(
	context: CoroutineContext = EmptyCoroutineContext,
	start: CoroutineStart = CoroutineStart.DEFAULT,
	block: suspend CoroutineScope.() -> Unit
): Job
```

// async의 시그니처
```kotlin
public fun <T> CoroutineScope.async(
	context: CoroutineContext = EmptyCoroutineContext,
	start: CoroutineStart = CoroutineStart.DEFAULT,
	block: suspend CoroutineScope.() -> T
): Deffered<T>
```

### CoroutineContext

- CoroutineScope의 주요 역할은 [[CoroutineContext]]라는 **데이터를 보관**하는 것.
```kotlin
public interface CoroutineScope {
	public val coroutineContext: CoroutineContext
}
```

`CoroutineContext`: 코루틴과 관련된 여러 **데이터**를 갖고있다.
(Context안에는 현재 코루틴의 이름, CoroutineExceptionHandler, Job, Coroutine[[Dispatcher]]가 들어있다.)
![[스크린샷 2023-09-21 오후 7.06.34.png]]
부모 코루틴에서 자식 코루틴을 만드는 과정
1. 자식 코루틴은 부모 코루틴과 같은 영역에서 생성된다.
2.  부모 코루틴의 context에서 직접 지정한 정보만 덮어써서 자식 코루틴의 context를 만든다.
3. 부모 - 자식 관계를 설정한다.

이 원리가 [[Structured Concurrency]]를 작동시킬 수 있는 기반이 된다.

클래스 내부에서 독립적인 CoroutineScope을 관리하면 해당 클래스에서 사용하던 코루틴을 한 번에 종료시킬 수 있다.
```kotlin
val asyncLogic = AsyncLogic()
asyncLogic.doSomething() // 비동기 작업 처리

asyncLogic.destroy() // 필요가 없어지면 모두 정리
```

