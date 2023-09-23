`callback 함수`: 함수에 파라미터로 들어가는 함수.
비동기 프로그램을 순차적으로 수행하고 싶을때 사용한다.

코틀린에서 `callback` 함수는 람다식으로 표현된다. 
람다 ([[lambda]])는 **메인 프로그램과 독립적으로 수행될 수 있는** 작은 코드조각을 의미한다.

람다 형식
```kotlin
{ argumentList -> codBody }
```
`argumentList`: 람다식의 매개변수.
`codBody`: 람다식이 수행할 로직.
`->`: 매개변수와 로직을 분리하는 역할.

```kotlin
val sum = { x: Int, y: Int, -> x + y }
```

```kotlin
fun printResult(x: Int, y, Int, sum: (Int, Int) -> Int) {
	val result = sum(x, y)
	println("$x와 $y의 합은 $result 입니다.")
}

printResult(2, 3, sum) // sum이라는 람다식을 callback함수로 전달한다.

// 2와 3의 합은 5입니다.
```

callback 함수는 비동기 이벤트를 처리하기 위해서 사용된다.
예를들어 **비동기**로 원격 서버에서 데이터를 가져와야하는 경우, callback 함수를 사용할 수 있다.
```kotlin
fun loadDataFromServer(callback: (List<String>) -> Unit) {
	// 원격 서버에서 데이터를 불러오는 시뮬레이션
	Thread.sleep(5000)

	// 더미 데이터 반환
	val data = listOf("A", "B", "D")

	// 불러온 데이터와 함께 callback 함수 호출
	callback(data)
}
```

```kotlin
fun main() {
	loadDataFromServer { data ->
		println("Loaded data: $data")
	}

	println("Loading data...")
}

// Loading data...
// (5초후) Loaded data: [A, B, D]
```
`loadDataFromServer` 함수를 호출하면서 람다식을 callback 함수로 전달한다.
람다식은 매개변수로 문자열 리스트를 받고 불러온 데이터를 비동기로 출력한다.