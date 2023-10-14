#papercut 

JVM 환경에서 사용가능한 벤치마크 도구.
> 2023.10.12 기준 JDK11 이상에서는 JMH가 잘 동작하지 않는 이슈 존재.
### plugin
```bash
// gradle 버전에 맡는 버전 선택
plugins {
	id "me.champeau.gradle.jmh" version "0.5.3"
}

// jmh 설정 적용
jmh {
	threads = 1
	fork = 1
	warmupIterations = 1
	iterations = 1
}
```

벤치마킹하고 싶은 코드는 `src/jmh/kotlin/패키지`내에 넣는다.

```kotlin

@State(Scope.Benchmark)
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.MICROSECONDS)
open class SequenceTest { // 테스트 대상은 확장 가능해야하므로 open
	private val fruits = mutableListOf<Fruit>()

	@Setup
	fun init() {
		(1..2_000_000).forEach { _ -> fruits.add(Fruit.random()) }
	}
	
	@Benchmark
	fun kotlinSequence() {
		val avg = fruits.asSequence()
			.filter { it.name == "사과" }
			.map { it.price }
			.take(10_000)
			.average()
	}
	
	  
	
	@Benchmark
	fun kotlinIterator() {
		val avg = fruits
			.filter { it.name == "사과" }
			.map { it.price }
			.take(10_000)
			.average()
	}
}
```

`@State`: 밴치마크에 사용되는 매개변수의 상태를 지정한다.
- `Scope.Benchmark`: 
	클래스내 인스턴스(`fruits`)들이 스레드에서 같은 값을 공유한다.

`@BenchmarkMode`: 벤치마크 방식을 결정.
- `Mode.AverageTime`:
	각 함수를 몇 번 실행하여 **평균 소요 시간**을 측정한다.

`@OutputTimeUnit`: 밴치마크 결과를 어떤 단위로 표기할지 결정.
- `TimeUnit.MICROSECONDS`:
	결과를 micro second(us) 단위로 지정.

`@Setup`: 벤치마크 수행 전에 호출되는 메서드를 지정. (`@BeforeEach` 느낌)

`@Benchmark`: 벤치마크 대상 함수 지정. (`@Test` 느낌)
### JMH 실행

```bash
./gradlew jmh
```

JMH 실행 결과는 `build/reports/jmh`에 `results.txt`로 생성된다.