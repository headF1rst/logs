#papercut 

```kotlin
fun main() {
	val fruits = listOf(
		MyFruit("사과", 1000L)
		MyFruit("바나나", 3000L)
	)

	val avg = fruits
		.filter { it.name == "사과" } // List<MyFruit>
		.map { it.price } // List<Long>
		.take(10_000)
		.average()
}

data class MyFruit(
	val name: String,
	val price: Long, // 1,000원 ~ 20,000원
)
```

`Iterable`을 사용하면 연산의 각 단계마다 중간 Collection이 임시로 생성된다. 대용량의 데이터를 처리하는 경우에는 중간 리스트를 만드는 과정이 비용이 된다.

`Sequence`

`Sequence`를 사용하기 위해서는 **asSequence**를 붙여주면 된다.
```kotlin
val avg = fruits.asSequence()
	.filter { it.name == "사과" }
	.map { it.price }
	.take(1_0000)
	.average()
```

![[스크린샷 2023-10-09 오후 5.45.23.png]]