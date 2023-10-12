#papercut 

`lazy`: 초기화를 get 호출 전으로 지연시킨 변수. 초기화 로직은 변수 선언과 동시에 한 곳에만 위차할 수 있다.

`by` : 변수의 getter를 뒤 객체의 getter로 이어준다. ([[위임 패턴]]을 사용.)

`by lazy` 람다 안에 초기화 로직을 작성한다.

```kotlin
class Person {
	// name의 getter를 Lazy 객체의 getter로 이어준다.
	val name: String by lazy {
		Thread.sleep(2_000L)
		"김수한무"
	}
}
```

`name`의 getter가 최초 호출될 때 한번만 실행되고, Thread-safe한 구조를 갖고있다.


