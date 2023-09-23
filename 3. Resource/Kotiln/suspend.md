`suspend` 지시어가 붙은 suspending function은 다른 suspending function을 부를 수 있는 능력이 생긴다.

[[코루틴 빌더]]에서 다른 함수를 받을 때, 이 삼수는 suspend 함수로 간주된다. -> suspending lambda

`suspension point` : 코루틴이 **중지 되었다가 재개될 수 있는 지점** (중지 될 수도 있는). suspend 함수도 suspension point이다.

### suspend 함수 활용

함수를 suspend 함수로 변경하면 이 안에서 어떤 비동기 라이브러리 구현체(`Completable Future` 또는 `Reactor`)를 사용할지 **확장성을 열어둘 수 있다.**

```kotlin
fun main(): Unit = runBlocking {
	val result1 = apiCall1()
	val result2 = apiCall2(result1)
	printWithThread(result2)
}

suspend fun apiCall1(): Int {
	return CoroutineScope(Dispatchers.Default).async {
		Thread.sleep(1_000L)
		100
	}.await()
}

suspend fun apiCall2(num: Int): Int {
	return CompletableFuture.supplyAsync { // 다른 비동기 라이브러리 사용
		Thread.sleep(1_000L)
		num * 2
	}.awiat()
}
```

suspend 함수를 인터페이스에서 사용하면, 인터페이스의 구현체 별로 다른 비동기 라이브러리를 사용할 수 있다.
### 코루틴 라이브러리에서 제공하는 suspend 함수

[[CoroutineScope]]
- 새로운 코루틴을 만든다.
- 주어진 함수 블록이 **바로 실행된다.**

