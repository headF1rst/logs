[[CoroutineScope]]
- 새로운 코루틴을 만든다.
- 주어진 함수 블록이 **즉시 호출된다**
- 해당 코루틴이 완전히 종료되어야 반환된다.

`withContext`
- 새로운 코루틴을 만든다.
- 주어진 함수 블록이 즉시 호출된다.
- 해당 코루틴이 완전히 종료되어야 반환된다.
- context에 변화를 줄 수 있다.
	- Dispatcher를 바꿔 사용할 때 활용 가능
```kotlin
fun main(): Unit = runBlocking {
	printWithThread("START")
	printWithThread(calculateResult())
	printWithThread("END")
}

suspend fun calculateResult(): Int = withContext(Dispatchers.Default) {
	val num1 = async {
		delay(1_000L)
		10
	}

	val num2 = async {
		delay(1_000L)
		20
	}

	num1.await() + num2.awiat()
}
// START
// 30
// END
```

`withTimeOut` , `withTimeoutOrNull` 
- 주어진 함수 블록이 시간 내에 완료되어야한다.
- 시간안에 코루틴이 완료되지 않으면...
	- `withTimeOut` : TimeoutCancellationException을 던진다.
	- `withTimeoutOrNull` : null을 반환.
- CoroutineScope과 나머지 동일