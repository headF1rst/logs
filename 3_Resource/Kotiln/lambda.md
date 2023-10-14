`lambda`: 이름없는 함수. 메서드 자체를 변수에 할당하거나 파라미터로 넘길 수 있다. (1급 시민)
```java
// 람다식
변수 -> 변수를 이용한 함수
(변수1, 변수2) -> 변수1과 변수2를 이용한 함수
```

람다는 `메서드 레퍼런스`를 활용할 수도 있다.

`함수 타입`: (파라미터 타입...) -> 반환 타입
파라미터를 받아서 반환 타입을 반환하는 함수.
```kotlin
val isApple: (Fruit) -> Boolean = { fruit: Fruit -> fruit.name == "사과" }
```

자바의 [[3_Resource/Java/lambda|lambda]]에서는 `Predicate` 타입을 받았었는데, 코틀린에서는 함수 자체를 파라미터로 받을 수 있다.
```kotlin
private fun filterFruits(
	fruits: List<Fruit>, filter: (Fruit) -> Boolean): List<Fruit> {
		val results = mutableListOf<Fruit>()
		for (fruit in fruits) {
			if (filter(fruit)) {
				results.add(fruit)
			}
		}
		return results
	}
)
```

함수를 호출하며, **마지막 파라미터가 lambda를 쓸 때**는 소괄호 밖으로 lambda를 뺄 수 있다.
```kotlin
// filterFruits(fruits, { fruit: Fruit -> fruit.name == "사과" })

filterFruits(fruits) { fruit: Fruit -> fruit.name == "사과" }

// 파라미터 타입 추론으로 타입 생략 가능
filterFruits(fruits) { fruit -> fruit.name == "사과" }
```

람다식의 변수가 한개일 경우, 파라미터를 `it (Fruit)`을 통해서 직접 참조할 수 있다.
```kotlin
filterFruits(fruits) { it.name == "사과" }
```

람다는 여러줄 작성할 수 있고, 마지막 줄의 결과가 람다의 반환값이된다.
```kotlin
filterFruits(fruits) { fruit ->
	println("사과만 받자")
	fruit.name == "사과"
}
```

Java의 [[3_Resource/Java/lambda|lambda]]를 쓸 때 final혹은 실질적으로 final인 변수만 사용할 수 있지만, 코틀린은 [[Closure]] 덕분에 이러한 제약사항이 존재하지 않는다.