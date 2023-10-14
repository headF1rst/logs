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

`Iterable`을 사용하면 **연산의 각 단계마다** 중간 Collection이 임시로 생성된다. 대용량의 데이터를 처리하는 경우에는 중간 리스트를 만드는 과정이 비용이 된다.
### Sequence

대용량 데이터를 처리해야 할 때 `Sequence`를 사용할 수 있다.

`Sequence`를 사용하기 위해서는 **asSequence**를 붙여주면 된다.

```kotlin
val avg = fruits.asSequence() // Sequence 사용
	.filter { it.name == "사과" }
	.map { it.price }
	.take(1_0000)
	.average()
```
### Sequence 동작원리

- 한 원소에 대해 **모든 연산**을 수행하고, 다음 원소로 넘어간다.
    - 각 원소 하나하나 마다 연산을 순차적으로 수행한다.
- 각 연산 단계가 모든 원소에 적용되지 않을 수 있다.
    - filter 연산에서 해당 원소가 `"사과"`가 아니라면 이후 연산은 수행되지 않는다.
    - `1_0000개`가 모이는 순간 이후 원소는 연산을 적용하지 않는다.
- 최종연산 전까지 계산 자체를 미리하지 않는다. (`지연연산`)

![[스크린샷 2023-10-09 오후 5.45.23.png]]

만약 컬랙션의 크기가 크지 않으면 `지연 연산`으로 인한 overhead로 인해서 `Iterable`보다 `Sequence`가 성능이 안좋을 수 있다.

`Sequence`는 **연산 순서에 따라 큰 성능 차이가 발생할 수 있다** 연산 순서가 바뀌면 처리해야하는 양이 바뀔 수 있기 때문.