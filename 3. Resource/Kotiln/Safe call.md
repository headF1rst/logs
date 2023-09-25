`?.`: null이 아니면 실행. null이면 실행하지 않는다. (그대로 null)

```kotlin
val str: String? = null
println(str?.length) // null 출력
```