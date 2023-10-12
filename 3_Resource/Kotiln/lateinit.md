#papercut 

코틀린은 [[주 생성자]]에 의해 **인스턴스화를 함과 동시에 프로퍼티에 값이 할당된다.**

```kotlin
fun main() {
	val person = Person("고산하") // 인스턴스 시점에...
}

class Person(
	private val name: String, // 프로퍼티에 값이 할당된다.
) {
	val isKo: Boolean
		get() = this.name.startsWith("고")

	val maskingName: String
		get() = name[0] + (1 util name.length).joinToString("") { "*" }
}
```

`lateinit`: **인스턴스화 시점**과 **프로퍼티 초기화 시점**을 **분리**할 수 있다.

`lateinit` 키워드를 통해서 초기값을 지정하지 않고도 null이 들어갈 수 없는 변수를 선언할 수 있다.
```kotlin
class Person {
	private lateinit var name: String

	val isKim: Boolean
		get() = this.name.startsWtih("김")

	val maskingName: String
		get() = name[0] + (1 util name.length).joinToString("") { "*" }
}
```

초기값이 지정되지 않았는데 변수를 사용하려 하면 예외가 발생하도록 로직이 내장되어있다.
```kotlin
fun main() {
	val p = Person()
	p.isKim // Error. UninitializedPropertyAccessException
}
```
### lateinit의 원리

`lateinit` 변수는 **컴파일 단계에서** `nullable`변수로 바뀌고, 변수에 접근하려 할 때 null이면 예외가 발생한다.
```java
// 컴파일된 코드
public final class Person {
	public String name;

	@NotNull
	public final String getName() {
		String var10000 = this.name;
		if (var10000 == null) {
			Intrinsics.throwUninitializedPropertyAccessException("name");
		}

		return var10000;
	}

	public final void setName(@NotNull String var1) {
		Intrinsics.
	}
}
```