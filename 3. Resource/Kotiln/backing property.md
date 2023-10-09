값을 가져오는 비용이 크고, 해당 변수가 사용되지 않을 수 있다면 초기화 로직을 1회만 실행시키고 싶은 요구사항에 `backing property`를 사용할 수 있다.

`name` 변수가 사용되는 경우에만 `_name`을 초기화하기 때문에 Thread.sleep()은 꼭 필요할 때 1회만 호출된다.
```kotlin
class Person {
	private var _name: String? = null
	val name: String
		get() {
			if (_name == null) {
				Thread.sleep(2_000L)
				this._name = "김수한무"
			}
			return this._name!!
		}
}
```

단, `backing property`가 필요한 변수마다 이러한 코드를 작성해줘야한다는 번거로움이 있다.

[[위임 패턴]], [[by lazy]]를 사용하여 이런 보일러플레이트를 제거할 수 있다.