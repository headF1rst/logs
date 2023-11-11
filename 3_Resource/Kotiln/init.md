#papercut 

`init 블록`: 클래스가 초기화되는 시점에 한 번 호출되는 블록.

`validation` 로직([[require]])이나 적절한 값을 만들어주는 로직에 사용된다.

```kotlin
class Person(
	val name: String,
	val age: Int,
) {
	init {
		// validation check
		require(name.isNotBlank()) { "이름은 필수입니다" }
	}
}
```