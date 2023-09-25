Elvis 연산자 앞의 연산 결과가 `null`이면 뒤의 값을 사용.
```kotlin
val str: String? = null
println(str?.length ?: 0) // 0 출력
```
[[Safe call]] (`?.`)을 사용해서 str이 널이면 전체가 null이 된다.
### Early return에 Elvis 연산자 사용하기

```kotlin
fun calculate(number: Long?): Long {
	number ?: return 0

	// 다음 로직

	// 자바였다면... if (number == null) { return 0; }
}
```