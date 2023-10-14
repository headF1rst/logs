코틀린은 `measureTimeMillis` 함수를 제공한다.

`measureTimeMillis`는 함수 내부 코드의 실행시간을 반환한다.

```kotlin
fun kotlinIterator() {
	val time = measureTimeMillis {
		val avg = fruits
			.filter { it.name == "사과" }
			.map { it.price }
			.take(10_000)
			.avarage()
	}
	println("소요 시간: ${time}ms")
}
```