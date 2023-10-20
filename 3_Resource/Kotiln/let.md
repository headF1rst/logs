#papercut 

[[Scope Functions]] 중 한 종류. [[3_Resource/Kotiln/lambda|lambda]]를 받아, lambda 결과를 반환한다.

```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R {
	return block(this)
}
```
### 하나 이상의 함수를 call chain 결과로 호출 하는 경우

```kotlin
val strings = listOf("APPLE", "CAR")

// List<Int> 자체를 print한다
strings.map { it.length } // 문자열을 길이로 변환
	.filter { it > 3 } // 변환값이 3초과인 길이들만 필터. List<Int>
	.let(::println) // .let { lengths -> println(lengths) }
```
### non-null 값에 대해서만 code block을 실행시킬 때

```kotlin
val length = str?.let {
	println(it.uppercase())
	it.length
}
```

[[Safe call]]을 통해서 `let`을 호출하고 대문자로 변환한 문자열을 출력한 뒤, 문자열의 길이를 반환한다.

